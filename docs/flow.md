---
title: Capture flow
description: The five-step WDGoWars onramp visualized as a flow diagram
brand: CAPTURE FLOW
tagline: / five-step progression / visual map /
---

The same five-step progression from the [newcomer onramp](onramp.md), drawn as a flow. Each lane represents one onramp step.

If you're reading this in Obsidian, the [`.canvas` version](../wdgo-capture-flow.canvas) in the repo root has the same layout as an interactive board. The Mermaid version below renders directly in GitHub Pages.

```mermaid
flowchart LR
    %% Step 1 — Phone only
    s1_phone[Android phone<br/>Wi-Fi + BT + GPS] --> s1_app[WiGLE Wifi Wardriving<br/>Android app]
    s1_app -->|auto-upload| wigle((WiGLE.net))

    %% Step 2 — Cross-post
    s2_phone[Android phone<br/>same as Step 1] --> s2_csv[.wiglecsv.gz export<br/>WigleWifi-1.6]
    s2_csv --> s2_w2w[wigle-to-wdgwars<br/>v1.6.2 --setup wizard]
    s2_w2w -->|POST /api/upload-csv| wdgwars_csv((wdgwars.pl<br/>/api/upload-csv))
    s2_csv -.->|or direct to WiGLE| wigle

    %% Step 3a — On-device uploaders
    s3a_cardputer[M5 Cardputer + GPS] --> s3a_bruce[LOCOSP Bruce fork<br/>v1.0-wdgwars]
    s3a_bruce -->|direct| wdgwars_csv
    s3a_xiao[XIAO ESP32-C5/S3/C6<br/>or T-Dongle C5] --> s3a_piglet[Piglet firmware<br/>hamspiced/piglet, 191★]
    s3a_piglet -->|web UI direct| wdgwars_csv
    s3a_pager[Hak5 Pineapple Pager<br/>+ u-blox 7 GPS] --> s3a_payload[LOCOSP Pineapple payload<br/>SYNC NOW button]
    s3a_payload -->|direct| wdgwars_csv
    s3a_rj[Raspberry Pi + LCD 1.44 + GPIO] --> s3a_rjpayload[wdgwars_upload.py<br/>Raspyjack payload]
    s3a_rjpayload -->|direct| wdgwars_csv

    %% Step 3b — Capture-only, PC feeder
    s3b_hw[Classic ESP32 / CYD<br/>+ GPS module] --> s3b_marauder[Marauder v1.13.0<br/>writes WigleWifi-1.4 to SD]
    s3b_marauder -->|SD pull| s3b_sd[wigle-to-wdgwars<br/>pads 1.4 to 1.6]
    s3b_sd --> wdgwars_csv

    %% Step 4 — WDGoWars-only slots
    s4_sdr[RTL-SDR<br/>+ 1090 MHz antenna] --> s4_dump[dump1090 / readsb]
    s4_dump --> s4_muninn[Muninn<br/>adsb-to-wdgwars v2.0.16]
    s4_lora[LoRa node<br/>Heltec / TTGO] --> s4_mm[MeshMapper CSV]
    s4_mm --> s4_heimdall[Heimdall<br/>meshcore-to-wdgwars v0.4.5]
    s4_muninn --> gungnir[gungnir v0.1.3<br/>HMAC + retry + cooldown]
    s4_heimdall --> gungnir
    gungnir -->|signed JSON<br/>POST /endpoint/upload/| wdgwars_json((wdgwars.pl<br/>/api/upload/))

    %% Styling — CRT palette matching the on-ramp brand
    classDef destinations fill:#3a2a05,stroke:#fbbf24,stroke-width:2px,color:#fbbf24
    classDef ondevice fill:#063312,stroke:#00e436,color:#00e436
    classDef pcside fill:#053946,stroke:#00e5ff,color:#00e5ff
    classDef transport fill:#3a0010,stroke:#ff004d,stroke-width:2px,color:#ff004d
    class wigle,wdgwars_csv,wdgwars_json destinations
    class s3a_bruce,s3a_piglet,s3a_payload,s3a_rjpayload ondevice
    class s2_w2w,s3b_sd,s4_muninn,s4_heimdall pcside
    class gungnir transport
```

## Reading the diagram

| Color | Meaning |
|---|---|
| Amber | Destination endpoints (WiGLE.net, wdgwars.pl) |
| Green | On-device upload — no PC step needed |
| Cyan | PC-side feeder — you upload from a computer |
| Pink | Shared transport library (gungnir handles HMAC + retry + cooldown for the signed-JSON path) |

## Notes

- **One source can feed two destinations.** A WiGLE Android export is accepted by both WiGLE.net (direct) and wdgwars.pl (via wigle-to-wdgwars).
- **Two upload paths on wdgwars.pl side.** Bulk Wi-Fi/BLE CSVs go to `/api/upload-csv` (multipart). ADS-B + MeshCore go to `/api/upload/` as signed JSON via gungnir.
- **The `/endpoint/*` mirror exists** as a Cloudflare-L7 bypass for clients that burst-POST. gungnir v0.1.2+ uses it by default. Hand-rolled clients should target `/endpoint/*` rather than `/api/*` if they POST in bursts.

For the full progression with hardware suggestions and skill prerequisites at each level, see the [Newcomer onramp](onramp.md). For the firmware × chip support matrix behind each tier, see the [Hardware survey](survey.md).

<script type="module">
  // GitHub Pages doesn't include Mermaid JS by default. This bootstrap finds
  // ```mermaid``` fenced code blocks (which Jekyll renders as
  // <pre><code class="language-mermaid">) and re-renders them as diagrams.
  // On the GitHub repo browser the script is stripped — GitHub's native
  // Markdown renderer already shows the diagram. So both contexts render.
  // Theme variables map onto the CRT brand palette so the diagram doesn't
  // look like a pastel sticker stuck on a black terminal.
  import mermaid from 'https://cdn.jsdelivr.net/npm/mermaid@10/dist/mermaid.esm.min.mjs';
  document.querySelectorAll('pre > code.language-mermaid').forEach((el) => {
    const div = document.createElement('div');
    div.className = 'mermaid';
    div.textContent = el.textContent;
    el.parentElement.replaceWith(div);
  });
  mermaid.initialize({
    startOnLoad: true,
    theme: 'base',
    themeVariables: {
      background: 'transparent',
      primaryColor: 'rgba(0,229,255,0.06)',
      primaryBorderColor: '#00e5ff',
      primaryTextColor: '#e0e0e0',
      secondaryColor: 'rgba(0,229,255,0.04)',
      tertiaryColor: 'rgba(0,229,255,0.02)',
      lineColor: 'rgba(0,229,255,0.45)',
      edgeLabelBackground: '#000',
      mainBkg: 'rgba(0,229,255,0.06)',
      nodeBorder: '#00e5ff',
      clusterBkg: 'rgba(0,229,255,0.03)',
      clusterBorder: 'rgba(0,229,255,0.30)',
      fontFamily: '"Share Tech Mono", ui-monospace, Consolas, monospace',
      fontSize: '13px'
    }
  });
</script>
