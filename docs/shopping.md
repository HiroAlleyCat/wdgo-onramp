---
title: Shopping
description: Buyer's checklist for each tier of the WDGoWars onramp
brand: SHOPPING
tagline: / tier-by-tier buyer's checklist / no fabricated prices /
---

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
| wigle-to-wdgwars feeder | [GitHub](https://github.com/Yggdrasil-AI-labs/wigle-to-wdgwars) | `./run.sh --setup` walks you through both keys. |
| Python 3.9+ on your PC | OS package manager | Required to run the feeder. |

## Tier 3 — Quick reference: solder vs solderless

The Tier 3 hardware paths split cleanly on whether you need to solder. Pick on that axis first; pick on-device-upload vs PC-upload (Tier 3a vs 3b) second.

| Solder skill | Paths |
|---|---|
| **No soldering at all** — everything plugs in via USB, Grove, JST, or stacked HAT | Tier 3a Path A (M5 Cardputer + Grove GPS) · Tier 3a Path C (Pineapple Pager + USB GPS) · Tier 3a Path D (Raspyjack rig + USB GPS) · Tier 3b Apex 5 (pre-flashed) · Tier 3b Pwnagotchi (HAT stack + PiSugar + USB GPS) |
| **Basic through-hole soldering needed** — 4 wires from a GPS module (VCC, GND, TX, RX) to header pads. Beginner-friendly first solder project. | Tier 3a Path B (Piglet on XIAO + GPS) · Tier 3b bare ESP32 / CYD + GPS + Marauder · Tier 3b Flipper Zero WiFi DevBoard + GPS |

If you don't solder and don't want to learn yet: **M5 Cardputer + LOCOSP Bruce fork** (Tier 3a Path A) is the cleanest "just works" pick. Apex 5 is the cleanest cheap-pick if you only care about capture and don't need on-device upload.

If you want to learn: a soldering iron, lead-free solder, helping hands, and 30 minutes of YouTube get you to GPS-on-ESP32. That's the same skill the rest of the hobby keeps using.

## Tier 3a — Dedicated device with on-device upload

Pick **one** of the four paths below. All upload directly to WDGoWars from the device; no PC step.

### Path A — M5 Cardputer + LOCOSP Bruce fork (most turnkey) — *No soldering*

Best for: someone who wants the easiest end-to-end experience with a built-in screen + keyboard.

| Item | Where |
|---|---|
| M5Stack Cardputer | [shop.m5stack.com](https://shop.m5stack.com) — search "Cardputer" |
| M5Stack GPS/BDS Unit (AT6668) | [shop.m5stack.com](https://shop.m5stack.com) — search "GPS BDS Unit" |
| Grove cable (usually included with the GPS unit) | included |
| LOCOSP Bruce fork firmware | [GitHub `v1.0-wdgwars` release](https://github.com/LOCOSP/bruce-firmware-wdgwars/releases/tag/v1.0-wdgwars) |

Flash via esptool or M5Burner. Set `bruceConfig.wdgwarsApiKey` in the device config.

**Optional C5 dual-band upgrade for Cardputer ADV / Tab5:** the [LAB5 M5MonsterC5](https://www.tindie.com/stores/lab/) add-on adds an ESP32-C5 + sub-1 GHz radio carrier. Pair with [C5Lab/projectZero firmware](https://github.com/C5Lab/projectZero) for the Marauder/projectZero feature set on a Cardputer chassis. Adds capture surfaces (2.4 GHz, 5 GHz, sub-1 GHz) the stock Cardputer can't reach. Not required for WDGoWars uploads, but worth knowing about for hardware-curious newcomers who landed on Path A.

### Path B — XIAO ESP32 + Piglet (most modern, web UI) — *Soldering required*

Best for: someone who likes web dashboards over device-screen menus.

| Item | Where |
|---|---|
| XIAO ESP32-C5, S3, or C6 | [Seeed Studio XIAO series](https://www.seeedstudio.com/Seeed-XIAO-Series-c-1964.html) |
| **OR:** LilyGO T-Dongle C5 (separate variant of Piglet) | [LilyGO](https://lilygo.cc) |
| GPS module — NEO-6M ATGM336H or compatible 5V TTL serial | varies (Amazon, AliExpress) |
| Piglet firmware | [GitHub](https://github.com/hamspiced/piglet) |

Piglet has a web UI: open the device IP in a browser, paste your API key, hit "Test Key" → "Upload All."

### Path C — Hak5 Pineapple Pager + LOCOSP payload (pocket form factor) — *No soldering*

Best for: someone who already owns or wants a Hak5 device.

| Item | Where |
|---|---|
| WiFi Pineapple Pager | [shop.hak5.org](https://shop.hak5.org) — search "Pineapple Pager" |
| u-blox 7 USB GPS stick | search any vendor for "u-blox 7 USB GPS" |
| LOCOSP Pineapple WDGoWars payload | [GitHub](https://github.com/LOCOSP/pineapple_pager_wdgwars) |

Requires a 3D GPS fix before scanning starts. Manual "SYNC NOW" button to upload.

### Path D — Raspyjack rig + WDGoWars payload (Pi-based) — *No soldering*

Best for: someone comfortable with Linux + GPIO who wants a fully scriptable platform.

| Item | Where |
|---|---|
| Raspberry Pi (Zero W / 3 / 4) | [Adafruit](https://www.adafruit.com/category/176) / [CanaKit](https://www.canakit.com) |
| Waveshare 1.44" LCD HAT | [waveshare.com](https://www.waveshare.com/1.44inch-lcd-hat.htm) |
| microSD card (16 GB or larger) | any vendor |
| USB or HAT GPS module | any vendor |
| Raspyjack firmware + `payloads/exfiltration/wdgwars_upload.py` | [GitHub](https://github.com/7h30th3r0n3/Raspyjack) |

## Tier 3b — Cheapest path (capture-only, you upload from PC)

### Bare ESP32-WROOM or CYD + GPS + Marauder — *Soldering required*

Cheapest entry into dedicated hardware, more steps per upload.

| Item | Where |
|---|---|
| Classic ESP32-WROOM dev board | AliExpress (search "ESP32-WROOM-32 dev board") |
| **OR:** Cheap Yellow Display (CYD, ESP32-2432S028) | AliExpress (search "ESP32 2432S028"); community hub at [witnessmenow/ESP32-Cheap-Yellow-Display](https://github.com/witnessmenow/ESP32-Cheap-Yellow-Display) lists tested boards and pinouts before you buy |
| GPS module — Teyleten Robot ATGM336H NEO-6M | Amazon / AliExpress |
| **OR:** DWEII GY-NEO6MV2 | Amazon / AliExpress |
| microSD card (any size — Marauder writes small files) | any |
| Marauder firmware | [GitHub `v1.12.1` release](https://github.com/justcallmekoko/ESP32Marauder/releases/tag/v1.12.1) — pick the binary that matches your board |
| Marauder GPS wiring guide | [Official wiki](https://github.com/justcallmekoko/ESP32Marauder/wiki/gps-modification) |
| wigle-to-wdgwars (for SD pulls) | [GitHub](https://github.com/Yggdrasil-AI-labs/wigle-to-wdgwars) |

GPS module is mandatory — without it, Marauder writes empty wardrive dumps. See [Hardware survey](survey.md) §3 for the source-code reference.

### Apex 5 — pre-flashed Marauder + GPS — *No soldering*

Best for: someone who'd rather pay extra to skip the GPS soldering.

| Item | Where |
|---|---|
| Apex 5 module | search [Tindie](https://www.tindie.com) for "Apex 5"; [CNX Software writeup](https://www.cnx-software.com/2026/02/11/esp32-marauder-5g-apex-5-module-for-flipper-zero-combines-esp32-c5-two-sub-ghz-radios-nrf24-and-gps/) covers the spec |

### Flipper Zero + WiFi DevBoard (Marauder) — *Soldering required*

Best for: someone who already owns a Flipper Zero and wants to use it as the controller for a wardriving rig. The Flipper itself doesn't have a 2.4 GHz Wi-Fi radio; the WiFi DevBoard plugs into the GPIO header and adds an ESP32-S2 running Marauder.

| Item | Where |
|---|---|
| Flipper Zero | [shop.flipperzero.one](https://shop.flipperzero.one) |
| Flipper WiFi DevBoard (ESP32-S2) | [Flipper Zero shop](https://shop.flipperzero.one) — official; third-party clones on AliExpress |
| Marauder firmware for the DevBoard | [GitHub `v1.12.1` release](https://github.com/justcallmekoko/ESP32Marauder/releases/tag/v1.12.1) — pick the `flipper.bin` asset |
| GPS module (NEO-6M / ATGM336H) wired to the DevBoard headers | per the [Marauder GPS wiki](https://github.com/justcallmekoko/ESP32Marauder/wiki/gps-modification) — same pin tables as the bare-ESP32 path above |
| microSD card | any |
| Optional: Flipper custom firmware (Momentum / Xtreme) | [Momentum](https://github.com/Next-Flip/Momentum-Firmware) — adds the Marauder companion app baked-in; not strictly required since the Marauder firmware runs on the DevBoard itself |
| wigle-to-wdgwars (for SD pulls) | [GitHub](https://github.com/Yggdrasil-AI-labs/wigle-to-wdgwars) |

Same GPS rule as bare Marauder: no module, no wardrive lines. The Flipper provides display + power + UI; the DevBoard does the actual Wi-Fi scanning.

### Pwnagotchi (handshake-focused, not WiGLE-CSV native) — *No soldering*

Best for: someone who specifically wants the Pwnagotchi build for the WPA handshake side of the hobby. Listed here because newcomers ask about it constantly. **Pwnagotchi does NOT produce WigleWifi-1.6 CSV out of the box** — it captures PCAP handshakes and (with the GPS plugin) location-tagged metadata. Conversion path to WDGoWars exists but isn't a one-liner. Don't buy a Pwnagotchi expecting points-per-mile parity with a Marauder rig.

| Item | Where |
|---|---|
| Raspberry Pi Zero 2 W (recommended) **OR** original Pi Zero W | [Adafruit](https://www.adafruit.com) / [CanaKit](https://www.canakit.com) |
| Waveshare 2.13" e-paper HAT (the iconic Pwnagotchi screen) | [waveshare.com](https://www.waveshare.com) — V2 or V4 |
| PiSugar 3 / PiSugar S Plus (LiPo + UPS HAT) | [pisugar.com](https://www.pisugar.com) — keeps it portable |
| microSD card (16 GB or larger) | any |
| External USB GPS or [bettercap-gps plugin](https://github.com/jayofelony/pwnagotchi) | for any chance of location-tagged output |
| Pwnagotchi firmware — jayofelony fork | [github.com/jayofelony/pwnagotchi](https://github.com/jayofelony/pwnagotchi) — the actively-maintained fork as of 2026 |
| Case (3D-printed or official) | various — Etsy / Thingiverse / Printables |

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
| Muninn feeder | [GitHub](https://github.com/Yggdrasil-AI-labs/adsb-to-wdgwars) |

### MeshCore LoRa nodes (Heimdall feeder)

Pocket-portable, captures other LoRa nodes within radio range. **MeshCore is the supported protocol for Heimdall today — Meshtastic exports need a parser nobody's written yet (see [Hardware survey](survey.md) §9).** If your goal is feeding the `meshcore_nodes` slot, buy hardware you can run MeshCore-compatible firmware on.

| Item | Where |
|---|---|
| Heltec WiFi LoRa 32 (V3) | [heltec.org](https://heltec.org) |
| **OR:** LilyGO T-Beam | [LilyGO](https://lilygo.cc) |
| **OR:** RAK Wireless WisBlock | [store.rakwireless.com](https://store.rakwireless.com) |
| MeshMapper export or compatible CSV writer | per device firmware |
| Heimdall feeder | [GitHub](https://github.com/Yggdrasil-AI-labs/meshcore-to-wdgwars) |

### Meshtastic-ecosystem gear (related but different protocol)

If you're getting into the broader LoRa scene rather than specifically chasing the WDGoWars MeshCore slot, Meshtastic is the more popular mesh protocol. The hardware below runs both Meshtastic and (on most boards) MeshCore — pick the firmware after picking the goal. Re-flash to MeshCore if you want Heimdall to ingest it today.

| Item | Where |
|---|---|
| LilyGO T-Echo (nRF52840 + SX1262, e-paper) | [LilyGO](https://lilygo.cc) — popular keychain-sized Meshtastic node |
| LilyGO T-Deck Plus (ESP32-S3 + keyboard + screen) | [LilyGO](https://lilygo.cc) — handheld Meshtastic with input |
| LilyGO T-Beam Supreme | [LilyGO](https://lilygo.cc) — newer T-Beam with U-blox GPS + 18650 holder |
| LilyGO Station G2 | [LilyGO](https://lilygo.cc) — solar-friendly stationary node |
| Heltec Vision Master T190 | [heltec.org](https://heltec.org) — newer Heltec with E-ink |
| RAK Wireless WisMesh Pocket | [store.rakwireless.com](https://store.rakwireless.com) — RAK's Meshtastic-ready pocket node |
| RAK4631 (nRF52840 + SX1262 core module) | [store.rakwireless.com](https://store.rakwireless.com) — the silicon most other Meshtastic builds share |
| Nano-G1 / Nano-G1 Explorer | [B&Q Consulting](https://www.bnqconsulting.com) — minimalist credit-card-sized Meshtastic node |
| Meshtastic firmware | [meshtastic.org](https://meshtastic.org) — official documentation + flashing tool |
| MeshCore firmware (the WDGoWars-supported alternative) | [meshcore.co.uk](https://meshcore.co.uk) — flash this instead if your goal is the Heimdall feed |

## Tier 5 — Always-on capture lab

A scaled-up rig for serious coverage. Most of these you already have if you've reached this tier. **A Linux laptop is a drop-in substitute for the Pi here** — Kismet runs the same on a ThinkPad / Framework / any monitor-mode-capable USB-Wi-Fi rig. The Pi is just the cheap always-on box; the software is what matters.

| Item | Where |
|---|---|
| Raspberry Pi 4 (4GB or 8GB) | [Adafruit](https://www.adafruit.com) / [CanaKit](https://www.canakit.com) — or any Linux laptop you already own |
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

### IMSI-catcher detectors (RX-only, defensive use)

Different game from the rest of Tier 7 — these don't feed WDGoWars at all, but they're listed here because they reuse the same SDR you bought for ADS-B. **RX-only**: passively log cellular base stations and flag anomalies suggesting a rogue eNB. Transmitting on cellular bands is a federal felony in most countries; these tools strictly receive.

| Tool | Where |
|---|---|
| Crocodile Hunter (EFF) | [github.com/EFForg/crocodilehunter](https://github.com/EFForg/crocodilehunter) — passive LTE base-station logger with anomaly flags; needs a USB Qualcomm modem (Quectel EC25 etc.) |
| SnoopSnitch (Android side) | [opensource.srlabs.de/projects/snoopsnitch](https://opensource.srlabs.de/projects/snoopsnitch) — needs a rooted Android phone with a Qualcomm chipset that exposes diag mode |
| Reference: srsRAN-Project (RX-only LTE/5G stack) | [github.com/srsran/srsRAN_Project](https://github.com/srsran/srsRAN_Project) — full SDR LTE/5G stack; ignore the eNB/gNB TX paths for this use case |

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

## Region-specific resellers and curated kits

The manufacturer storefronts above ship globally but rarely fastest or cheapest. These resellers are worth checking for region-domestic shipping or pre-assembled kits.

| Shop | Region | What you'd buy there |
|---|---|---|
| [DSTIKE](https://dstike.com) (also [DSTIKE on Tindie](https://www.tindie.com/stores/dstike/)) | global | Pre-built ESP32 wardrivers, Deauthers, NugVR, and other purpose-built dev boards. Closest thing to a turnkey wardriving board without flashing it yourself. |
| [Lab401](https://lab401.com) | EU | Exclusive EU distributor for Flipper Zero, Hak5, Proxmark. Avoids US-to-EU customs friction. |
| [Hacker Warehouse](https://hackerwarehouse.com) | US | Hak5, Flipper Zero, and wardriving accessories in one US-domestic store. |
| [hamspiced on Tindie](https://www.tindie.com/stores/hamspiced/) + [Midwest Gadgets Piglet product page](https://www.midwestgadgets.org/product-page/piglet) | global | Pre-flashed Piglet hardware if you'd rather skip the flash step on Tier 3a Path B. |
| [LAB5 on Tindie](https://www.tindie.com/stores/lab/) + [lab5-11 Shopify](https://lab5-11.myshopify.com/) | EU (Wrocław PL), ships global | ESP32-C5 Marauder add-on PCBs: LAB ESP32C5 (Flipper Pager mod) and M5MonsterC5 (Cardputer ADV / Tab5 carrier). Pairs with [C5Lab/projectZero firmware](https://github.com/C5Lab/projectZero). |

## What NOT to buy (newcomer trap)

- **Bare ESP32-C3 boards as a wardriving target.** Marauder has no C3 binary, Bruce has no C3 port, GhostESP runs but the output lacks BSSID on some commands. C3 is poorly served by stock firmware. If you have one, treat it as a "build your own ESPHome probe firmware" project, not a turnkey wardriver. Details in [Hardware survey](survey.md) §3.
- **A second device on the same API key thinking it doubles your points.** Same key on two devices is one driver with two feeders, not two contesting drivers. See [Newcomer onramp](onramp.md) § Common pitfalls.
- **Marauder firmware without a GPS module.** The wardrive dumps will be empty. GPS is mandatory, not optional.
- **iPhone for the capture side.** No official WiGLE app on iOS. There are third-party scanners but none of them feed WiGLE-format CSV cleanly.

## Credits for everything listed

See [Credits](credits.md) for the maintainers, projects, and vendors that make this hobby possible.
