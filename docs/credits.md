---
title: Credits
description: The maintainers, projects, and vendors that make the WDGWars ecosystem possible
brand: CREDITS
tagline: / maintainers <span>·</span> projects <span>·</span> vendors <span>·</span> communities /
---

This guide stands on a lot of community work. Acknowledging it here, organized by what each project contributes to the WDGWars-or-WiGLE pipeline.

> **Want to be listed?** Run a wardriving Discord, ship a relevant GitHub project, sell wardriving hardware, or cover WDGWars / wardriving on YouTube? Open an issue or PR at [github.com/HiroAlleyCat/wdgo-onramp](https://github.com/HiroAlleyCat/wdgo-onramp), or ask HiroAlleyCat (or any active contributor) on the WDGWars Discord. Bar to be added: one primary source we can verify (your README, channel About page, or storefront).

## The game

**LOCOSP** — created [WDGWars (wdgwars.pl)](https://wdgwars.pl) and maintains the firmware family that uploads to it. The whole reason this repo exists. Repos:

| Repo | What it is |
|---|---|
| [WatchDogsGo](https://github.com/LOCOSP/WatchDogsGo) | The game source itself (Pyxel frontend for ESP32-C5 + ClockworkPi uConsole). The `plugins/wardrive_upload.py` is the reference HMAC implementation. |
| [bruce-firmware-wdgwars](https://github.com/LOCOSP/bruce-firmware-wdgwars) | Bruce fork with on-device WDGWars upload (`src/modules/gps/wdgwars.cpp`). Tag `v1.0-wdgwars`. |
| [pineapple_pager_wdgwars](https://github.com/LOCOSP/pineapple_pager_wdgwars) | Hak5 WiFi Pineapple Pager payload — WigleWifi-1.6 CSV, GPS required, `/api/upload-csv`. |
| [WDGWatch](https://github.com/LOCOSP/WDGWatch) | LilyGO T-Watch Ultra companion firmware. |

**FusedStamen** — maintains a [uConsole-focused fork of WatchDogsGo](https://github.com/FusedStamen/WatchDogsGo) that tracks upstream and hardens the game for the ClockworkPi uConsole (CM4): GPS PTY + bridge baud config, Bluetooth adapter pinning via udev, a packet sniffer and airodump-ng handshake-capture path driven from an AWUS036ACM, a XIAO auto-detect launcher, and bridge-stability fixes (scan rate limiting, cache fallback, upload-timeout handling). Same author as the [antenna-database](https://github.com/FusedStamen/antenna-database) credited under General-hobby documentation.

## Capture firmwares

**justcallmekoko (Mark Spencer)** — creator and maintainer of the dominant ESP32 wardriving firmware family. The KokosStripClub Discord + the near-daily nightly releases keep the Marauder community a moving target in a healthy way.

| Repo | Role |
|---|---|
| [ESP32Marauder](https://github.com/justcallmekoko/ESP32Marauder) | The primary ESP32 wardriving firmware. 11.6k+ stars. |
| [ESP32DualBandWardriver](https://github.com/justcallmekoko/ESP32DualBandWardriver) | C5 dual-band wardriver; its README points users at the WDGWars leaderboard alongside WiGLE. |

**pr3y and the Bruce contributors** — [Bruce upstream firmware (BruceDevices/firmware)](https://github.com/BruceDevices/firmware). LOCOSP's WDGWars-flavored Bruce is a fork; the upstream is what gives Bruce its broad M5 + LilyGO + CYD support.

**Spooks4576** — [Ghost_ESP](https://github.com/Spooks4576/Ghost_ESP). Wide chip support (classic + S2 + S3 + C3 + C6). The only stock binary for bare ESP32-C3, with the caveat that some commands lack BSSID on that chip. **This repo is archived** as of the 2026-07-25 check (last push 2025-04-22); see GhostESP Revival below for the maintained continuation.

**7h30th3r0n3** — two heavyweight community projects:

| Repo | Role |
|---|---|
| [Evil-M5Project](https://github.com/7h30th3r0n3/Evil-M5Project) | M5-family pentest suite — Cardputer, AtomS3, Core2/CoreS3/Fire/AWS support. |
| [Raspyjack](https://github.com/7h30th3r0n3/Raspyjack) | Pi-based redteam toolkit. Ships the WDGWars upload payload at `payloads/exfiltration/wdgwars_upload.py`. |

**hamspiced** — [Piglet (`hamspiced/piglet`)](https://github.com/hamspiced/piglet). Modern XIAO ESP32 wardriving firmware (198★ as of 2026-07-25, actively shipping) with a clean web UI for WDGWars uploads. Adds XIAO C5/S3/C6 to the on-device-uploader chip set. The fourth confirmed on-device WDGWars uploader. Piglet hardware is sold via [hamspiced on Tindie](https://www.tindie.com/stores/hamspiced/) and [Midwest Gadgets](https://www.midwestgadgets.org/product-page/piglet).

**codehedge** — [Biscuit (`biscuitshop.us`)](https://biscuitshop.us), docs at the [Biscuit Wiki](https://codehedge.github.io/Biscuit-Wiki/). Commercial, phone-app-controlled WiFi/BLE research device line: dual-ESP32 Biscuit Pro / Ultra (dual-band WiFi 6 + BLE), a single-chip ESP32-C5 Biscuit DIY, and BiscuitNode mesh satellites. Has a GPS wardrive mode with WiGLE upload and a multi-destination upload framework. Feed WDGWars via WiGLE data → wigle-to-wdgwars. **Support status updated 2026-07-25:** LOCOSP's press page now lists Biscuit under THE GEAR as *"community-built portable wardriving devices, natively supported by WDGWars (integration in progress)"* and names Biscuit as a consumer of `GET /api/me` for key validation, so integration work is real but unfinished. The free [DIY Biscuit](https://biscuitshop.us/pages/diy-biscuits) firmware runs on any ESP32-C5 with 8 MB flash + 8 MB PSRAM (T-Dongle C5, XIAO ESP32C5, generic C5 dev boards) via the browser flasher at [flasher.biscuitshop.us](https://flasher.biscuitshop.us).

**Hellz (Sean Clossey)** — [HellzGate C5 (`Hellz0wnzJ00/hellzgate`)](https://github.com/Hellz0wnzJ00/hellzgate), site [hellzgate.com](https://hellzgate.com). An in-development ESP32-C5 multi-node passive survey array (one master + up to nine scanner nodes over an I²C backplane, dual-band Wi-Fi + BLE, GPS). Firmware closed-source; on-device WiGLE / WDGWars upload is on the Phase 3 roadmap. WDGWars Discord mod.

**JesseCHale** — [HaleHound (`JesseCHale/HaleHound-CYD`)](https://github.com/JesseCHale/HaleHound-CYD), web flasher at [flash.halehound.com](https://flash.halehound.com). ESP32-DIV-lineage multi-protocol CYD toolkit (1.4k+★); its wardrive mode writes WiGLE-compatible CSV to SD that feeds wigle-to-wdgwars. Capture-only — no native WDGWars upload. Also does passive Flock Safety ALPR and Raven detection by BLE fingerprint, which overlaps DeflockJoplin/pack. Docs at [segfault.solutions/halehound](https://segfault.solutions/halehound); built units sold at [halehound.com](https://halehound.com/). Upstream is [cifertech/ESP32-DIV](https://github.com/cifertech/ESP32-DIV).


**cifertech** — [ESP32-DIV](https://github.com/cifertech/ESP32-DIV), the ESP32-S3 multi-band handheld HaleHound forked from, plus the earlier [wardriver3000](https://github.com/cifertech/wardriver3000). Site: [cifertech.net](https://cifertech.net/).

**GhostESP Revival** — [GhostESP-Revival/GhostESP](https://github.com/GhostESP-Revival/GhostESP) and [ghostesp.net](https://ghostesp.net). Picked up GhostESP after [Spooks4576/Ghost_ESP](https://github.com/Spooks4576/Ghost_ESP) was archived, and now claims 46 board targets with WiGLE CSV export and split-channel wardriving over a dual-ESP32 bridge. If you are running GhostESP today, this is almost certainly the tree you want.

**0ct0sec** — [M5PORKCHOP](https://github.com/0ct0sec/M5PORKCHOP). Cardputer firmware whose WARHOG mode is GPS wardriving with WiGLE and WPA-SEC hookups, wrapped in an XP-and-trophies layer. Best README in the catalog, for a given value of "best".


**Joseph Hewitt** — [wardriver.uk Rev3](https://github.com/JosephHewitt/wardriver_rev3) and the [wardriver.uk](https://wardriver.uk) wiki. A dual-ESP32 board that does one thing, documented properly, from before the current handheld wave.

**Jabari Lucien (NSM-Barii)** — [flock-back](https://github.com/NSM-Barii/flock-back) (passive ALPR camera detection while wardriving) and [Dooku](https://github.com/NSM-Barii/Dooku), the hardened Pi 5 + Kismet + 4-adapter rig built around it.

**LAB5 / C5Lab (Labolatorium, Wrocław PL)** — hardware + firmware group focused on the ESP32-C5. Their [projectZero firmware](https://github.com/C5Lab/projectZero) (181★ as of 2026-07-25) runs on the Flipper Zero Pager via their LAB ESP32C5 add-on PCB, and on Cardputer ADV / Tab5 via their M5MonsterC5 add-on (193★ for the [M5MonsterC5-CardputerADV repo](https://github.com/C5Lab/M5MonsterC5-CardputerADV)). LOCOSP's press page calls the C5 running projectZero *"the absolute foundation"* of the WDGWars rig. The add-ons expose sub-1 GHz capture on chips that otherwise can't do it. Hardware sold from [Tindie store](https://www.tindie.com/stores/lab/) and [lab5-11 Shopify](https://lab5-11.myshopify.com/); quick-start at [c5lab.github.io/projectZero](https://c5lab.github.io/projectZero/).

## Communities

The Discord servers and forums where the people behind the above firmwares answer questions. Invite links rot; the durable references are the project README files.

| Community | Reference link | What it's for |
|---|---|---|
| WDGWars / LOCOSP | [wdgwars.pl/press](https://wdgwars.pl/press), [LOCOSP on GitHub](https://github.com/LOCOSP) | Game itself, leaderboard, upload pipeline questions. Direct contact via Discord DM to `@locosp`. |
| KokosStripClub (Marauder) | [justcallmekoko on GitHub](https://github.com/justcallmekoko), [server listing](https://discord.com/servers/willstunforfood-776211399918878760) | Marauder firmware, chip-specific gotchas, nightly-build chat. Server name is "WillStunForFood" on the public listing. |
| Bruce | [BruceDevices/firmware README](https://github.com/BruceDevices/firmware) | Upstream Bruce builds across M5 + LilyGO + CYD targets. |
| MeshCore | [meshcore-dev/MeshCore README](https://github.com/meshcore-dev/MeshCore) | MeshCore LoRa firmware and node-discovery questions. |
| RTL-SDR Blog | [r/RTLSDR](https://old.reddit.com/r/RTLSDR/) | Most active community hub. No official Discord. |
| Flipper Zero | [flipperzero.one/discord](https://flipperzero.one/discord) | Adjacent hardware community; relevant for users adding Flipper to a wardriving kit. |
| Hak5 | [hak5.org community links](https://hak5.org/pages/community-links) | Adjacent hardware community; relevant for Pineapple Pager users. |
| LAB5 / C5Lab | [c5lab.github.io/projectZero](https://c5lab.github.io/projectZero/) (the quick-start lists the current Discord invite) | ESP32-C5 add-on hardware and projectZero firmware questions. |
| M5Stack | [m5stack.com](https://m5stack.com) (footer links the official Discord) | Vendor-official; broad scope across Cardputer, Tab5, StickC, and the rest of the M5 line. |
| Cardputer community (unofficial) | [terremoth/awesome-m5stack-cardputer README](https://github.com/terremoth/awesome-m5stack-cardputer); [r/CardPuter](https://www.reddit.com/r/CardPuter) | Cardputer firmware comparisons, apps, mods, peripherals. |

## Third-party feeders

Tools authored outside LOCOSP and HiroAlleyCat that POST to wdgwars.pl.

| Maintainer | Repo | Role |
|---|---|---|
| **phutur1st** | [intercept-wdgwars](https://github.com/phutur1st/intercept-wdgwars) | Exports ADS-B from an `intercept` PostgreSQL database into the aircraft.json shape, uploads to wdgwars. |
| **DeflockJoplin** | [pack (P.A.C.K.)](https://github.com/DeflockJoplin/pack) | Rust passive capture suite — 802.11 + BLE + GPS, WiGLE CSV output, Flock-camera detection, wdgwars upload. |
| **InfIux** | [Wardriving-Log-Aggregation](https://github.com/InfIux/Wardriving-Log-Aggregation) | Aggregates Marauder v8 `.log` files for upload to WDGWars or WiGLE. |

## Parent platform

**[WiGLE](https://wigle.net)** — the long-running, free, ad-free hobby database. WDGWars's bulk-upload format is WigleWifi-1.6 (WiGLE's own spec at [api.wigle.net/csvFormat.html](https://api.wigle.net/csvFormat.html)). The hobby in this corner of the internet would not exist without WiGLE.

The official WiGLE Android app ([Google Play](https://play.google.com/store/apps/details?id=net.wigle.wigleandroid)) is the recommended Tier-1 capture tool in [Newcomer onramp](onramp.md).

## General-hobby documentation

**ringmast4r** — the closest thing the broader wardriving hobby has to a unified one-stop reference.

| Source | What it is |
|---|---|
| [Homebrew Wardriving roladex](https://ringmast4r.org/html-roladex/homebrew-wardriving) | Build comparisons across hardware classes. |
| [The Wonderful World of Wardriving (substack)](https://ringmast4r.substack.com/p/the-wonderful-world-of-wardriving) | Hobby overview and history. |
| [I Mapped 2.98 Million WiFi Networks (substack)](https://ringmast4r.substack.com/p/i-mapped-298-million-wifi-networks) | Personal-scale ops writeup. |

**[agucova/awesome-esp](https://github.com/agucova/awesome-esp)** — broader ESP8266/32 project curation, useful for finding adjacent firmwares.

**FusedStamen** — [antenna-database](https://github.com/FusedStamen/antenna-database). An empirically measured WiFi antenna SWR database for wardriving, characterized on a LiteVNA 64 with a documented methodology and field-tested on a Biscuit Ultra. 130+ antennas across 20+ batches with Good / Marginal / Do_Not_Use verdicts based on worst in-band SWR. The reference for "is this antenna actually resonant in-band, or just marketed that way" — covers WiFi sticks, paddles, MIMO panels, plus LoRa 915 / ADS-B 1090 / GPS L1 / BLE.

**Runaque** — author of the [Tab5 Wardriver build on Hackster](https://www.hackster.io/Runaque/tab5-wardriver-a-custom-gps-enabled-wardriving-platform-d5948a). Recipe for the high-end Tab5 path.

## YouTube and video coverage

Video is how most newcomers see a working rig before they spend money, and wardriving is a hobby where a two-minute clip settles arguments a spec sheet can't. Channels first, then specific videos worth the click, then how to get your own coverage picked up by the game itself.

### Channels

| Creator | Where | Coverage |
|---|---|---|
| **Talking Sasquach** | [YouTube](https://www.youtube.com/@TalkingSasquach), [Odysee mirror](https://odysee.com/@talkingsasquach:1) | The widest device-comparison coverage in this space. Ran a WDGWars shootout covering Marauder, the Pineapple Pager, a Cardputer running Porkchop, HaleHound, Biscuit Pro, and Piglet, rather than reviewing one board in isolation. Also covers Flipper Zero and HackRF. |
| **Valley Tech Solutions** | [YouTube](https://www.youtube.com/@Valleytechsolutions) | WDGWars collabs, wardriving rigs, on-device walkthroughs. |
| **justcallmekoko** | [YouTube](https://www.youtube.com/justcallmekoko), [justcallmekokollc.com](https://www.justcallmekokollc.com) | Marauder firmware demos and hardware tours from the firmware author. |
| **GhostStrats (Spooks4576)** | [YouTube](https://www.youtube.com/channel/UCzSZPWtTRA4G946XRAn2XLQ) | Ghost_ESP firmware walkthroughs across many chips. Note that GhostESP development has since moved to [GhostESP-Revival](https://github.com/GhostESP-Revival/GhostESP). |
| **7h30th3r0n3** | [YouTube](https://www.youtube.com/channel/UCN1sTrFbvdliXTUOsY5DkyA) | Evil-M5Project and Raspyjack demos from their author. |
| **ringmast4r** | [ringmast4r.org](https://ringmast4r.org), [substack](https://ringmast4r.substack.com), [Instagram](https://www.instagram.com/ringmast4r/), [X](https://x.com/Ringmast4r) | Wardriving hobby coverage and personal-scale ops writeups. Video archive on-site rather than on YouTube. |

Channel handles rot slower than Discord invites but faster than repos. Where a creator also authors a firmware, the GitHub profile in the sections above is the durable pointer.

### Specific videos worth the click

Linked by URL rather than by channel, because several of these come from small channels or one-off uploads. Titles are as published.

| Video | Why |
|---|---|
| [I Tested Every Wardriving Device for Watchdog Go Wars](https://odysee.com/@talkingsasquach:1/i-tested-every-wardriving-device-for:f) | Side-by-side of the current device field against the same game. The single most useful thing to watch before buying anything in the shopping list. |
| [WDG Wars Wardriving Like Never Before! (Ft Biscuit Pro)](https://www.youtube.com/watch?v=GGWn1pYOOzE) | WDGWars gameplay with the Biscuit Pro in the loop. |
| [Watch Dogs Go Wars Just Turned Wardriving Into a Real Game!](https://www.youtube.com/watch?v=t9L4_y4XF2s) | Game-side overview: territory, leaderboard, what the map actually looks like in play. |
| [Interview with Hedge, Biscuit Founder: Pro, DIY, Ultra, and the Future of the Biscuit Project](https://www.youtube.com/watch?v=vNkoFyHpOS0) | Where the Biscuit line is going, from the person building it. |
| [Biscuit Pro: The Swiss Army Knife of Wardriving Tools](https://www.youtube.com/watch?v=7lDQLJAP3TM) | Feature tour of the app-driven workflow. |
| [New Halehound CYD Firmware: Complete Walkthrough](https://www.youtube.com/watch?v=25et7KJzb1s) | HaleHound end to end on cheap CYD hardware. |
| [The Great Warhog: war driving with porkchop on the cardputer ADV](https://www.youtube.com/watch?v=0oI8loqHycQ) | Porkchop's WARHOG mode, including the GPS pin settings the ADV needs. |
| [Super Easy DIY Biscuit on LilyGo T-Dongle ESP32-C5 — Flash & Wardrive in Minutes](https://www.youtube.com/watch?v=bb_XThgtQ68) | The free DIY Biscuit path on a T-Dongle C5, start to capture. |

### Getting your own coverage in front of players

LOCOSP auto-features community wardriving videos on the WDGWars map and on the press page. Mechanism, quoted from [wdgwars.pl/press](https://wdgwars.pl/press) (read 2026-07-25):

- Put one of `#wdgwars`, `#WDGWars`, `#WatchDogsGoWars`, or `#WatchDogsGo` in the video **title or description**. Case doesn't matter; the title is better because it's more visible. For shorts, put it in the description too.
- Featured videos appear as a center overlay on the main map (once per user, with a close button) and as a thumbnail grid in the Community videos section of the press page.
- It runs on RSS polling, refreshed roughly every 30 minutes. No approval step, no API keys, no OAuth.
- Only channels an admin has added to the tracker are scanned, so the one manual step is DMing `@locosp` on Discord or GitHub to get your channel added. After that, tagging is all it takes.

That page is also the canonical place to discover new WDGWars-specific coverage as it ships.

## Adjacent open-source tooling

Projects the onramp leans on without listing them as feeders themselves. Each is the reference implementation for its slot. The full catalog of wardriving-capable projects, including everything with no WDGWars uploader at all, is in [Hardware survey](survey.md) §3e, with star counts and last-push dates in §10.

| Project | Repo | Role |
|---|---|---|
| Kismet | [kismetwireless/kismet](https://github.com/kismetwireless/kismet) | The reference Linux Wi-Fi capture stack. Recommended in Tier 5 for monitor-mode quality. Canonical upstream is `git://kismetwireless.net/git/kismet.git`; GitHub is the maintained mirror. |
| MeshCore (upstream) | [meshcore-dev/MeshCore](https://github.com/meshcore-dev/MeshCore) | The MeshCore LoRa firmware itself. Heimdall reads MeshMapper exports produced by devices running this. |
| dump1090-fa | [flightaware/dump1090](https://github.com/flightaware/dump1090) | FlightAware's Mode-S decoder. The most common ADS-B input source Muninn ingests. |
| readsb | [wiedehopf/readsb](https://github.com/wiedehopf/readsb) | The decoder that powers adsb.lol and airplanes.live. Drop-in replacement for dump1090. |
| tar1090 | [wiedehopf/tar1090](https://github.com/wiedehopf/tar1090) | Web map UI for readsb / dump1090. Useful debugging companion when Muninn isn't capturing. |
| Stratux | [cyoung/stratux](https://github.com/cyoung/stratux) | ADS-B receiver firmware for portable EFB use; also a recognised Muninn input format. |
| GDL-90 decode/encode | [etdey/gdl90](https://github.com/etdey/gdl90) | Reference Python library for the GDL-90 frame format used by Stratux. |
| Cardputer launcher | [bmorcelli/Launcher](https://github.com/bmorcelli/Launcher) | Multi-board firmware launcher (Cardputer + ADV, LilyGO, CYD, Marauder targets). ~1.6k stars. |
| Cardputer apps suite | [d4rkmen/M5Apps](https://github.com/d4rkmen/M5Apps) | Cardputer-exclusive app suite. v1.x for IOMatrix and ADV with TCA8418. 75★, v2.5 released April 2026. |
| Cardputer firmware index | [ru84r8/Cardputer-firmware-list](https://github.com/ru84r8/Cardputer-firmware-list) | Curated index of Cardputer firmwares (Bruce, Evil Cardputer, Marauder, emulators, LoRa chat). |
| Cardputer awesome list | [terremoth/awesome-m5stack-cardputer](https://github.com/terremoth/awesome-m5stack-cardputer) | Canonical awesome-list. Primary source for the Cardputer Discord + r/CardPuter links above. |

## HiroAlleyCat feeder family

The sibling repos to this one. Released under MIT, source available, primary author maintained.

| Repo | What it does | Latest |
|---|---|---|
| [adsb-to-wdgwars (Muninn)](https://github.com/Yggdrasil-AI-labs/adsb-to-wdgwars) | ADS-B → wdgwars `aircraft` slot. Accepts AVR, SBS-1, dump1090, readsb, tar1090, VRS, Stratux, Mode-S Beast, NDJSON, Mayhem, GDL-90, CSV. | v2.0.16 |
| [meshcore-to-wdgwars (Heimdall)](https://github.com/Yggdrasil-AI-labs/meshcore-to-wdgwars) | MeshCore LoRa → wdgwars `meshcore_nodes` slot. | v0.4.5 |
| [wigle-to-wdgwars](https://github.com/Yggdrasil-AI-labs/wigle-to-wdgwars) | WigleWifi-1.6 CSV → wdgwars bulk via multipart upload. | v1.6.2 |
| [gungnir](https://github.com/Yggdrasil-AI-labs/gungnir) | Shared HMAC transport library used by the three above. | v0.1.3 |
| [wdgwars-api-tester](https://github.com/Yggdrasil-AI-labs/wdgwars-api-tester) | Systematic probe of the WDGWars HTTP API surface. | v0.13.3 |
| [wdgwars-discord-stats](https://github.com/Yggdrasil-AI-labs/wdgwars-discord-stats) | Build-your-own WDGWars stats display in Discord (live channels, webhook, war-feed) + a consolidated WDGWars API reference. | v1.4.1 |

## Hardware vendors

Vendors mentioned across [Shopping list](shopping.md). Linked here for one-page reference.

| Vendor | What they make |
|---|---|
| [M5Stack](https://m5stack.com) / [shop](https://shop.m5stack.com) | Cardputer, Tab5, GPS units (AT6668), StickC family |
| [Hak5](https://shop.hak5.org) | WiFi Pineapple Pager and the wider Hak5 ecosystem |
| [Seeed Studio](https://www.seeedstudio.com) | XIAO ESP32 dev boards (C5, S3, C6) |
| [LilyGO](https://lilygo.cc) | T-Dongle, T-Beam (LoRa), T-Display, T-Watch Ultra |
| [Heltec](https://heltec.org) | WiFi LoRa 32 (MeshCore-compatible LoRa node) |
| [RAK Wireless](https://store.rakwireless.com) | WisBlock LoRa modules |
| [Waveshare](https://www.waveshare.com) | LCD HATs for Raspberry Pi (Raspyjack rig) |
| [Adafruit](https://www.adafruit.com) | Pis, USB GPS, dongles, breakout boards |
| [NooElec](https://www.nooelec.com) | RTL-SDR dongles (ADS-B) |
| [RTL-SDR Blog](https://www.rtl-sdr.com) | Reference RTL-SDR v4 dongle |
| [Alfa Network](https://www.alfa.com.tw) | Monitor-mode USB Wi-Fi adapters |
| [CanaKit](https://www.canakit.com) | Pi kits |
| [Tindie](https://www.tindie.com) | Pre-flashed devices including the Apex 5 (Marauder + GPS + extras). Notable stores: [DSTIKE](https://www.tindie.com/stores/dstike/), [hamspiced (Piglet)](https://www.tindie.com/stores/hamspiced/). |
| [FlightAware](https://flightaware.com) | Outdoor 1090 MHz antennas for serious ADS-B range |
| [DSTIKE](https://dstike.com) | Pre-built ESP32 wardrivers, Deauthers, NugVR, and other purpose-built dev boards. Sells direct and on Tindie. |
| [Lab401](https://lab401.com) | EU exclusive distributor for Flipper Zero, Hak5, Proxmark. Avoids US-to-EU customs friction for newcomers in Europe. |
| [Hacker Warehouse](https://hackerwarehouse.com) | US reseller stocking Hak5, Flipper Zero, and wardriving accessories in one domestic store. |
| [LAB5 on Tindie](https://www.tindie.com/stores/lab/) and [lab5-11 Shopify](https://lab5-11.myshopify.com/) | C5Lab's storefronts. Wrocław PL. ESP32-C5 Marauder add-on PCBs: LAB ESP32C5 for Flipper Pager, M5MonsterC5 for Cardputer ADV / Tab5. Ships global. |
| [Biscuit Shop](https://biscuitshop.us) | Biscuit Pro and Biscuit Ultra (app-driven dual-ESP wardrivers), a CYD 2.8" Marauder/Bruce battery + GPS mod DIY kit, and merch. Also publishes the free [DIY Biscuit](https://biscuitshop.us/pages/diy-biscuits) firmware and web flasher for ESP32-C5 boards you already own. |
| [HaleHound](https://halehound.com) | Complete built HaleHound units (including a 3.5" build) for people who want the firmware without sourcing a CYD and modules. Free [web flasher](https://flash.halehound.com/) if you'd rather build your own. |
| [Midwest Gadgets](https://www.midwestgadgets.org/product-page/piglet) | US storefront carrying the Piglet wardriver alongside hamspiced's other builds. |
| [witnessmenow/ESP32-Cheap-Yellow-Display](https://github.com/witnessmenow/ESP32-Cheap-Yellow-Display) | Community hub for the Cheap Yellow Display boards. No canonical seller; boards ship from AliExpress, Amazon, and eBay listings. |

## This repo

[wdgo-onramp](https://github.com/HiroAlleyCat/wdgo-onramp) is [MIT licensed](LICENSE). Fork freely. If you adapt the guide for your community, an attribution link back to the repo is appreciated but not required.

Author: **HiroAlleyCat** on GitHub.

## Corrections welcome

If you authored one of the projects above and want the attribution updated, corrected, or removed — open an issue or PR. The contribution rules are in [CONTRIBUTING.md](CONTRIBUTING.md).
