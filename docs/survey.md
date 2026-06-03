---
title: Hardware survey
description: Firmware × chip support matrix, community tools catalog, decision tree, API gotchas
---

# Wardriving hardware + firmware survey for WDGoWars

> Citation policy: every concrete claim about a firmware/repo/chip has an inline link to a primary source pulled live on 2026-06-02. Prices are intentionally absent — see §6 for the reasoning. A handful of cells are labeled "field-tested but not citable from public docs" — those are working knowledge from running the feeders, flagged so a future maintainer can re-verify if they doubt the claim.

> **Looking for the newcomer onramp?** See [Newcomer onramp](onramp.md) for the leveled walkthrough from "just got an Android phone" → "lab-scale capture." The companion canvas at [Capture flow diagram](flow.md) visualizes the same paths as a flow diagram.

## 1. How a capture reaches wdgwars.pl

Two server-side upload paths exist. Picking firmware mostly reduces to which path it can use.

| Path | Endpoint | Auth | Format | Notes |
|---|---|---|---|---|
| Bulk WiFi/BLE CSV | `POST /api/upload-csv` | `X-API-Key` header, multipart | WigleWifi-1.6 CSV | Used by Bruce-WDGoWars fork on-device and by the Pineapple Pager payload — confirmed in primary source READMEs cited below. |
| Signed JSON envelope | `POST /api/upload/` | HMAC via `gungnir` | JSON, slot-typed (`aircraft`, `meshcore_nodes`, …) | Used by HiroAlleyCat feeders (Muninn, Heimdall, wigle-to-wdgwars). |

A `/endpoint/*` mirror exists for clients that want to dodge Cloudflare's L7 rate limit on `/api/*` (returns 429 + code 1027 on cold-IP bursts). The shared transport handles this at the library layer — `gungnir` tag [v0.1.2](https://github.com/HiroAlleyCat/gungnir/releases/tag/v0.1.2) flipped the default base URL; pin >= v0.1.2 to inherit. Hand-rolled HTTP clients (Bruce on-device, anything you write yourself) need the URL flip too if they want the bypass.

## 2. Firmwares that upload to WDGoWars directly (on-device, no PC needed)

Four projects verified to upload to WDGoWars from the capture device itself as of 2026-06-02.

