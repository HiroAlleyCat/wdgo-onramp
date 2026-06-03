---
title: WDGoWars feeder spec
description: Implementation specification for the signed-JSON envelope, with a verified golden test vector
---

# WDGoWars feeder specification (signed-JSON envelope)

> **Use [gungnir](https://github.com/HiroAlleyCat/gungnir) if you're writing in Python.** This spec exists for the cases where you can't — a Rust / Go / C / firmware-side feeder, or when you want to audit what gungnir is doing. The two reference implementations cited above produce byte-identical output for the same (payload, key, nonce) tuple.

## 1. Endpoint

| Use | URL |
|---|---|
| Bulk record upload (recommended — bypasses CF L7) | `POST https://wdgwars.pl/endpoint/upload/` |
| Bulk record upload (legacy — works but trips CF L7 on cold-IP bursts) | `POST https://wdgwars.pl/api/upload/` |
| Identity check | `GET https://wdgwars.pl/api/me` |
| WiGLE-CSV multipart bulk (different path entirely) | `POST https://wdgwars.pl/api/upload-csv` |

The `/endpoint/*` alias is a server-side PHP router that points at the same handler as `/api/*` but its URL pattern doesn't trip Cloudflare's automatic DDoS L7 rate limit. Both accept the same envelope and produce the same response. Source: [`gungnir/__init__.py`](https://github.com/HiroAlleyCat/gungnir/blob/main/gungnir/__init__.py) `DEFAULT_API_URL` comment block.

## 2. Required headers

| Header | Value | Why |
|---|---|---|
| `X-API-Key` | 64-char hex (the user's API key from `wdgwars.pl/profile`) | Server identifies the driver. |
| `User-Agent` | `<tool>/<version>` minimum, e.g. `mytool/1.0.0`. Optional `(+<repo-url>)` suffix. | Cloudflare Bot Fight Mode returns HTTP 403 + code 1010 for empty UAs and `python-requests/*`. Anything else passes. The WatchDogsGo plugin uses `WatchDogsGo/<ver> (<platform>; Python/<x.y>)`. |
| `Accept` | `application/json` | Server returns JSON; this avoids any negotiation drift. |
| `Content-Type` | `application/json` (when POSTing the envelope) | Standard. |

Do not use `python-requests/*` or send an empty `User-Agent` — Cloudflare drops both before they reach origin PHP. Source: comment block in [`plugins/wardrive_upload.py`](https://github.com/LOCOSP/WatchDogsGo/blob/main/plugins/wardrive_upload.py).

## 3. The envelope

Quoted verbatim from [`gungnir/envelope.py`](https://github.com/HiroAlleyCat/gungnir/blob/main/gungnir/envelope.py):

```
{
    "data":  base64(json(payload)),
    "nonce": hex(8 random bytes),           # 16 hex chars
    "sig":   hex(hmac_sha256(key, nonce + data_b64))
}
```

### 3.1 Build steps (language-agnostic)

```
1. Build the payload dict (see §4 for slot shapes).
2. body_json = json.dumps(payload, separators=(",", ":"))
   — COMPACT form, no whitespace, no trailing newline.
3. data_b64 = base64(utf8_bytes(body_json))
   — standard base64, NOT urlsafe variant.
4. nonce = hex(secure_random(8 bytes))
   — 8 raw bytes → 16 hex chars (lowercase).
5. sig_input = utf8_bytes(nonce + data_b64)
   — string concatenation, NOT base64 of concatenation.
6. sig = hex(hmac_sha256(key=utf8_bytes(api_key), msg=sig_input))
   — KEY IS THE RAW ASCII BYTES OF THE 64-CHAR HEX STRING, NOT THE
     UNHEX'D 32 BYTES. This is the #1 thing third-party feeders get
     wrong. Verify against gungnir/envelope.py:build_envelope().
7. body = json.dumps({"data": data_b64, "nonce": nonce, "sig": sig})
8. POST body to the endpoint with the headers from §2.
```

### 3.2 Reference Python (the source — DO NOT paraphrase, copy):

```python
def build_envelope(payload: dict, api_key: str, nonce: str | None = None) -> dict:
    body_json = json.dumps(payload, separators=(",", ":"))
    data_b64 = base64.b64encode(body_json.encode()).decode()
    if nonce is None:
        nonce = secrets.token_hex(8)
    sig = hmac.new(
        api_key.encode(),
        (nonce + data_b64).encode(),
        hashlib.sha256,
    ).hexdigest()
    return {"data": data_b64, "nonce": nonce, "sig": sig}
```

Source: [`gungnir/envelope.py`](https://github.com/HiroAlleyCat/gungnir/blob/main/gungnir/envelope.py).

### 3.3 Golden vector (verified 2026-06-02)

If your implementation produces this exact envelope for the inputs below, you match the canonical algorithm byte-for-byte. Pulled from [`tests/test_envelope.py::test_envelope_signature_matches_muninn_v1_11_1`](https://github.com/HiroAlleyCat/gungnir/blob/main/tests/test_envelope.py) and re-executed locally on 2026-06-02.

**Inputs:**

```
api_key = "test-key-12345678"
nonce   = "deadbeefcafebabe"
payload = {
    "networks":       [],
    "aircraft":       [{"icao": "A8A5DD", "lat": 42.0, "lon": -81.0}],
    "meshcore_nodes": []
}
```

**Intermediate (compact JSON of payload):**

```
{"networks":[],"aircraft":[{"icao":"A8A5DD","lat":42.0,"lon":-81.0}],"meshcore_nodes":[]}
```

**Expected envelope output:**

```json
{
  "data":  "eyJuZXR3b3JrcyI6W10sImFpcmNyYWZ0IjpbeyJpY2FvIjoiQThBNUREIiwibGF0Ijo0Mi4wLCJsb24iOi04MS4wfV0sIm1lc2hjb3JlX25vZGVzIjpbXX0=",
  "nonce": "deadbeefcafebabe",
  "sig":   "fde565e9a27ca0a8f42f7ba06b18ed95b8f18795d209ee7857fb9706fda334be"
}
```

If your `sig` doesn't match `fde565e9a27ca0a8f42f7ba06b18ed95b8f18795d209ee7857fb9706fda334be`, work backwards through these checks:

| Symptom | Likely cause |
|---|---|
| Your `data` differs from the expected | JSON serialization includes whitespace (use `separators=(",",":")` equivalent) OR key order differs (must be `networks`, `aircraft`, `meshcore_nodes` exactly) OR using URL-safe base64 instead of standard base64. |
| Your `data` matches but `sig` differs | HMAC key was unhex'd before use (must be raw ASCII bytes of the hex string), OR HMAC input was `data + nonce` not `nonce + data`, OR you signed bytes(payload) instead of bytes(data_b64). |
| `nonce` in your output is 16 chars but `sig` differs | Same key/order issue — re-check the order is exactly `nonce + data_b64`. |

The gungnir test suite also asserts this contract via `test_envelope_signature_matches_muninn_v1_11_1` — if it breaks, every deployed feeder's signatures stop matching what the server expects. That test is the canonical regression net.

## 4. Payload shape

The payload is a single JSON object with **three required keys**: `networks`, `aircraft`, `meshcore_nodes`. **All three must be present** — the server rejects payloads missing any of the three with a generic 400. Fill the slot your tool produces; pass `[]` for the others.

```python
{
    "networks":       [...],   # WiFi/BLE records, may be []
    "aircraft":       [...],   # ADS-B records, may be []
    "meshcore_nodes": [...],   # MeshCore LoRa nodes, may be []
}
```

Source: [`gungnir/envelope.py`](https://github.com/HiroAlleyCat/gungnir/blob/main/gungnir/envelope.py) `build_payload()` docstring.

### 4.1 Slot record shapes

Per-slot record shapes are the source of truth in the reference implementations — read them directly rather than transcribing here, because they evolve:

- **`networks` records:** see [`gungnir`](https://github.com/HiroAlleyCat/gungnir) consumers and the WatchDogsGo `_collect_networks()` function (`plugins/wardrive_upload.py`). WiFi auth strings are normalized via the `AUTH_MAP` dict (WPA3/SAE → "WPA3", WPA2-PSK → "WPA2", ESS/OPEN/empty → "OPEN", etc).
- **`aircraft` records:** see [Muninn parser](https://github.com/HiroAlleyCat/adsb-to-wdgwars/tree/main/muninn) for all supported source formats (AVR, SBS-1, dump1090, readsb, tar1090, VRS, Stratux, Mode-S Beast, NDJSON, Mayhem, GDL-90, CSV) and their normalized field set.
- **`meshcore_nodes` records:** see [Heimdall parser](https://github.com/HiroAlleyCat/meshcore-to-wdgwars).

> Don't invent fields. The server has strict acceptance on a handful of fields and silently drops the rest — see §6 on silent-drop detection.

## 5. Transport behavior (recommended defaults)

From [`gungnir/transport.py`](https://github.com/HiroAlleyCat/gungnir/blob/main/gungnir/transport.py):

| Behavior | Default | Reason |
|---|---|---|
| Request timeout | 120.0 s | Server can be slow on large chunks. |
| Whoami timeout | 30.0 s | Single small call. |
| Max retry attempts | 3 | Transient 5xx + network errors. |
| Backoff base | 2.0 s exponential | 2s → 4s → 8s. |
| Inter-chunk cooldown | 1.0 s | Stops back-to-back bursting on a 30-chunk batch. |
| On HTTP 429 | **Abort the whole batch.** Do NOT continue with more chunks. | Continuing deepens the server-side cooldown. Gungnir raises `BatchAborted` with `retry_after` if the server provided one. |
| On 5xx + network error | Retry with backoff up to max attempts | Standard transient handling. |
| On 4xx (other than 429) | Surface error to caller, do not retry | Client errors aren't fixed by retry. |

### 5.1 Chunking

The server accepts variable-size payloads but starts chunked uploads becoming necessary above a few hundred records. Specific chunk size is consumer-set, not server-enforced — Muninn and Heimdall both chunk for memory + cooldown reasons more than wire-protocol reasons. Use the `DEFAULT_CHUNK_COOLDOWN = 1.0s` between chunks.

## 6. Silent-drop detection

The server accepts a malformed payload with HTTP 200 and reports `0` records processed instead of returning 4xx. This is a known footgun — the gungnir library detects it and raises `SilentDrop`. Source: [`gungnir/diagnostics.py`](https://github.com/HiroAlleyCat/gungnir/blob/main/gungnir/diagnostics.py).

If you write your own client, after each POST compare what you sent with what the server reports it processed. A 200 with mismatched counts is a silent drop, not success.

## 7. Identity / whoami

`GET https://wdgwars.pl/api/me` with the `X-API-Key` header returns:
- the username + gang
- user stats
- earned badges (see `BADGE_GAME_TO_SERVER` in `plugins/wardrive_upload.py` for the badge-ID mapping)

Use this to validate a key before queuing any uploads. `/api/me` stays on `/api/*` not `/endpoint/*` (single-call, not affected by CF burst limits).

## 8. Things to test before declaring your feeder shipped

Field-tested checklist used for Muninn, Heimdall, and wigle-to-wdgwars:

- [ ] Build envelope for a known payload + known key + known nonce — assert byte-identical match against gungnir output. Use `tests/test_envelope.py` golden vectors as fixtures.
- [ ] All three payload slots present (even when empty).
- [ ] JSON serialized with `separators=(",",":")` — extra whitespace breaks the HMAC.
- [ ] User-Agent does NOT match `python-requests/*` and is not empty.
- [ ] Receives 200 with non-zero processed-count for a valid record — silent-drop is your first concern, not 4xx.
- [ ] Receives 429 → aborts whole batch, does not push the next chunk.
- [ ] Receives 5xx → retries with exponential backoff up to 3 attempts.
- [ ] API key is never logged, never serialized to disk in plaintext outside the user's intentionally-saved keyfile, never emitted in error messages. Use a `scrub()` helper (gungnir's [`keys.scrub()`](https://github.com/HiroAlleyCat/gungnir/blob/main/gungnir/keys.py) is the reference implementation).
- [ ] `--dry-run` mode exists and DOES NOT POST anything. Missing dry-run guards have caused at least one incident where a routine end-to-end test of a scheduler ended up uploading synthetic aircraft records to a real account. Treat the dry-run as a hard isolation, not a flag that "usually works."

## 9. Known feeder gaps (where new feeders could land)

Per the survey's §9 cross-link:

| Gap | What would close it |
|---|---|
| Marauder PC feeder | Read `apps_data/marauder/dumps/wardrive_*.txt` (WiGLE 1.4, 11 cols), pad to WigleWifi-1.6, multipart POST to `/api/upload-csv`. Mostly a `wigle-to-wdgwars` subcommand. |
| Bruce SD-batch replay ergonomics | Already addressable via wigle-to-wdgwars; the gap is ergonomics around "I have 20 dated CSVs, dedup + upload them all." |
| Bare ESP32-C3 capture with BSSID on stock firmware | Stock Marauder/Bruce/GhostESP don't solve this cleanly. Custom ESPHome probe firmware is the practical fit. |
| LoRa-meshtastic alternate format | Heimdall handles MeshMapper exports. If meshtastic-native exports differ in shape, a sibling parser would slot into Heimdall as another input source. |
| Browser-only WiGLE Android handoff | wigle-to-wdgwars CLI exists; a Pyodide build mirroring Muninn's pattern would let users drag a `.wiglecsv.gz` into a webpage and upload from there. |

## 10. Quick reference (cheat-sheet shape)

If you only read one section, read this:

```
URL:    POST https://wdgwars.pl/endpoint/upload/

Headers:
  X-API-Key:    <64-char hex>
  User-Agent:   <yourtool>/<version>
  Content-Type: application/json
  Accept:       application/json

Body:
  {
    "data":  base64(compact_json({"networks":[], "aircraft":[...], "meshcore_nodes":[]})),
    "nonce": 16-char-lowercase-hex(random 8 bytes),
    "sig":   lowercase-hex(hmac_sha256(api_key.utf8_bytes, (nonce + data).utf8_bytes))
  }

On 200 with mismatched counts → silent drop, not success.
On 429 → stop the entire batch, do not push more chunks.
On 5xx → exponential backoff retry, 3 attempts, base 2s.
```
