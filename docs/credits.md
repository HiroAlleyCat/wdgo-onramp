---
title: Credits
description: The maintainers, projects, and vendors that make the WDGoWars ecosystem possible
brand: CREDITS
tagline: / maintainers <span>·</span> projects <span>·</span> vendors <span>·</span> communities /
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

**hamspiced** — [Piglet (`hamspiced/piglet`)](https://github.com/hamspiced/piglet). Modern XIAO ESP32 wardriving firmware (148★, actively shipping) with a clean web UI for WDGoWars uploads. Adds XIAO C5/S3/C6 to the on-device-uploader chip set. The fourth confirmed on-device WDGoWars uploader. Piglet hardware is sold via [hamspiced on Tindie](https://www.tindie.com/stores/hamspiced/) and [Midwest Gadgets](https://www.midwestgadgets.org/product-page/piglet).

**LAB5 / C5Lab (Labolatorium, Wrocław PL)** — hardware + firmware group focused on the ESP32-C5. Their [projectZero firmware](https://github.com/C5Lab/projectZero) (163★) runs on the Flipper Zero Pager via their LAB ESP32C5 add-on PCB, and on Cardputer ADV / Tab5 via their M5MonsterC5 add-on (157★ for the [M5MonsterC5-CardputerADV repo](https://github.com/C5Lab/M5MonsterC5-CardputerADV)). The add-ons expose sub-1 GHz capture on chips that otherwise can't do it. Hardware sold from [Tindie store](https://www.tindie.com/stores/lab/) and [lab5-11 Shopify](https://lab5-11.myshopify.com/); quick-start at [c5lab.github.io/projectZero](https://c5lab.github.io/projectZero/).

## Communities

The Discord servers and forums where the people behind the above firmwares answer questions. Invite links rot; the durable references are the project README files.

| Community | Reference link | What it's for |
|---|---|---|
| WDGoWars / LOCOSP | [wdgwars.pl/press](https://wdgwars.pl/press), [LOCOSP on GitHub](https://github.com/LOCOSP) | Game itself, leaderboard, upload pipeline questions. Direct contact via Discord DM to `@locosp`. |
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

## Creators and video coverage

YouTube channels and personal sites producing wardriving and WDGoWars-specific content. Linked separately because they're how most newcomers see a working rig before they buy one.

| Creator | Where | Coverage |
|---|---|---|
| **Valley Tech Solutions** | [YouTube](https://www.youtube.com/@Valleytechsolutions) | WDGoWars collabs, wardriving rigs, on-device walkthroughs. |
| **justcallmekoko** | [YouTube](https://www.youtube.com/justcallmekoko) | Marauder firmware demos and hardware tours from the firmware author. |
| **GhostStrats (Spooks4576)** | [YouTube](https://www.youtube.com/channel/UCzSZPWtTRA4G946XRAn2XLQ) | Ghost_ESP firmware walkthroughs across many chips. |
| **7h30th3r0n3** | [YouTube](https://www.youtube.com/channel/UCN1sTrFbvdliXTUOsY5DkyA) | Evil-M5Project and Raspyjack demos from their author. |
| **ringmast4r** | [ringmast4r.org](https://ringmast4r.org), [substack](https://ringmast4r.substack.com), [Instagram](https://www.instagram.com/ringmast4r/), [X](https://x.com/Ringmast4r) | Wardriving hobby coverage and personal-scale ops writeups. Video archive on-site rather than on YouTube. |

LOCOSP maintains [wdgwars.pl/press](https://wdgwars.pl/press) for content creators covering the game; that page is the canonical place to discover new WDGoWars-specific coverage as it ships.

## Adjacent open-source tooling

Projects the onramp leans on without listing them as feeders themselves. Each is the reference implementation for its slot.

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
| [Tindie](https://www.tindie.com) | Pre-flashed devices including the Apex 5 (Marauder + GPS + extras). Notable stores: [DSTIKE](https://www.tindie.com/stores/dstike/), [hamspiced (Piglet)](https://www.tindie.com/stores/hamspiced/). |
| [FlightAware](https://flightaware.com) | Outdoor 1090 MHz antennas for serious ADS-B range |
| [DSTIKE](https://dstike.com) | Pre-built ESP32 wardrivers, Deauthers, NugVR, and other purpose-built dev boards. Sells direct and on Tindie. |
| [Lab401](https://lab401.com) | EU exclusive distributor for Flipper Zero, Hak5, Proxmark. Avoids US-to-EU customs friction for newcomers in Europe. |
| [Hacker Warehouse](https://hackerwarehouse.com) | US reseller stocking Hak5, Flipper Zero, and wardriving accessories in one domestic store. |
| [LAB5 on Tindie](https://www.tindie.com/stores/lab/) and [lab5-11 Shopify](https://lab5-11.myshopify.com/) | C5Lab's storefronts. Wrocław PL. ESP32-C5 Marauder add-on PCBs: LAB ESP32C5 for Flipper Pager, M5MonsterC5 for Cardputer ADV / Tab5. Ships global. |
| [witnessmenow/ESP32-Cheap-Yellow-Display](https://github.com/witnessmenow/ESP32-Cheap-Yellow-Display) | Community hub for the Cheap Yellow Display boards. No canonical seller; boards ship from AliExpress, Amazon, and eBay listings. |

## This repo

[wdgo-onramp](https://github.com/HiroAlleyCat/wdgo-onramp) is [MIT licensed](LICENSE). Fork freely. If you adapt the guide for your community, an attribution link back to the repo is appreciated but not required.

Author: **HiroAlleyCat** on GitHub.

## Corrections welcome

If you authored one of the projects above and want the attribution updated, corrected, or removed — open an issue or PR. The contribution rules are in [CONTRIBUTING.md](CONTRIBUTING.md).
