---
title: Capture flow diagram
description: The five-step WDGoWars onramp visualized as a flow diagram
---

# Capture flow diagram

The same five-step progression from the [newcomer onramp](onramp.md), drawn as a flow. Each lane represents one onramp step.

If you're reading this in Obsidian, the [`.canvas` version](../wdgo-capture-flow.canvas) in the repo root has the same layout as an interactive board. The Mermaid version below renders directly in GitHub Pages.

```mermaid
flowchart LR
    %% Step 1 — Phone only
    s1_phone[Android phone<br/>Wi-Fi + BT + GPS] --> s1_app[WiGLE Wifi Wardriving<br/>Android app]
    s1_app -->|auto-upload| wigle((WiGLE.net))

    %% Step 2 — Cross-post
    s2_phone[Android phone<br/>same as Step 1] --> s2_csv[.wiglecsv.gz export<br/>WigleWifi-1.6]
    s2_csv --> s2_w2w[wigle-to-wdgwars<br/>v1.2.0 --setup wizard]
    s2_w2w -->|POST /api/upload-csv| wdgwars_csv((wdgwars.pl<br/>/api/upload-csv))
    s2_csv -.->|or direct to WiGLE| wigle

    %% Step 3a — On-device uploaders
    s3a_cardputer[M5 Cardputer + GPS] --> s3a_bruce[LOCOSP Bruce fork<br/>v1.0-wdgwars]
    s3a_bruce -->|direct| wdgwars_csv
    s3a_xiao[XIAO ESP32-C5/S3/C6<br/>or T-Dongle C5] --> s3a_piglet[Piglet firmware<br/>hamspiced/piglet, 148★]
    s3a_piglet -->|web UI direct| wdgwars_csv
    s3a_pager[Hak5 Pineapple Pager<br/>+ u-blox 7 GPS] --> s3a_payload[LOCOSP Pineapple payload<br/>SYNC NOW button]
    s3a_payload -->|direct| wdgwars_csv
    s3a_rj[Raspberry Pi + LCD 1.44 + GPIO] --> s3a_rjpayload[wdgwars_upload.py<br/>Raspyjack payload]
    s3a_rjpayload -->|direct| wdgwars_csv

    %% Step 3b — Capture-only, PC feeder
    s3b_hw[Classic ESP32 / CYD<br/>+ GPS module] --> s3b_marauder[Marauder v1.12.1<br/>writes WigleWifi-1.4 to SD]
    s3b_marauder -->|SD pull| s3b_sd[wigle-to-wdgwars<br/>pads 1.4 to 1.6]
    s3b_sd --> wdgwars_csv

    %% Step 4 — WDGoWars-only slots
    s4_sdr[RTL-SDR<br/>+ 1090 MHz antenna] --> s4_dump[dump1090 / readsb]
    s4_dump --> s4_muninn[Muninn<br/>adsb-to-wdgwars v2.0.10]
    s4_lora[LoRa node<br/>Heltec / TTGO] --> s4_mm[MeshMapper CSV]
    s4_mm --> s4_heimdall[Heimdall<br/>meshcore-to-wdgwars v0.2.2]
    s4_muninn --> gungnir[gungnir v0.1.2<br/>HMAC + retry + cooldown]
    s4_heimdall --> gungnir
    gungnir -->|signed JSON<br/>POST /endpoint/upload/| wdgwars_json((wdgwars.pl<br/>/api/upload/))

    %% Styling
    classDef destinations fill:#fff3cd,stroke:#856404,stroke-width:2px
    classDef ondevice fill:#d4edda,stroke:#155724
    classDef pcside fill:#d1ecf1,stroke:#0c5460
    classDef transport fill:#f8d7da,stroke:#721c24,stroke-width:2px
    class wigle,wdgwars_csv,wdgwars_json destinations
    class s3a_bruce,s3a_piglet,s3a_payload,s3a_rjpayload ondevice
    class s2_w2w,s3b_sd,s4_muninn,s4_heimdall pcside
    class gungnir transport
```

## Reading the diagram

| Color | Meaning |
|---|---|
| Yellow | Destination endpoints (WiGLE.net, wdgwars.pl) |
| Green | On-device upload — no PC step needed |
| Blue | PC-side feeder — you upload from a computer |
| Red | Shared transport library (gungnir handles HMAC + retry + cooldown for the signed-JSON path) |

## Notes

- **One source can feed two destinations.** A WiGLE Android export is accepted by both WiGLE.net (direct) and wdgwars.pl (via wigle-to-wdgwars).
- **Two upload paths on wdgwars.pl side.** Bulk Wi-Fi/BLE CSVs go to `/api/upload-csv` (multipart). ADS-B + MeshCore go to `/api/upload/` as signed JSON via gungnir.
- **The `/endpoint/*` mirror exists** as a Cloudflare-L7 bypass for clients that burst-POST. gungnir v0.1.2+ uses it by default. Hand-rolled clients should target `/endpoint/*` rather than `/api/*` if they POST in bursts.

For the full progression with hardware suggestions and skill prerequisites at each level, see the [Newcomer onramp](onramp.md). For the firmware × chip support matrix behind each tier, see the [Hardware survey](survey.md).
