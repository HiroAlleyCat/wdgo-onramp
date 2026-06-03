---
tags: [wdgwars, shopping, hardware, newcomer]
created: 2026-06-03
last-verified: 2026-06-03
related:
  - "[[wdgo-newcomer-progression]]"
  - "[[wardriving-hardware-survey]]"
  - "[[CREDITS]]"
---

# Shopping list

What to buy at each level of the [[wdgo-newcomer-progression]]. Organized to match the onramp's tier numbering.

**No prices listed.** Hardware prices drift week-to-week per vendor; the canonical answer is "click the link and see today's price for today's variant." Vendor links point at manufacturers when possible (more stable than reseller listings).

**Where multiple paths exist at the same tier, pick one.** More devices ≠ more leaderboard points (one API key = one driver — see [[wdgo-newcomer-progression]] § Common pitfalls).

## Tier 1 — Phone only

The cheapest possible start. Validate you like the hobby before spending money.

| Item | Where | Notes |
|---|---|---|
| Android phone | already yours | No iPhone equivalent — Android only for the official WiGLE app. |
| WiGLE Wifi Wardriving | [Google Play](https://play.google.com/store/apps/details?id=net.wigle.wigleandroid) | Official WiGLE app. Free. |
| WiGLE account | [wigle.net](https://wigle.net) | Free signup. |

## Tier 2 — Cross-post WiGLE captures to WDGoWars

Same Tier-1 hardware. Add software + a second account.

| Item | Where | Notes |
|---|---|---|
| WDGoWars account + API key | [wdgwars.pl/profile](https://wdgwars.pl/profile) | Free. Profile → API Keys → generate. |
| wigle-to-wdgwars feeder | [GitHub](https://github.com/HiroAlleyCat/wigle-to-wdgwars) | `./run.sh --setup` walks you through both keys. |
| Python 3.9+ on your PC | OS package manager | Required to run the feeder. |

## Tier 3a — Dedicated device with on-device upload

Pick **one** of the four paths below. All upload directly to WDGoWars from the device; no PC step.

### Path A — M5 Cardputer + LOCOSP Bruce fork (most turnkey)

Best for: someone who wants the easiest end-to-end experience with a built-in screen + keyboard.

| Item | Where |
|---|---|
| M5Stack Cardputer | [shop.m5stack.com](https://shop.m5stack.com) — search "Cardputer" |
| M5Stack GPS/BDS Unit (AT6668) | [shop.m5stack.com](https://shop.m5stack.com) — search "GPS BDS Unit" |
| Grove cable (usually included with the GPS unit) | included |
| LOCOSP Bruce fork firmware | [GitHub `v1.0-wdgwars` release](https://github.com/LOCOSP/bruce-firmware-wdgwars/releases/tag/v1.0-wdgwars) |

Flash via esptool or M5Burner. Set `bruceConfig.wdgwarsApiKey` in the device config.

### Path B — XIAO ESP32 + Piglet (most modern, web UI)

Best for: someone who likes web dashboards over device-screen menus.

| Item | Where |
|---|---|
| XIAO ESP32-C5, S3, or C6 | [Seeed Studio XIAO series](https://www.seeedstudio.com/Seeed-XIAO-Series-c-1964.html) |
| **OR:** LilyGO T-Dongle C5 (separate variant of Piglet) | [LilyGO](https://lilygo.cc) |
| GPS module — NEO-6M ATGM336H or compatible 5V TTL serial | varies (Amazon, AliExpress) |
| Piglet firmware | [GitHub](https://github.com/hamspiced/piglet) |

Piglet has a web UI: open the device IP in a browser, paste your API key, hit "Test Key" → "Upload All."

### Path C — Hak5 Pineapple Pager + LOCOSP payload (pocket form factor)

Best for: someone who already owns or wants a Hak5 device.

| Item | Where |
|---|---|
| WiFi Pineapple Pager | [shop.hak5.org](https://shop.hak5.org) — search "Pineapple Pager" |
| u-blox 7 USB GPS stick | search any vendor for "u-blox 7 USB GPS" |
| LOCOSP Pineapple WDGoWars payload | [GitHub](https://github.com/LOCOSP/pineapple_pager_wdgwars) |

Requires a 3D GPS fix before scanning starts. Manual "SYNC NOW" button to upload.

### Path D — Raspyjack rig + WDGoWars payload (Pi-based)

Best for: someone comfortable with Linux + GPIO who wants a fully scriptable platform.

| Item | Where |
|---|---|
| Raspberry Pi (Zero W / 3 / 4) | [Adafruit](https://www.adafruit.com/category/176) / [CanaKit](https://www.canakit.com) |
| Waveshare 1.44" LCD HAT | [waveshare.com](https://www.waveshare.com/1.44inch-lcd-hat.htm) |
| microSD card (16 GB or larger) | any vendor |
| USB or HAT GPS module | any vendor |
| Raspyjack firmware + `payloads/exfiltration/wdgwars_upload.py` | [GitHub](https://github.com/7h30th3r0n3/Raspyjack) |

## Tier 3b — Cheapest path (capture-only, you upload from PC)

### Bare ESP32-WROOM or CYD + GPS + Marauder

Cheapest entry into dedicated hardware, more steps per upload.

| Item | Where |
|---|---|
| Classic ESP32-WROOM dev board | AliExpress (search "ESP32-WROOM-32 dev board") |
| **OR:** Cheap Yellow Display (CYD, ESP32-2432S028) | AliExpress (search "ESP32 2432S028") |
| GPS module — Teyleten Robot ATGM336H NEO-6M | Amazon / AliExpress |
| **OR:** DWEII GY-NEO6MV2 | Amazon / AliExpress |
| microSD card (any size — Marauder writes small files) | any |
| Marauder firmware | [GitHub `v1.12.1` release](https://github.com/justcallmekoko/ESP32Marauder/releases/tag/v1.12.1) — pick the binary that matches your board |
| Marauder GPS wiring guide | [Official wiki](https://github.com/justcallmekoko/ESP32Marauder/wiki/gps-modification) |
| wigle-to-wdgwars (for SD pulls) | [GitHub](https://github.com/HiroAlleyCat/wigle-to-wdgwars) |

GPS module is mandatory — without it, Marauder writes empty wardrive dumps. See [[wardriving-hardware-survey]] §3 for the source-code reference.

### Apex 5 — pre-flashed Marauder + GPS

Best for: someone who'd rather pay extra to skip the GPS soldering.

| Item | Where |
|---|---|
| Apex 5 module | search [Tindie](https://www.tindie.com) for "Apex 5"; [CNX Software writeup](https://www.cnx-software.com/2026/02/11/esp32-marauder-5g-apex-5-module-for-flipper-zero-combines-esp32-c5-two-sub-ghz-radios-nrf24-and-gps/) covers the spec |

## Tier 3 — High-end purchased option

### M5 Tab5 Wardriver

Best for: someone who wants the most-screen-real-estate option.

| Item | Where |
|---|---|
| M5Stack Tab5 | [shop.m5stack.com](https://shop.m5stack.com) — search "Tab5" |
| M5Stack GPS/BDS Unit (AT6668) | [shop.m5stack.com](https://shop.m5stack.com) — search "GPS BDS Unit" |
| Custom Tab5 Wardriver firmware | follow the [Hackster build by Runaque](https://www.hackster.io/Runaque/tab5-wardriver-a-custom-gps-enabled-wardriving-platform-d5948a) |

The Hackster post is the recipe — the project doesn't have a one-click installer yet.

## Tier 4 — WDGoWars-only data slots

These data types only score on WDGoWars; WiGLE doesn't accept them.

### ADS-B aircraft (Muninn feeder)

Stationary capture rig. Mount the antenna where it has sky view.

| Item | Where |
|---|---|
| RTL-SDR USB dongle | [RTL-SDR Blog v4 (official store)](https://www.rtl-sdr.com/buy-rtl-sdr-dvb-t-dongles/) **OR** [NooElec NESDR](https://www.nooelec.com/store/) |
| 1090 MHz antenna (basic) | usually included with the dongle |
| Optional outdoor antenna | [FlightAware 26" antenna](https://flightaware.com/adsb/) for serious range |
| Optional N-type-to-SMA adapter + LMR-400 coax | any RF distributor |
| Raspberry Pi 4 (4GB or 8GB) | [Adafruit](https://www.adafruit.com) / [CanaKit](https://www.canakit.com) |
| dump1090-fa, readsb, or tar1090 | FlightAware feeder or [readsb](https://github.com/wiedehopf/readsb) |
| Muninn feeder | [GitHub](https://github.com/HiroAlleyCat/adsb-to-wdgwars) |

### MeshCore LoRa nodes (Heimdall feeder)

Pocket-portable, captures other LoRa nodes within radio range.

| Item | Where |
|---|---|
| Heltec WiFi LoRa 32 (V3) | [heltec.org](https://heltec.org) |
| **OR:** LilyGO T-Beam | [LilyGO](https://lilygo.cc) |
| **OR:** RAK Wireless WisBlock | [store.rakwireless.com](https://store.rakwireless.com) |
| MeshMapper export or compatible CSV writer | per device firmware |
| Heimdall feeder | [GitHub](https://github.com/HiroAlleyCat/meshcore-to-wdgwars) |

## Tier 5 — Always-on capture lab

A scaled-up rig for serious coverage. Most of these you already have if you've reached this tier.

| Item | Where |
|---|---|
| Raspberry Pi 4 (4GB or 8GB) | [Adafruit](https://www.adafruit.com) / [CanaKit](https://www.canakit.com) |
| Monitor-mode USB Wi-Fi adapter | [Alfa AWUS036ACH or AWUS036ACS](https://www.alfa.com.tw) |
| USB GPS receiver (u-blox-based) | [Adafruit](https://www.adafruit.com) |
| External battery / power bank (for mobile use) | any |
| Kismet | [kismetwireless.net](https://www.kismetwireless.net) — install via package manager on Pi |
| Multiple ESP32 fleet (mix of Bruce + Marauder + Piglet on different channels) | per Tier 3 paths |
| Separate WDGoWars API key per attribution stream | [wdgwars.pl/profile](https://wdgwars.pl/profile) — one key = one driver |

## What NOT to buy (newcomer trap)

- **Bare ESP32-C3 boards as a wardriving target.** Marauder has no C3 binary, Bruce has no C3 port, GhostESP runs but the output lacks BSSID on some commands. C3 is poorly served by stock firmware. If you have one, treat it as a "build your own ESPHome probe firmware" project, not a turnkey wardriver. Details in [[wardriving-hardware-survey]] §3.
- **A second device on the same API key thinking it doubles your points.** Same key on two devices is one driver with two feeders, not two contesting drivers. See [[wdgo-newcomer-progression]] § Common pitfalls.
- **Marauder firmware without a GPS module.** The wardrive dumps will be empty. GPS is mandatory, not optional.
- **iPhone for the capture side.** No official WiGLE app on iOS. There are third-party scanners but none of them feed WiGLE-format CSV cleanly.

## Credits for everything listed

See [[CREDITS]] for the maintainers, projects, and vendors that make this hobby possible.
