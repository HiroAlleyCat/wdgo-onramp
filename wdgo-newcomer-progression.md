---
tags: [wdgwars, newcomer, onramp, progression]
created: 2026-06-02
last-verified: 2026-06-02
related:
  - "[[wardriving-hardware-survey]]"
  - "[[wdgo-feeder-spec]]"
  - "[[wdgo-capture-flow]]"
---

# WDGoWars onramp — from WiGLE to advanced

The fastest legitimate path from zero to scoring points on wdgwars.pl. Each level builds on the one before it. Stop wherever the next step doesn't sound fun yet, come back when it does.

> **Visual learner?** Open [[wdgo-capture-flow]] for the same five-step progression as a flow diagram (capture device → firmware → destination). Renders in Obsidian as a canvas; readable as JSON anywhere else.

## Step 0 — What you're actually doing

Wardriving is capturing wireless signals (Wi-Fi APs, Bluetooth devices, ADS-B aircraft, LoRa mesh nodes) while moving around. The hobby existed long before the game.

| Platform | Role |
|---|---|
| [WiGLE.net](https://wigle.net) | The long-running community database. Most veteran wardrivers say "I'm WiGLE'ing." Free hobby project, no ads. |
| [WDGoWars (wdgwars.pl)](https://wdgwars.pl) | A community wardriving game by LOCOSP. Same captures, adds gang play, cell-grid territory, leaderboards, and accepts aircraft + LoRa mesh slots WiGLE doesn't have. |

You can play either, or both. Same hardware feeds both. The progression below points you at WiGLE first because it's the lowest-friction entry, then crosses you over to WDGoWars without losing the WiGLE captures.

## Step 1 — Phone only (free)

The lowest-effort start.

**Android user:**
1. Install [WiGLE Wifi Wardriving](https://wigle.net/tools) from Google Play (the official app, by WiGLE)
2. Sign up at [wigle.net](https://wigle.net) for a free account
3. Open the app, grant Location + Wi-Fi permissions
4. Hit the scan toggle, then take a walk or drive
5. The app logs networks; uploads to WiGLE.net when you're back on Wi-Fi

**iPhone user:** there is no equivalent first-class WiGLE iOS app. Either borrow an Android device, ride along with an Android-running friend, or skip to Step 3 with a dedicated capture device.

**What this teaches:** the pace of capture, what RSSI looks like in practice, what "first to see" feels like when you find a network nobody's submitted before.

**Time to first WiGLE points:** under an hour if you live near anything populated.

## Step 2 — Cross-post the same captures to WDGoWars (free, ~15 min one-time setup)

Same captures from Step 1, second leaderboard.

1. Make an account at [wdgwars.pl](https://wdgwars.pl)
2. Profile → API Keys → generate a 64-char key, save it
3. From WiGLE Android: Share → save the `.wiglecsv.gz` file to your PC
4. Install [wigle-to-wdgwars](https://github.com/HiroAlleyCat/wigle-to-wdgwars):
   - Linux/Mac: `./run.sh --setup`
   - Windows: `run.bat --setup`
   - The wizard prompts for both keys (WiGLE + WDGoWars), validates them, optionally installs a daily timer
5. Drop the file in the watched directory, or run with `--schedule` for hands-off

That's it — one capture, two leaderboards.

**What this teaches:** how API keys work, the WigleWifi-1.6 CSV format, that the same data can feed multiple services.

## Step 3 — Dedicated capture device (small spend)

A handheld or pocket device that captures continuously without draining your phone battery. Two paths: easy (uploads itself) or cheap (you upload from PC).

### 3a. Easiest path: on-device upload

Four firmwares verified to upload directly to WDGoWars from the capture device. All write WigleWifi-1.6 CSV and POST to `/api/upload-csv` with your API key.

| Hardware | Firmware | Form factor | How it uploads |
|---|---|---|---|
| **M5Stack Cardputer + GPS unit** | [LOCOSP Bruce fork](https://github.com/LOCOSP/bruce-firmware-wdgwars) tag [`v1.0-wdgwars`](https://github.com/LOCOSP/bruce-firmware-wdgwars/releases/tag/v1.0-wdgwars) | handheld with screen + keyboard | Set API key in device config (`bruceConfig.wdgwarsApiKey`), device uploads on its own when on Wi-Fi |
| **XIAO ESP32-C5 / S3 / C6 + GPS** OR **T-Dongle C5** | [Piglet](https://github.com/hamspiced/piglet) (148 stars, actively shipping) | small dev board, controlled via web UI | Browser → device IP → paste API key → "Test Key" → "Upload All". "Get your API Key" link points directly at wdgwars.pl/profile. |
| **Hak5 WiFi Pineapple Pager + u-blox 7 USB GPS** | [LOCOSP Pineapple Pager payload](https://github.com/LOCOSP/pineapple_pager_wdgwars) | pocket | "SYNC NOW" upload button after you have a 3D GPS fix |
| **Raspberry Pi + LCD 1.44" + GPIO buttons (Raspyjack rig)** | [Raspyjack `payloads/exfiltration/wdgwars_upload.py`](https://github.com/7h30th3r0n3/Raspyjack/blob/main/payloads/exfiltration/wdgwars_upload.py) | small box | Payload script on the Raspyjack image |

If you're picking ONE for a clean newcomer experience, **Cardputer + Bruce fork** is the most turnkey (built-in keyboard + screen, no external web UI needed). If you like web dashboards over device menus, **Piglet on a XIAO C5** is the most modern. The Pineapple Pager path is best only if you already own a Pager.

### 3b. Cheapest path: capture-only, upload from PC

Buy less, do more steps per upload.

- Bare classic ESP32-WROOM, **or** a Cheap Yellow Display (CYD 2.8" / 2432S028), **or** an M5 StickC Plus
- Add a GPS module — [ESP32Marauder GPS wiki](https://github.com/justcallmekoko/ESP32Marauder/wiki/gps-modification) lists two tested ones (Teyleten Robot ATGM336H NEO-6M, DWEII GY-NEO6MV2) with pin-by-pin wiring
- Flash [Marauder v1.12.1](https://github.com/justcallmekoko/ESP32Marauder/releases/tag/v1.12.1) for your specific board
- Capture wardrive sessions, pull the SD card to your PC
- Run wigle-to-wdgwars on the SD dump

> **Critical: Marauder writes WigleWifi-1.4 with only 11 columns** (verified at [`WiFiScan.h:682`](https://github.com/justcallmekoko/ESP32Marauder/blob/master/esp32_marauder/WiFiScan.h#L682) — missing the `Frequency`, `RCOIs`, `MfgrId` that WDGoWars expects from 1.6). wigle-to-wdgwars handles the padding, so this is not a blocker — just don't expect Marauder dumps to be byte-compatible with WiGLE web upload either.

> **Critical: Marauder needs a GPS module attached** or the wardrive dumps are empty (verified in source at [`WiFiScan.cpp:515-551`](https://github.com/justcallmekoko/ESP32Marauder/blob/master/esp32_marauder/WiFiScan.cpp#L515) — the `wardrive_line` is only written when `gps_obj.getGpsModuleStatus()` AND `getFixStatus()` are both true).

If you want a pre-flashed device without doing the GPS mod yourself, the [Apex 5](https://www.cnx-software.com/2026/02/11/esp32-marauder-5g-apex-5-module-for-flipper-zero-combines-esp32-c5-two-sub-ghz-radios-nrf24-and-gps/) is sold on Tindie at $99 (price per CNX Software, 2026-02-11 — re-check before buying) with Marauder + GPS already configured.

**What this teaches:** ESP32 flashing (esptool), GPS modules and antennas, the difference between "device uploads" and "you upload from PC", that not all firmwares produce the same CSV shape.

## Step 4 — Pick up the WDGoWars-only slots (aircraft + LoRa mesh)

WiGLE doesn't track aircraft or LoRa mesh nodes. WDGoWars does. These are points only available on WDGoWars.

| Slot | Tool | What you need |
|---|---|---|
| **Aircraft** (ADS-B) | [Muninn (adsb-to-wdgwars)](https://github.com/HiroAlleyCat/adsb-to-wdgwars) v2.0.10 | RTL-SDR USB dongle (~$30 typical) + 1090 MHz antenna + a small Linux box (Pi works). Run dump1090-fa or readsb to decode; Muninn watches the output directory and uploads. Stationary — mount the antenna where it has sky view. |
| **MeshCore LoRa** | [Heimdall (meshcore-to-wdgwars)](https://github.com/HiroAlleyCat/meshcore-to-wdgwars) v0.2.2 | LoRa node (Heltec / TTGO / similar). Export MeshMapper CSV. Heimdall uploads to the `meshcore_nodes` slot. Pocket-portable. |

Both feeders use the signed-JSON path (HMAC envelope via gungnir), not the multipart CSV path. Same API key as your other uploads.

**What this teaches:** that "wardriving" is broader than Wi-Fi — radio observation more generally. Aircraft is a stationary capture game; MeshCore is mobile. They reward different play styles.

## Step 5 — Lab-scale always-on capture

If you got here, you're not asking how to start. You're asking how to scale.

- Raspberry Pi 4 + Kismet with a monitor-mode USB Wi-Fi adapter for best capture quality
- A fleet of ESP32s on different channels and bands (mix of Bruce, Piglet, Marauder)
- Stationary ADS-B with an outdoor antenna for hundreds of nautical miles of range
- Multiple WDGoWars API keys for split-driver attribution (one key = one driver — same key on two devices counts as one driver with two feeders)
- Alternative firmwares to test: [Evil-M5Project](https://github.com/7h30th3r0n3/Evil-M5Project), [Bruce upstream](https://github.com/BruceDevices/firmware), [Ghost_ESP](https://github.com/Spooks4576/Ghost_ESP)
- Custom feeders for sources nobody's built yet (see [[wardriving-hardware-survey]] §9 for the gap list)

For the full chip × firmware support matrix and the complete community tools catalog, jump to [[wardriving-hardware-survey]].

## Common newcomer pitfalls

| Pitfall | What's actually going on |
|---|---|
| Bought a bare ESP32-C3 and can't find good firmware | Marauder has no C3 binary. Bruce has no C3 port. GhostESP has a C3 binary but the output lacks BSSID on some commands. C3 is poorly served by stock firmware — avoid unless you want to write your own. |
| Bought Marauder without a GPS module | Wardrive dumps will be empty. GPS is mandatory, not optional. |
| Same API key on multiple devices | All captures attribute to one driver. Need separate keys for split-driver attribution. |
| Phone app reports "wrong password" | Could actually be Cloudflare returning 429 on a cold IP, not an auth failure. Try again after a minute. |
| Upload returned 429 in the middle of a batch | Stop the whole batch. Continuing makes the cooldown deeper. Pin gungnir >= v0.1.2 (uses `/endpoint/*` URLs which bypass CF L7) or hit `/endpoint/upload-csv` directly in your client. |

## Where to go next

- [[wardriving-hardware-survey]] — the reference doc behind this onramp. Full firmware × chip matrix, every community tool I could verify, decision tree.
- [[wdgo-feeder-spec]] — implementation spec for writing your own feeder in any language. Includes a verified golden test vector.
- [WDGoWars portal](https://wdgwars.pl) and [API help](https://wdgwars.pl/help/) — the source of truth for game mechanics and the API surface.

---

This progression is sequenced for points-per-effort, not skill-per-effort. A determined newcomer can skip directly to Step 3 if they're hardware-comfortable and don't want the Android phase. Don't over-prescribe to yourself.
