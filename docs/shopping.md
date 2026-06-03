---
title: Shopping list
description: Buyer's checklist for each tier of the WDGoWars onramp
---


# Shopping list

What to buy at each level of the [Newcomer onramp](onramp.md). Organized to match the onramp's tier numbering.

**No prices listed.** Hardware prices drift week-to-week per vendor; the canonical answer is "click the link and see today's price for today's variant." Vendor links point at manufacturers when possible (more stable than reseller listings).

**Where multiple paths exist at the same tier, pick one.** More devices ≠ more leaderboard points (one API key = one driver — see [Newcomer onramp](onramp.md) § Common pitfalls).

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

GPS module is mandatory — without it, Marauder writes empty wardrive dumps. See [Hardware survey](survey.md) §3 for the source-code reference.

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

## Tier 6 — Portable Linux rigs (run the game + capture on one device)

Handheld Linux computers that can run both the WatchDogsGo game itself and the capture stack (Kismet, dump1090, Muninn, Heimdall). The "high-tier player" form factor.

### Path A — ClockworkPi uConsole (LOCOSP's reference platform for the game)

Best for: someone who wants the hardware [LOCOSP/WatchDogsGo](https://github.com/LOCOSP/WatchDogsGo) names in its repo description. Capture + play on one handheld.

| Item | Where |
|---|---|
| ClockworkPi uConsole kit | [clockworkpi.com](https://www.clockworkpi.com/product-page/uconsole-kit-rpi-cm4-lite) — kit minus the compute module |
| Raspberry Pi Compute Module 4 Lite (no eMMC) | [Adafruit](https://www.adafruit.com) / [CanaKit](https://www.canakit.com) — mainstream pairing, SD card holds the OS. CM5 is the newer + faster + pricier alternative. |
| ClockworkPi uConsole LoRa expansion board | [clockworkpi.com](https://www.clockworkpi.com) — optional, pairs with the MeshCore slot |
| ClockworkPi uConsole 4G expansion board | [clockworkpi.com](https://www.clockworkpi.com) — optional, lets the rig upload from the field without tethering |
| Monitor-mode USB Wi-Fi adapter | [Alfa AWUS036ACM (mt7612u)](https://www.alfa.com.tw) — well-supported monitor-mode driver on Linux |
| USB GPS receiver (u-blox-based) | [Adafruit](https://www.adafruit.com) |
| WatchDogsGo game | [GitHub](https://github.com/LOCOSP/WatchDogsGo) — Pyxel frontend; uConsole is in the repo's hardware list |
| Kismet | install via apt |

### Path B — ClockworkPi DevTerm

Best for: someone who prefers a BlackBerry-style keyboard form factor over the uConsole's handheld layout. Same compute-module socket as the uConsole, so the capture-side gear is identical.

| Item | Where |
|---|---|
| ClockworkPi DevTerm kit | [clockworkpi.com](https://www.clockworkpi.com) |
| (compute module + Wi-Fi adapter + GPS + Kismet + WatchDogsGo same as Path A) | |

### Path C — Steam Deck as a portable warbox

Best for: someone who already owns a Deck and wants to wardrive without buying new hardware.

| Item | Where |
|---|---|
| Steam Deck (LCD or OLED) | [steamdeck.com](https://www.steamdeck.com) — switch to Desktop Mode for the capture session |
| Monitor-mode USB Wi-Fi adapter | as Path A — the built-in radio doesn't do monitor mode reliably |
| USB GPS receiver | as Path A |
| Kismet | flatpak / distrobox |

## Tier 7 — High-end SDR + outdoor antenna chain

Stepping up from a basic RTL-SDR dongle to serious RF capture. Most of these feed Muninn the same way an RTL-SDR does — decode with dump1090-fa or readsb, point Muninn at the output directory. The TX-capable ones extend into transmit work (know the legalities for your region before you key up).

### Handheld standalone SDR — HackRF PortaPack

A HackRF One with a screen, keypad, and battery sled. Runs the [Mayhem](https://github.com/portapack-mayhem/mayhem-firmware) open firmware so it captures, replays, and transmits across 1 MHz–6 GHz without a computer attached. Closest thing the SDR scene has to a "tricorder" form factor.

| Item | Where |
|---|---|
| HackRF One | [greatscottgadgets.com/hackrf](https://greatscottgadgets.com/hackrf/one/) — half-duplex 1 MHz–6 GHz, 20 Msps |
| PortaPack H4M (current rev) or H2M | search vendors for "HackRF PortaPack H4M" / "H2M" — shell + screen + keypad + battery housing for the HackRF |
| Mayhem firmware | [portapack-mayhem/mayhem-firmware](https://github.com/portapack-mayhem/mayhem-firmware) — flash via DFU, replaces the stock Havok/PortaPack firmware |
| Optional: extra LiPo packs + telescoping whip | any | for field use |

Standalone use cases: ADS-B / AIS / POCSAG capture in the field, sub-GHz capture (315/433/868 MHz), GSM survey, Wi-Fi beacon spotter, recordings to microSD. Not a WDGoWars uploader on its own — pull the SD output to a PC and run it through Muninn (aircraft) or wigle-to-wdgwars (Wi-Fi CSV).

### Higher-tier SDRs (computer-attached)

| Item | Where |
|---|---|
| AirSpy R2 / Mini | [airspy.us](https://airspy.us) — better dynamic range than RTL-SDR, still cheap |
| AirSpy HF+ Discovery | [airspy.us](https://airspy.us) — HF-specialist (9 kHz–31 MHz + 60–260 MHz). Different game from ADS-B, but the buyer at this tier often wants both |
| SDRplay RSPdx-R2 | [sdrplay.com](https://www.sdrplay.com) — 14-bit, 1 kHz–2 GHz, popular for combined ADS-B + HF work |
| SDRplay RSPduo | [sdrplay.com](https://www.sdrplay.com) — dual-tuner, two coherent channels in one box |
| Nooelec NESDR SMArt v5 | [nooelec.com/store](https://www.nooelec.com/store) — RTL-SDR with built-in 0.5 ppm TCXO, SMA, metal case. Cheap, solid baseline |
| RTL-SDR Blog v4 | [rtl-sdr.com/buy](https://www.rtl-sdr.com/buy-rtl-sdr-dvb-t-dongles/) — the current generation of the RTL-SDR Blog dongle |
| ADALM-Pluto (PlutoSDR) | [analog.com/pluto](https://www.analog.com/en/resources/evaluation-hardware-and-software/evaluation-boards-kits/adalm-pluto.html) — TX-capable 70 MHz–6 GHz (with the standard hack), AD9363. Education-priced, popular with the SDR-learning crowd |
| LimeSDR Mini 2.0 | [crowdsupply.com/lime-micro](https://www.crowdsupply.com/lime-micro) — TX/RX 10 MHz–3.5 GHz, full duplex |
| LimeSDR USB | [crowdsupply.com/lime-micro](https://www.crowdsupply.com/lime-micro) — TX/RX 100 kHz–3.8 GHz, 2×2 MIMO |
| Nuand bladeRF 2.0 micro xA4 / xA9 | [nuand.com](https://www.nuand.com) — TX/RX 47 MHz–6 GHz, 2×2 MIMO. xA9 has the bigger FPGA |
| Ettus USRP B200mini / B205mini-i | [ettus.com](https://www.ettus.com) — research-grade SDR. Pricey; usually overkill unless you have a specific reason to spec it |
| KrakenSDR (5× phase-coherent RTL-SDRs) | [krakenrf.com](https://www.krakenrf.com) — 5 coherent channels for direction-finding / angle-of-arrival |
| KrakenSDR antenna array kit | [krakenrf.com](https://www.krakenrf.com) — 5 calibrated magnetic-mount antennas + ground plane + KrakenSDR DOA-DSP / KrakenRDF / DragonOS software. Buy the kit with the SDR — separate antennas have to be phase-matched yourself |

### Outdoor antennas

Different antenna types for different jobs. Most ADS-B feeders just need the FlightAware blade; the rest are for wider-band work.

| Item | Where |
|---|---|
| FlightAware 26" outdoor antenna | [FlightAware store](https://flightaware.com/adsb/) — the hobby standard for 1090 MHz |
| FlightAware Pro Stick Plus (1090 SAW filter + LNA built in) | [FlightAware store](https://flightaware.com/adsb/) — alternative to RTL-SDR Blog v4 if you specifically want pre-filtered ADS-B |
| Discone (25 MHz–1.3 GHz, omnidirectional) | search vendors for "discone antenna" — Diamond D3000N, Comet DS150S, etc. Wideband all-rounder |
| Log-periodic (directional wideband) | various — for pointed surveys across a wide band |
| Yagi 1090 MHz (directional ADS-B) | various — for serious gain in one heading, e.g. an airport corridor |
| Yagi 2.4 / 5 GHz (directional Wi-Fi) | TP-Link / Alfa / various — for distance Wi-Fi capture |
| Diamond X300 / Comet GP-9 (vertical, 2m / 70cm) | various — for amateur-band monitoring alongside the wardriving rig |
| External LNA — Uputronics 1090 MHz | [store.uputronics.com](https://store.uputronics.com) — mount at the antenna, not the receiver, for best gain |
| LMR-400 coax + N-type-to-SMA pigtails | any RF distributor — keep the run as short as your install allows |
| Lightning arrestor (N-type, gas-discharge) | any RF distributor — if the antenna is mast-mounted outdoors |

### RF accessories worth owning at this tier

| Item | Where |
|---|---|
| 1090 MHz SAW bandpass filter (inline) | [Uputronics](https://store.uputronics.com) / [Nooelec](https://www.nooelec.com/store) — cleans ADS-B from FM-broadcast desense |
| FM broadcast notch filter (88–108 MHz) | [Nooelec FM Bandstop](https://www.nooelec.com/store) — adjacent fix when the local FM stations are overloading the front end |
| Bias-T injector (5V or 12V) | various — for powering a remote LNA over the coax |
| Step attenuator | various — for taming over-strong signals during testing |
| GPSDO (GPS-disciplined oscillator) | Leo Bodnar Mini Precision / various — frequency reference for SDRs that accept an external 10 MHz clock |

### Better monitor-mode Wi-Fi adapters

| Item | Where |
|---|---|
| Alfa AWUS1900 (quad-band 802.11ac, four external antennas) | [alfa.com.tw](https://www.alfa.com.tw) — 2.4 + 5 GHz, heavyweight option |
| Alfa AWUS036ACM (mt7612u, dual-band) | [alfa.com.tw](https://www.alfa.com.tw) — smaller, very well-supported monitor mode |
| Panda Wireless PAU09 (RT5572) | [pandawireless.com](https://pandawireless.com) — old reliable for monitor-mode dev work |

### Software that matches the gear

| Tool | Where |
|---|---|
| DragonOS Focal | [sourceforge.net/projects/dragonos-focal](https://sourceforge.net/projects/dragonos-focal/) — Ubuntu spin pre-loaded with most SDR tooling (GNU Radio, gqrx, dump1090, kalibrate-rtl, OpenWebRX, KrakenSDR stack, etc.). Saves a day of install work |
| GNU Radio | [gnuradio.org](https://www.gnuradio.org) — flowgraph-based DSP framework underneath most of the above |
| OpenWebRX | [openwebrx.de](https://www.openwebrx.de) — browser-served SDR receiver, multi-user |
| gqrx | [gqrx.dk](https://gqrx.dk) — desktop SDR scanner, good first tool |

## Tier 8 — Vehicle install (always-on roaming capture)

If your daily commute is the capture run. Listed for completeness so the tier ladder doesn't dead-end; nothing here is bench-tested by this repo's author. Most wardrivers use ad-hoc car setups rather than permanent installs.

| Item | Where |
|---|---|
| 12V → USB-C PD car adapter | any — enough wattage to run a Pi 4 / 5 + screen + adapters |
| Roof magmount Wi-Fi antenna (2.4 / 5 GHz dual-band) | any — magnet base, N-type or SMA; pairs with the AWUS036ACM |
| Magmount 1090 MHz blade antenna | [FlightAware](https://flightaware.com/adsb/) — for an in-vehicle ADS-B feeder |
| External GPS antenna + receiver | u-blox — roof-mount for clean sky view |
| Mounting plate / RAM mount for the Pi or handheld | any |

## What NOT to buy (newcomer trap)

- **Bare ESP32-C3 boards as a wardriving target.** Marauder has no C3 binary, Bruce has no C3 port, GhostESP runs but the output lacks BSSID on some commands. C3 is poorly served by stock firmware. If you have one, treat it as a "build your own ESPHome probe firmware" project, not a turnkey wardriver. Details in [Hardware survey](survey.md) §3.
- **A second device on the same API key thinking it doubles your points.** Same key on two devices is one driver with two feeders, not two contesting drivers. See [Newcomer onramp](onramp.md) § Common pitfalls.
- **Marauder firmware without a GPS module.** The wardrive dumps will be empty. GPS is mandatory, not optional.
- **iPhone for the capture side.** No official WiGLE app on iOS. There are third-party scanners but none of them feed WiGLE-format CSV cleanly.

## Credits for everything listed

See [Credits](credits.md) for the maintainers, projects, and vendors that make this hobby possible.
