---
title: Credits & acknowledgments
description: The maintainers, projects, and vendors that make the WDGoWars ecosystem possible
---


# Credits

This guide stands on a lot of community work. Acknowledging it here, organized by what each project contributes to the WDGoWars-or-WiGLE pipeline.

## The game

**LOCOSP** — created [WDGoWars (wdgwars.pl)](https://wdgwars.pl) and maintains the firmware family that uploads to it. The whole reason this repo exists. Repos:

| Repo | What it is |
|---|---|
| [WatchDogsGo](https://github.com/LOCOSP/WatchDogsGo) | The game source itself (Pyxel frontend for ESP32-C5 + ClockworkPi uConsole). The `plugins/wardrive_upload.py` is the reference HMAC implementation. |
| [bruce-firmware-wdgwars](https://github.com/LOCOSP/bruce-firmware-wdgwars) | Bruce fork with on-device WDGoWars upload (`src/modules/gps/wdgwars.cpp`). Tag `v1.0-wdgwars`. |
| [pineapple_pager_wdgwars](https://github.com/LOCOSP/pineapple_pager_wdgwars) | Hak5 WiFi Pineapple Pager payload — WigleWifi-1.6 CSV, GPS required, `/api/upload-csv`. |
| [WDGWatch](https://github.com/LOCOSP/WDGWatch) | LilyGO T-Watch Ultra companion firmware. |

## Capture firmwares

**justcallmekoko (Mark Spencer)** — creator and maintainer of the dominant ESP32 wardriving firmware family. The KokosStripClub Discord + the near-daily nightly releases keep the Marauder community a moving target in a healthy way.

| Repo | Role |
|---|---|
| [ESP32Marauder](https://github.com/justcallmekoko/ESP32Marauder) | The primary ESP32 wardriving firmware. 11k+ stars. |
| [ESP32DualBandWardriver](https://github.com/justcallmekoko/ESP32DualBandWardriver) | C5 dual-band wardriver; its README points users at the WDGoWars leaderboard alongside WiGLE. |

**pr3y and the Bruce contributors** — [Bruce upstream firmware (BruceDevices/firmware)](https://github.com/BruceDevices/firmware). LOCOSP's WDGoWars-flavored Bruce is a fork; the upstream is what gives Bruce its broad M5 + LilyGO + CYD support.

**Spooks4576** — [Ghost_ESP](https://github.com/Spooks4576/Ghost_ESP). Wide chip support (classic + S2 + S3 + C3 + C6). The only stock binary for bare ESP32-C3, with the caveat that some commands lack BSSID on that chip.

**7h30th3r0n3** — two heavyweight community projects:

| Repo | Role |
|---|---|
| [Evil-M5Project](https://github.com/7h30th3r0n3/Evil-M5Project) | M5-family pentest suite — Cardputer, AtomS3, Core2/CoreS3/Fire/AWS support. |
| [Raspyjack](https://github.com/7h30th3r0n3/Raspyjack) | Pi-based redteam toolkit. Ships the WDGoWars upload payload at `payloads/exfiltration/wdgwars_upload.py`. |

**hamspiced** — [Piglet (`hamspiced/piglet`)](https://github.com/hamspiced/piglet). Modern XIAO ESP32 wardriving firmware (148★, actively shipping) with a clean web UI for WDGoWars uploads. Adds XIAO C5/S3/C6 to the on-device-uploader chip set. The fourth confirmed on-device WDGoWars uploader.

## Third-party feeders

Tools authored outside LOCOSP and HiroAlleyCat that POST to wdgwars.pl.

| Maintainer | Repo | Role |
|---|---|---|
| **phutur1st** | [intercept-wdgwars](https://github.com/phutur1st/intercept-wdgwars) | Exports ADS-B from an `intercept` PostgreSQL database into the aircraft.json shape, uploads to wdgwars. |
| **DeflockJoplin** | [pack (P.A.C.K.)](https://github.com/DeflockJoplin/pack) | Rust passive capture suite — 802.11 + BLE + GPS, WiGLE CSV output, Flock-camera detection, wdgwars upload. |
| **InfIux** | [Wardriving-Log-Aggregation](https://github.com/InfIux/Wardriving-Log-Aggregation) | Aggregates Marauder v8 `.log` files for upload to WDGoWars or WiGLE. |

## Parent platform

**[WiGLE](https://wigle.net)** — the long-running, free, ad-free hobby database. WDGoWars's bulk-upload format is WigleWifi-1.6 (WiGLE's own spec at [api.wigle.net/csvFormat.html](https://api.wigle.net/csvFormat.html)). The hobby in this corner of the internet would not exist without WiGLE.

The official WiGLE Android app ([Google Play](https://play.google.com/store/apps/details?id=net.wigle.wigleandroid)) is the recommended Tier-1 capture tool in [Newcomer onramp](onramp.md).

## General-hobby documentation

**ringmast4r** — the closest thing the broader wardriving hobby has to a unified one-stop reference.

| Source | What it is |
|---|---|
| [Homebrew Wardriving roladex](https://ringmast4r.org/html-roladex/homebrew-wardriving) | Build comparisons across hardware classes. |
| [The Wonderful World of Wardriving (substack)](https://ringmast4r.substack.com/p/the-wonderful-world-of-wardriving) | Hobby overview and history. |
| [I Mapped 2.98 Million WiFi Networks (substack)](https://ringmast4r.substack.com/p/i-mapped-298-million-wifi-networks) | Personal-scale ops writeup. |

**[agucova/awesome-esp](https://github.com/agucova/awesome-esp)** — broader ESP8266/32 project curation, useful for finding adjacent firmwares.

**Runaque** — author of the [Tab5 Wardriver build on Hackster](https://www.hackster.io/Runaque/tab5-wardriver-a-custom-gps-enabled-wardriving-platform-d5948a). Recipe for the high-end Tab5 path.

## HiroAlleyCat feeder family

The sibling repos to this one. Released under MIT, source available, primary author maintained.

| Repo | What it does | Latest |
|---|---|---|
| [adsb-to-wdgwars (Muninn)](https://github.com/HiroAlleyCat/adsb-to-wdgwars) | ADS-B → wdgwars `aircraft` slot. Accepts AVR, SBS-1, dump1090, readsb, tar1090, VRS, Stratux, Mode-S Beast, NDJSON, Mayhem, GDL-90, CSV. | v2.0.10 |
| [meshcore-to-wdgwars (Heimdall)](https://github.com/HiroAlleyCat/meshcore-to-wdgwars) | MeshCore LoRa → wdgwars `meshcore_nodes` slot. | v0.2.2 |
| [wigle-to-wdgwars](https://github.com/HiroAlleyCat/wigle-to-wdgwars) | WigleWifi-1.6 CSV → wdgwars bulk via multipart upload. | v1.2.0 |
| [gungnir](https://github.com/HiroAlleyCat/gungnir) | Shared HMAC transport library used by the three above. | v0.1.2 |
| [wdgwars-api-tester](https://github.com/HiroAlleyCat/wdgwars-api-tester) | Systematic probe of the WDGoWars HTTP API surface. | main branch |

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
| [Tindie](https://www.tindie.com) | Pre-flashed devices including the Apex 5 (Marauder + GPS + extras) |
| [FlightAware](https://flightaware.com) | Outdoor 1090 MHz antennas for serious ADS-B range |

## This repo

[wdgo-onramp](https://github.com/HiroAlleyCat/wdgo-onramp) is [MIT licensed](LICENSE). Fork freely. If you adapt the guide for your community, an attribution link back to the repo is appreciated but not required.

Author: **HiroAlleyCat** on GitHub.

## Corrections welcome

If you authored one of the projects above and want the attribution updated, corrected, or removed — open an issue or PR. The contribution rules are in [CONTRIBUTING.md](CONTRIBUTING.md).