| Firmware | Hardware | Upload path | Source citation |
|---|---|---|---|
| **LOCOSP Bruce fork** ([repo](https://github.com/LOCOSP/bruce-firmware-wdgwars), tag [`v1.0-wdgwars`](https://github.com/LOCOSP/bruce-firmware-wdgwars/releases/tag/v1.0-wdgwars) released 2026-04-09) | M5 family + everything upstream Bruce supports — see §4 | `POST /api/upload-csv`, WigleWifi-1.6 | Tree at the tag contains [`src/modules/gps/wdgwars.cpp`](https://github.com/LOCOSP/bruce-firmware-wdgwars/blob/v1.0-wdgwars/src/modules/gps/wdgwars.cpp) (verified via `gh api .../git/trees/v1.0-wdgwars?recursive=1`). |
| **Piglet** ([hamspiced/piglet](https://github.com/hamspiced/piglet), 148 stars, last commit 2026-06-03) | XIAO ESP32-C5, S3, C6; separate T-Dongle C5 variant. C++ Arduino-based firmware. Designed for XIAO + external GPS. | `POST /api/upload-csv` for bulk + `GET /api/me` for key validation. X-API-Key header. Controlled via browser web UI — "Test Key" / "Upload All" buttons. | Verified live via code search: [`Arduino Files/Piglet/WigleUpload.h`](https://github.com/hamspiced/piglet/blob/main/Arduino%20Files/Piglet/WigleUpload.h) declares `wdgwarsTestKey()`, `uploadFileToWdgwars()`, `uploadAllCsvsToWdgwars()`. UI element labeled `<label>WDGoWars API Key</label>` with link to `wdgwars.pl/profile`. Adds C5/S3/C6 to the on-device-uploader chip support set. |
| **LOCOSP Pineapple Pager WDGoWars** ([repo](https://github.com/LOCOSP/pineapple_pager_wdgwars)) | Hak5 WiFi Pineapple Pager + u-blox 7 USB GPS stick | `POST /api/upload-csv`, WigleWifi-1.6 | README quotes verbatim: *"Stores everything as standard WigleWifi-1.6 CSV"* and *"Manual SYNC NOW uploads pending CSVs to POST /api/upload-csv"*. GPS is mandatory: *"3D fix required before scan starts."* |
| **Raspyjack WDGoWars payload** ([repo](https://github.com/7h30th3r0n3/Raspyjack), payload at [`payloads/exfiltration/wdgwars_upload.py`](https://github.com/7h30th3r0n3/Raspyjack/blob/main/payloads/exfiltration/wdgwars_upload.py)) | Raspberry Pi + LCD 1.44" + GPIO buttons (same Raspyjack base unit) | `POST /api/upload-csv` for CSV + `/api/upload` for JSON + `/api/me` for profile checks, X-API-Key header, multipart Wigle CSV | Payload script reads from `/root/Raspyjack/loot/wardriving/sessions/`. Raspyjack repo description doesn't mention WDGoWars — the upload path lives only in this payload file. |

> Piglet was wrongly listed as "not found" in §A of v3 — that was a search-term issue (looked for `wdgwars piglet` instead of `hamspiced piglet`). Reinstated to §2 as the third confirmed on-device uploader after a Marauder-ecosystem deep-dive surfaced [`hamspiced/piglet`](https://github.com/hamspiced/piglet). Raspyjack was similarly reinstated in v4 after the README-only dismissal turned out to miss the payload script.

## 3. Capture firmwares that need a PC-side feeder

These firmwares produce useful capture data but don't upload to WDGoWars directly. Convert via [wigle-to-wdgwars](https://github.com/HiroAlleyCat/wigle-to-wdgwars) (for WiGLE-compatible CSV) or the slot-typed feeders.

| Firmware | Hardware fit (current release) | Output | Feeder needed |
|---|---|---|---|
| **ESP32 Marauder v1.12.1** ([release](https://github.com/justcallmekoko/ESP32Marauder/releases/tag/v1.12.1), repo 11k+ stars, nightlies near-daily) | See §4 chip matrix | WigleWifi-1.4 (11 cols, missing Frequency/RCOIs/MfgrId) to SD — verified at [`WiFiScan.h:682`](https://github.com/justcallmekoko/ESP32Marauder/blob/master/esp32_marauder/WiFiScan.h#L682) hard-coding `WigleWifi-1.4` + 11-field header. wardrive_line construction at WiFi `:4508` and BLE `:547`/`:1132`/`:4646`/`:7346` emits those 11 fields. | wigle-to-wdgwars after SD pull (pads to 1.6). **Marauder requires a GPS module attached** — without it the wardrive dumps are empty. Verified at [`WiFiScan.cpp:515-551`](https://github.com/justcallmekoko/ESP32Marauder/blob/master/esp32_marauder/WiFiScan.cpp#L515) gating `wardrive_line` behind `getGpsModuleStatus()` AND `getFixStatus()`. GPS modification community is well-documented — [official wiki](https://github.com/justcallmekoko/ESP32Marauder/wiki/gps-modification) lists Teyleten Robot ATGM336H NEO-6M + DWEII GY-NEO6MV2 with pin tables. |
| **Bruce upstream v1.15** ([release](https://github.com/BruceDevices/firmware/releases/tag/1.15)) | See §4 chip matrix | WigleWifi CSV to SD | Upstream Bruce does NOT have the WDGoWars upload path — only the LOCOSP fork does. SD pull → wigle-to-wdgwars, OR flash the LOCOSP fork. |
| **GhostESP VA1.4.8** ([release](https://github.com/Spooks4576/Ghost_ESP/releases/tag/VA1.4.8), released 2025-03-31 — no release in 14+ months as of 2026-06-02) | See §4 chip matrix | Varies by command; output shape lacks BSSID on some commands on bare C3 in headless USB-CDC. Verify the output of `list -a` or `capture -beacon` on your specific chip before depending on it. | Wigle-format conversion is uncertain — verify per command before depending on it. |
| **Evil-M5Project** ([README](https://github.com/7h30th3r0n3/Evil-M5Project)) | M5Cardputer (recommended), Core1/Core2/Fire/AWS/CoreS3/CoreS3 SE/AtomS3; beta on CYD2USB/CYD1USB/M5Stick v1.1+v2; slave-mode on ESP32-C3/C5/AtomS3/AtomS3 Lite/WEMOS D1 Mini | "Wigle-compatible CSV files" on Cardputer with GPS (no column spec given in README) | wigle-to-wdgwars. No native WDGoWars upload. |
| **WiGLE Android** | Android phone | `.wiglecsv.gz` via Share | wigle-to-wdgwars |
| **Kismet / hcxdumptool / airodump-ng** | Pi 4 / Linux laptop / desktop + monitor-mode WiFi adapter | WiGLE-compatible CSV | wigle-to-wdgwars |
| **Pwnagotchi** | Pi Zero W | PCAP (WPA handshakes primary); GPS plugin adds locations | Not a one-line WDGoWars story. Conversion path exists but is not direct WiGLE-CSV by default. |

### 3a. HiroAlleyCat feeders + supporting tooling

The HiroAlleyCat WDGoWars family — sibling repos to this one. All Python. The three feeders plus the shared transport library cover the common upload paths.

#### Active feeders (public)

| Tool | Latest | Slot / endpoint | Capture sources accepted | Notable features | Last commit |
|---|---|---|---|---|---|
| [**Muninn** (adsb-to-wdgwars)](https://github.com/HiroAlleyCat/adsb-to-wdgwars) | [v2.0.10](https://github.com/HiroAlleyCat/adsb-to-wdgwars/releases/tag/v2.0.10) (2026-06-01) | `aircraft` slot via signed JSON | AVR, SBS-1, dump1090, readsb, tar1090, VRS, Stratux, Mode-S Beast, NDJSON, Mayhem, GDL-90, CSV | Ships as both CLI (Python) and browser (Pyodide) with the same parser core. `--setup` wizard, `--watch` daemon, scheduler auto-installer (systemd/cron/schtasks), `--preview` parser dry-run. 6 stars. | 2026-06-02 |
| [**Heimdall** (meshcore-to-wdgwars)](https://github.com/HiroAlleyCat/meshcore-to-wdgwars) | [v0.2.2](https://github.com/HiroAlleyCat/meshcore-to-wdgwars/releases/tag/v0.2.2) (2026-06-01) | `meshcore_nodes` slot via signed JSON | MeshMapper MeshCore LoRa exports | CLI + browser Pyodide, same parser-core pattern as Muninn. PEP 668 / Bookworm setup fix in v0.2.2. 1 star. | 2026-06-01 |
| [**wigle-to-wdgwars**](https://github.com/HiroAlleyCat/wigle-to-wdgwars) | [v1.2.0](https://github.com/HiroAlleyCat/wigle-to-wdgwars/releases/tag/v1.2.0) (2026-06-02) | Bulk WiFi/BLE via `POST /api/upload-csv` (multipart) | Any WiGLE-format CSV — WiGLE Android exports, Kismet, hcxdumptool, airodump-ng, Bruce SD pulls, Marauder dumps (after column-pad) | `--setup` wizard with key validator + `--schedule` auto-installer for daily 03:00 dry-run. Cross-platform: systemd + cron + Windows schtasks. As of v1.1.0 routes signed-JSON via gungnir; the `/api/upload-csv` multipart path is the bulk option for CSV. 3 stars. | 2026-06-02 |

#### Shared transport (public)

| Tool | Latest | Purpose |
|---|---|---|
| [**gungnir**](https://github.com/HiroAlleyCat/gungnir) | [v0.1.2](https://github.com/HiroAlleyCat/gungnir/releases/tag/v0.1.2) (2026-05-31) | Python library: HMAC envelope, retry, cooldown, silent-drop detection. The signed-JSON transport for Muninn, Heimdall, and wigle-to-wdgwars. v0.1.2 flipped the default base URL to `/endpoint/*` for Cloudflare L7 bypass — pin >= v0.1.2 to inherit the fix. **Writing a feeder in another language?** See [Feeder spec](spec.md) — the language-agnostic envelope spec derived from gungnir + WatchDogsGo's reference plugin. |

#### Observability

| Tool | Purpose |
|---|---|
| [**wdgwars-api-tester**](https://github.com/HiroAlleyCat/wdgwars-api-tester) | Systematic probe of the WDGoWars HTTP API surface. Stdlib-only Python 3, single file. Detects outages, distinguishes route-not-bound from auth-rejected, fingerprints styled 404 pages. No tagged releases yet — main branch is the surface. |

### 3b. Third-party community feeders

Tools authored by people outside LOCOSP and HiroAlleyCat that POST to wdgwars.pl. Every entry here has been verified by reading its README and/or its actual upload code. Vetting depth varies — see the rightmost column.

| Tool | Maintainer | What it does | Endpoint(s) | Last commit | Vetting |
|---|---|---|---|---|---|
| [phutur1st/intercept-wdgwars](https://github.com/phutur1st/intercept-wdgwars) | phutur1st | Exports live ADS-B from an `intercept` PostgreSQL DB into the `aircraft.json` shape (dump1090-fa / readsb) and uploads to wdgwars. | `POST /api/upload-csv` (per `convert.py` + README — note: CSV path is used for aircraft data here, not the signed-JSON path Muninn uses) | 2026-06-01 — active | README + code path verified. README carries explicit *"I AM NOT RESPONSIBLE FOR SHADOW BANS"* disclaimer. Session files do not auto-prune. |
| [DeflockJoplin/pack](https://github.com/DeflockJoplin/pack) (P.A.C.K. — Passive Acquisition and Capture Kit) | DeflockJoplin | Rust capture suite: 802.11 management frames in monitor mode, BLE via BlueZ, GPS via gpsd. Outputs WiGLE CSV + DeFlock alert CSV. Standout: Flock camera detection method. | wdgwars upload referenced as a working feature; endpoint in [`backend/src/uploader.rs`](https://github.com/DeflockJoplin/pack/blob/master/backend/src/uploader.rs) → `const WDG_UPLOAD_URL: &str = "https://wdgwars.pl/api/upload-csv";` | 2026-05-27 — active, 5 stars | README says *"What is definitely working well today is: Wardriving, Flock Detection, Logging, Uploads, and Home Zone. All other features should be considered in progress and unfinished."* Listed-as-working but I have not driven it end-to-end. |
| [InfIux/Wardriving-Log-Aggregation](https://github.com/InfIux/Wardriving-Log-Aggregation) | InfIux | Aggregates Marauder v8 `.log` files for upload to WDGoWars or WiGLE. Python + tkinter file-picker. | Not specified in README. Tool prepares the file; the user submits via WDGoWars or WiGLE web UI. | 2026-05-18 | Personal-utility tier (0 stars). Verified scope-only — confirm it works for your Marauder version before relying on it. |
| [7h30th3r0n3/Raspyjack-Payloads (wickednull fork)](https://github.com/wickednull/raspyjack-payloads) | wickednull | Custom RaspyJack payloads. Not yet confirmed to include a wdgwars upload payload — listed here so the family is visible. | n/a (verify per-payload) | 2026-05-25 | Adjacent — not personally checked for a wdgwars-specific payload in this fork. |

### 3c. Reference uploader implementation (the game itself)

The WatchDogsGo game source includes a plugin that is, in effect, the canonical reference for how a WDGoWars uploader is supposed to work. Worth reading before writing your own client.

| Tool | Path | What it does | Endpoint | Auth | Why it matters |
|---|---|---|---|---|---|
| **WatchDogsGo wardrive_upload plugin** | [`plugins/wardrive_upload.py`](https://github.com/LOCOSP/WatchDogsGo/blob/main/plugins/wardrive_upload.py) in [LOCOSP/WatchDogsGo](https://github.com/LOCOSP/WatchDogsGo) | Posts WiFi, ADS-B, and MeshCore data — all three slots — from inside the game. Manual "Upload All" / "Upload Latest" triggers. Optional auth-push: game polls `/api/auth/pending/` to act as 2FA. | `POST /api/upload/` (signed JSON path), env `WARDRIVE_API_URL` overrides | `X-API-Key` header + HMAC-SHA256 over `nonce || base64(payload)` | This is the reference for the signed-JSON envelope. If you're writing a new feeder and the gungnir transport doesn't fit, this is the source of truth to match against. |

### 3d. LOCOSP companion firmware

| Firmware | Hardware | What it does |
|---|---|---|
| **WDGWatch** ([repo](https://github.com/LOCOSP/WDGWatch)) | LilyGO T-Watch Ultra (ESP32-S3) | LOCOSP-authored companion firmware ("AKA PipBoy-3000"). Repo description only — feature surface not audited in this pass. |
| **WatchDogsGo (game itself)** ([repo](https://github.com/LOCOSP/WatchDogsGo), 34 stars) | ESP32-C5 + ClockworkPi uConsole (per repo description) | The actual game engine is open source. Pyxel game frontend. Mentioning so readers know the game side is auditable. |

## 4. Chip-fit matrix (Marauder, Bruce, GhostESP)

Built from current-release **asset binary names** (not memory). Every cell traces to an asset in the linked release page.

| Chip | Marauder v1.12.1 | Bruce upstream v1.15 | GhostESP VA1.4.8 |
|---|---|---|---|
| **Classic ESP32 (WROOM)** | yes — `flipper.bin`, `v6.bin`, `v6_1.bin`, `v8.bin`, `kit.bin`, `lddb.bin`, `mini.bin`, `mini_v3.bin`, `old_hardware.bin`, `marauder_v7.bin`, `marauder_dev_board_pro.bin`, `rev_feather.bin` | yes — via m5stack-core4mb/core16mb/core2/cores3/cplus1_1/cplus2/sticks3/dinmeter and many LilyGO variants | yes — `esp32-generic.zip`, `esp32v5_awok.zip`, `ghostboard.zip`, `MarauderV4_FlipperHub.zip`, `MarauderV6_AwokDual.zip` |
| **ESP32-S2** | **no asset** | no asset | yes — `esp32s2-generic.zip` |
| **ESP32-S3** | yes — `multiboardS3.bin` | yes — `Bruce-esp32-s3-devkitc-1.bin` + multiple LilyGO S3 variants (`t-display-s3`, `t-watch-s3`, etc.) | yes — `esp32s3-generic.zip`, `ESP32-S3-Cardputer.zip` |
| **ESP32-C3** | **no asset** | **no asset** | yes — `esp32c3-generic.zip` |
| **ESP32-C5** | yes — `esp32c5devkitc1.bin` | yes — `Bruce-esp32-c5.bin`, `Bruce-esp32-c5-tft.bin`, `Bruce-nm-cyd-c5.bin` | **no asset** |
| **ESP32-C6** | **no asset** | no asset | yes — `esp32c6-generic.zip` |
| **ESP32-H2** | n/a (no WiFi capability) | n/a | n/a |
| **M5 Cardputer** | yes — `m5cardputer.bin`, `m5cardputer_adv.bin` | yes — `Bruce-m5stack-cardputer.bin` | yes — `ESP32-S3-Cardputer.zip` |
| **M5 StickC Plus / Plus2** | yes — `m5stickc_plus.bin`, `m5stickc_plus2.bin` | yes — `Bruce-m5stack-cplus1_1.bin`, `Bruce-m5stack-cplus2.bin` | (not enumerated as a dedicated asset) |
| **CYD 2.8" (2432S028)** | yes — `cyd_2432S028.bin`, `cyd_2432S028_2usb.bin` | yes — `Bruce-CYD-2432S028.bin`, `Bruce-CYD-2USB.bin` | yes — `CYDDualUSB.zip`, `CYDMicroUSB.zip`, `CYD2USB.zip`, multiple others |
| **CYD 2.4" / 3.5"** | `cyd_2432S024_guition.bin`, `cyd_3_5_inch.bin` | several CYD variants enumerated | several CYD variants enumerated |
| **LilyGO T-Watch Ultra / T-Deck / T-Display-S3** | not a Marauder primary target | yes — extensive LilyGO coverage (`t-deck`, `t-deck-pro`, `t-display-s3*`, `t-embed`, `t-hmi`, `t-lora-pager`, `t-watch-s3`) | not enumerated |
| **M5 Tab5** | (no asset, but a community Tab5 wardriver project exists [on Hackster](https://www.hackster.io/Runaque/tab5-wardriver-a-custom-gps-enabled-wardriving-platform-d5948a) using a custom firmware) | not enumerated | not enumerated |

Reading the matrix:

- **Want one wildcard board?** Classic ESP32-WROOM. All three firmwares support it.
- **Best modern target?** ESP32-S3. All three firmwares support it; PSRAM common.
- **C5 buyers:** Marauder and Bruce yes, GhostESP no (as of VA1.4.8). C5 also gets dual-band 2.4 + 5 GHz, but not all firmwares use the 5 GHz radio yet — verify before assuming.
- **C3 buyers:** ONLY GhostESP has a stock binary. Marauder and Bruce both lack one. Bare-C3 wardriving is GhostESP-or-build-your-own. The GhostESP output also lacks BSSID in some command modes on headless USB-CDC C3 boards — verify before depending on it.
- **C6 buyers:** Only GhostESP has a stock binary. C6 is on Marauder/Bruce roadmap-watch territory.

## 5. M5 Tab5 (purchased high-end option)

Verified specs from [Hackster Tab5 Wardriver build](https://www.hackster.io/Runaque/tab5-wardriver-a-custom-gps-enabled-wardriving-platform-d5948a):

- **Chips:** ESP32-P4 (main processor) + ESP32-C6 (WiFi/BT radio)
- **Camera:** built-in 2 MP SC2356
- **GPS:** external — M5Stack GPS/BDS Unit with SMA antenna (AT6668 chipset), GPS+BDS+GLONASS, Grove PORT.A
- **Released:** early 2026
- **Caveat (from source):** *"at the project's time had minimal documentation and undiscovered firmware bugs"*

Price not stated in source. See §6.

## 6. Hardware tiers (without prices — see why below)

The previous draft cited price tiers (~$20 / ~$55 / ~$110 / ~$200) sourced from a single secondhand pass over [ringmast4r's homebrew page](https://ringmast4r.org/html-roladex/homebrew-wardriving). On direct fetch on 2026-06-02 that page returned **"No products found"** — so the tier numbers were not actually grounded in a verifiable primary source. They are removed from this version. Pricing on these boards drifts on Aliexpress and Amazon, vendor by vendor, week by week; quote prices only after pulling them live for the specific store the reader will use.

Tiers reframed as form-factor + skill investment:

| Tier | Build | Form factor | Skill investment |
|---|---|---|---|
| **Phone-only** | Android + WiGLE app → wigle-to-wdgwars | pocket | None. Validate you like the hobby before spending. |
| **ESP32 budget** | Bare classic-ESP32 + GPS module + battery, flash Marauder | breadboard / 3D-printed | Soldering, SD-card pulls, run feeder once. |
| **M5 Cardputer + LOCOSP Bruce fork** | Cardputer + GPS unit, flash [LOCOSP `v1.0-wdgwars`](https://github.com/LOCOSP/bruce-firmware-wdgwars/releases/tag/v1.0-wdgwars) | handheld w/ keyboard + screen | Easiest end-to-end: on-device upload, no PC step. The "just works" target. |
| **CYD 2.8"** | ESP32-2432S028 board + GPS, flash Marauder OR Bruce | small display | Cheap second device. Either firmware works. |
| **M5 Tab5 Wardriver** | M5Stack Tab5 + M5 GPS/BDS Unit, custom firmware | tablet | High-end purchased option. Firmware is custom — Hackster post is the recipe. |
| **Hak5 Pineapple Pager** | Pager + u-blox 7 USB GPS, [LOCOSP Pineapple payload](https://github.com/LOCOSP/pineapple_pager_wdgwars) | pocket | Second confirmed on-device WDGoWars uploader. APP_HANDOFF-compatible per README. |
| **Raspberry Pi + Kismet** | Pi 4 + monitor-mode USB WiFi + GPS + battery | small box / car-mount | Linux familiarity. Best capture quality. Capture → wigle-to-wdgwars. |
| **SDR add-on (ADS-B)** | RTL-SDR + 1090 MHz antenna + Pi → Muninn | small box | Different game — feeds `aircraft` slot. |
| **MeshCore radio** | LoRa node (Heltec / TTGO / similar) → Heimdall | pocket | Feeds `meshcore_nodes` slot. |
| **LilyGO T-Watch Ultra** | T-Watch Ultra (ESP32-S3) + LOCOSP [WDGWatch](https://github.com/LOCOSP/WDGWatch) | wrist | LOCOSP companion firmware exists. Feature surface not audited in this pass. |

## 7. Decision tree for newcomers

```
Do you already own an Android phone?
├─ Yes → start with WiGLE app + wigle-to-wdgwars. Zero spend.
│        Validate you like the hobby before committing hardware.
└─ No (or want a dedicated device) → continue ↓

Do you want zero PC steps after capture?
├─ Yes → M5 Cardputer + GPS, flash LOCOSP Bruce v1.0-wdgwars.
│        OR Hak5 Pineapple Pager + u-blox 7 GPS, install LOCOSP Pineapple payload.
│        Both confirmed on-device uploaders today.
└─ No (you're OK pulling SD cards / running a feeder) → continue ↓

Best capture quality?
└─ Pi 4 + Kismet + monitor-mode USB WiFi + GPS. Capture → wigle-to-wdgwars.

Buying an ESP32 board specifically?
├─ Want broadest firmware support? Classic ESP32-WROOM or ESP32-S3.
├─ Want dual-band 2.4+5 GHz? ESP32-C5 — but check the firmware uses the 5 GHz radio.
├─ Got a bare C3? GhostESP is the only stock binary; Marauder/Bruce don't have one.
└─ ESP32-H2? Wrong chip — no WiFi.
```

## 8. WDGoWars-specific gotchas

From WDGoWars portal docs + field-tested integrations:

1. **One API key = one driver.** Putting the same key on two devices = one driver with two feeders, not two contesting drivers. Split-driver attribution needs two keys.
2. **Cell grid is 0.02° lat × 0.03° lon.** Cron-rebuilds every 5 minutes server-side. Map UI can lag actual state.
3. **Read endpoints return caller-scope only.** You see your own captures via `/api/me/aps?since=`; you don't see who else is in a cell beyond dominant-owner/totals.
4. **Cloudflare L7 shield trips on cold-IP bursts to `/api/*`** — 429 + code 1027 BEFORE reaching origin. Use `/endpoint/*` aliases or pin `gungnir >= v0.1.2`.
5. **Marauder needs GPS attached.** Without GPS module, wardrive dumps are empty (memory-only, not re-verified in this pass — flag before recommending).
6. **Phone app reporting "wrong password"** may be CF 429 on a cold IP, not an auth failure. A single GET to `/api/me` from a never-seen IP can return 429 instantly with the CF rate-limit body. If the app can't tell 429 from 401, it may surface CF rate-limiting as bad-credentials. Try again after a minute before assuming the password is wrong.

## 9. Known WDGoWars feeder gaps

Cross-ref: [Feeder spec](spec.md) §9 has the same list with implementation hints for anyone writing a new feeder.

| Gap | Status |
|---|---|
| Marauder PC feeder | Not yet built. Would read `apps_data/marauder/dumps/wardrive_*.txt` (WiGLE-1.4, 11 cols), pad to WigleWifi-1.6, multipart POST to `/api/upload-csv`. Mostly a `wigle-to-wdgwars` subcommand. |
| Bruce SD-batch replay ergonomics | Already addressable via wigle-to-wdgwars; ergonomics gap only — better UX for "I have 20 dated CSVs, dedup + upload them all." |
| Bare ESP32-C3 capture with BSSID on stock firmware | None of Marauder / Bruce / GhostESP solves this cleanly today. Custom ESPHome probe firmware is the practical fit. |
| LoRa-meshtastic native exports | Heimdall handles MeshMapper exports. Sibling parser would slot in if meshtastic-native shapes differ. |
| Browser-only WiGLE Android handoff | wigle-to-wdgwars CLI exists; a Pyodide build mirroring Muninn's pattern would let users drag a `.wiglecsv.gz` into a webpage. |

## 11. WiGLE — the parent hobby database

WiGLE.net is the long-running community wardriving database that WDGoWars's CSV upload format is derived from. Most of what someone learns capturing for WDGoWars is directly portable to WiGLE, and vice versa. Worth understanding the relationship before deciding which platform to feed (or both).

### 11.1 Relationship to WDGoWars

| Aspect | WiGLE.net | WDGoWars |
|---|---|---|
| Founded | Long-running (pre-2010s) | Newer, game-style overlay |
| Model | Free hobby project, no ads, no user monetization ([WiGLE FAQ](https://wigle.net/faq) — *"This is a free hobby project. We don't run ads or monetize our users."*) | Free hobby game by LOCOSP |
| What it tracks | Wireless networks (WiFi + BT/BLE) | WiFi + BLE + ADS-B aircraft + MeshCore LoRa |
| Submission format | WigleWifi-1.6 CSV ([spec](https://api.wigle.net/csvFormat.html)) | Same WigleWifi-1.6 CSV for WiFi/BLE bulk (`/api/upload-csv`); separate signed-JSON envelope for aircraft + meshcore |
| Capture app | WiGLE Wifi Wardriving (Android, official) | None first-party; relies on Bruce/Pineapple/Raspyjack on-device or PC-side feeders |
| API access | Free, rate-limited; *"Query limits start low and increase with good behavior"*. Commercial licensing *"suspended at the present time"* | API key per user from `wdgwars.pl/profile`; CF L7 burst limits exist |
| Daily reset | 00:00 US/Pacific | Cell grid rebuilds every 5 min server-side; no daily reset |
| Leaderboard | "WiGLE rank" — cumulative networks observed | Per-cell + gang + global, plus driver-by-API-key attribution |

### 11.2 Practical implications

- **Same capture can feed both.** If you're running WiGLE Wifi Wardriving on Android, the `.wiglecsv.gz` it produces is exactly what [wigle-to-wdgwars](https://github.com/HiroAlleyCat/wigle-to-wdgwars) ingests. One Android session can submit to both platforms.
- **WiGLE has a much larger network database.** Network ID claim ("first to see" credit) is WiGLE-side. WDGoWars doesn't change that.
- **WiGLE has no aircraft / MeshCore slots.** ADS-B (Muninn) and MeshCore (Heimdall) are WDGoWars-only feeders — WiGLE doesn't accept those data types.
- **WiGLE API is read-heavy, WDGoWars API is write-heavy.** WiGLE rate-limits queries; WDGoWars rate-limits the *write* path via CF L7 on `/api/*`.
- **WiGLE's CSV spec is the canonical source for WigleWifi-1.6.** [api.wigle.net/csvFormat.html](https://api.wigle.net/csvFormat.html) — confirmed live 2026-06-02 to define the 14-column header `MAC,SSID,AuthMode,FirstSeen,Channel,Frequency,RSSI,CurrentLatitude,CurrentLongitude,AltitudeMeters,AccuracyMeters,RCOIs,MfgrId,Type` plus a pre-header device-info row.
- **Marauder's dump format is older.** Marauder writes 11-column WigleWifi-1.4 dumps missing `Frequency`, `RCOIs`, and `MfgrId` — verified at [`WiFiScan.h:682`](https://github.com/justcallmekoko/ESP32Marauder/blob/master/esp32_marauder/WiFiScan.h#L682). Any feeder padding Marauder dumps to WigleWifi-1.6 needs to know which columns to fill and with what defaults.

### 11.3 Submitting to WiGLE directly

If a user just wants to feed WiGLE (not WDGoWars), the simplest paths:

1. **Phone-only:** Install WiGLE Wifi Wardriving from Google Play, sign up at wigle.net, the app uploads automatically when WiFi is available.
2. **From a hardware capture rig:** SD-pull the WigleWifi-1.6 CSV from your device (Bruce, etc.) and upload via wigle.net's web upload form.
3. **Programmatic:** WiGLE has a documented HTTP API at api.wigle.net — see their FAQ for query/submission patterns.

For users feeding both: [wigle-to-wdgwars](https://github.com/HiroAlleyCat/wigle-to-wdgwars) handles the WDGoWars side; the same source CSV goes to WiGLE via their own tooling.

### 11.4 Things WiGLE confirms about WigleWifi-1.6 (for spec verification)

Pulled live from [api.wigle.net/csvFormat.html](https://api.wigle.net/csvFormat.html) on 2026-06-02:

- Header row: `MAC,SSID,AuthMode,FirstSeen,Channel,Frequency,RSSI,CurrentLatitude,CurrentLongitude,AltitudeMeters,AccuracyMeters,RCOIs,MfgrId,Type` (14 fields)
- Pre-header device-info row: `appRelease`, `model`, `device`, `brand`, and planetary-coordinate fields
- `Type` field is always `WIFI` for Wi-Fi observations (WiGLE also tracks BT/BLE with different Type values)

## A. Editorial notes — common misconceptions and why they're wrong

Things that look like obvious facts about this ecosystem but turn out to be wrong (or trickier than they look) when you check the source. Kept visible so readers can apply the same scepticism to other claims in the same space.

| Common claim | The actual story |
|---|---|
| "Biscuit / Piglet / Raspyjack / M5MonsterC5 all upload to WDGoWars on-device." | Mixed. Piglet does ([hamspiced/piglet](https://github.com/hamspiced/piglet) with web-UI WDGoWars upload). Raspyjack has a payload script ([7h30th3r0n3/Raspyjack `payloads/exfiltration/wdgwars_upload.py`](https://github.com/7h30th3r0n3/Raspyjack/blob/main/payloads/exfiltration/wdgwars_upload.py)) but the repo description doesn't say so — grep the code, not the README. M5MonsterC5 ([C5Lab/M5MonsterC5-CardputerADV](https://github.com/C5Lab/M5MonsterC5-CardputerADV)) is based on JanOS / Project Zero and doesn't upload to WDGoWars at all. Biscuit doesn't appear to exist as a public repo — name may be a misremembering. |
| "WiGLE-1.4 and WiGLE-1.6 differ by three columns (Frequency, RCOIs, MfgrId)." | True, but WiGLE's [own spec page](https://api.wigle.net/csvFormat.html) only documents 1.6 today. The 1.4 column list is citable from [Marauder's `WiFiScan.h:682`](https://github.com/justcallmekoko/ESP32Marauder/blob/master/esp32_marauder/WiFiScan.h#L682) which still hard-codes the `WigleWifi-1.4,…` header + 11 fields. So the delta is real, just not described in any one place by WiGLE. |
| "Marauder works fine without a GPS module — it just won't tag coordinates." | Wrong. [`WiFiScan.cpp:515-551`](https://github.com/justcallmekoko/ESP32Marauder/blob/master/esp32_marauder/WiFiScan.cpp#L515) gates the `wardrive_line` construction behind `gps_obj.getGpsModuleStatus()` AND `getFixStatus()`. No GPS module → no line written. The dumps will be empty. |
| "GhostESP on bare ESP32-C3 works fine for wardriving." | The current release (`VA1.4.8`, 2025-03-31) flashes and boots on C3, but `list -a` output lacks BSSID and channel on some commands when running headless USB-CDC. There's been no release in 14+ months. If you flash a C3 today and care about BSSID for WiGLE-compatible CSV, verify before depending on it. |
| "ringmast4r's homebrew page has price tiers ($20 / $55 / $110 / $200+) for builds." | The [page](https://ringmast4r.org/html-roladex/homebrew-wardriving) returned "No products found" on direct fetch 2026-06-02. Earlier guides repeating those numbers may have hallucinated them from a search snippet. Quote prices only after a fresh per-vendor pull. |
| "The M5 Tab5 wardriver is a ~$200 build." | Tab5 hardware specs (ESP32-P4 + C6 + external M5 AT6668 GPS, released early 2026) are confirmed in the [Hackster build](https://www.hackster.io/Runaque/tab5-wardriver-a-custom-gps-enabled-wardriving-platform-d5948a). Price is not in the source — don't quote it. |

## B. Sources (all fetched live 2026-06-02 unless noted)

Primary:
- [LOCOSP/bruce-firmware-wdgwars](https://github.com/LOCOSP/bruce-firmware-wdgwars), tag [v1.0-wdgwars](https://github.com/LOCOSP/bruce-firmware-wdgwars/releases/tag/v1.0-wdgwars)
- [LOCOSP/pineapple_pager_wdgwars](https://github.com/LOCOSP/pineapple_pager_wdgwars) README
- [LOCOSP/WatchDogsGo](https://github.com/LOCOSP/WatchDogsGo)
- [LOCOSP/WDGWatch](https://github.com/LOCOSP/WDGWatch)
- [justcallmekoko/ESP32Marauder release v1.12.1](https://github.com/justcallmekoko/ESP32Marauder/releases/tag/v1.12.1)
- [BruceDevices/firmware release 1.15](https://github.com/BruceDevices/firmware/releases/tag/1.15)
- [Spooks4576/Ghost_ESP release VA1.4.8](https://github.com/Spooks4576/Ghost_ESP/releases/tag/VA1.4.8)
- [7h30th3r0n3/Evil-M5Project README](https://github.com/7h30th3r0n3/Evil-M5Project)
- [HiroAlleyCat/gungnir tag v0.1.2](https://github.com/HiroAlleyCat/gungnir/releases/tag/v0.1.2)
- [WiGLE CSV format spec (1.6)](https://api.wigle.net/csvFormat.html)
- [Hackster — Tab5 Wardriver build](https://www.hackster.io/Runaque/tab5-wardriver-a-custom-gps-enabled-wardriving-platform-d5948a)

Tertiary (background, not directly cited above):
- [ringmast4r — Wonderful World of Wardriving (substack)](https://ringmast4r.substack.com/p/the-wonderful-world-of-wardriving) — general hobby overview
- [agucova/awesome-esp](https://github.com/agucova/awesome-esp) — broader ESP curation

WDGoWars portal: [wdgwars.pl](https://wdgwars.pl) / [API help](https://wdgwars.pl/help/)

HiroAlleyCat feeders (this repo's siblings):
- [adsb-to-wdgwars (Muninn)](https://github.com/HiroAlleyCat/adsb-to-wdgwars)
- [meshcore-to-wdgwars (Heimdall)](https://github.com/HiroAlleyCat/meshcore-to-wdgwars)
- [wigle-to-wdgwars](https://github.com/HiroAlleyCat/wigle-to-wdgwars)
- [gungnir](https://github.com/HiroAlleyCat/gungnir)
- [wdgwars-api-tester](https://github.com/HiroAlleyCat/wdgwars-api-tester)

Third-party community tools verified during this pass:
- [phutur1st/intercept-wdgwars](https://github.com/phutur1st/intercept-wdgwars) — intercept PostgreSQL → wdgwars
- [DeflockJoplin/pack](https://github.com/DeflockJoplin/pack) — Rust P.A.C.K. with Flock detection + wdgwars upload
- [InfIux/Wardriving-Log-Aggregation](https://github.com/InfIux/Wardriving-Log-Aggregation) — Marauder v8 log aggregator
- [7h30th3r0n3/Raspyjack `payloads/exfiltration/wdgwars_upload.py`](https://github.com/7h30th3r0n3/Raspyjack/blob/main/payloads/exfiltration/wdgwars_upload.py)
- [LOCOSP/WatchDogsGo `plugins/wardrive_upload.py`](https://github.com/LOCOSP/WatchDogsGo/blob/main/plugins/wardrive_upload.py) — reference HMAC implementation

