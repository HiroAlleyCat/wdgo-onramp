---
title: wdgo-onramp
description: A leveled guide for new wardrivers — from WiGLE on your phone to advanced multi-source capture
---

# WDGoWars Onramp

A leveled guide for new players coming to [WDGoWars](https://wdgwars.pl), the community wardriving game by LOCOSP. Five steps from "I have an Android phone" to "I run a multi-source capture lab," with every product fact cited from a primary source.

## Read order

1. **[Newcomer onramp](onramp.md)** — start here. Five sequenced steps from zero to scoring on WDGoWars leaderboards.
2. **[Capture flow diagram](flow.md)** — the same progression as a flow diagram (Mermaid here, interactive Obsidian canvas in the repo root).
3. **[Shopping list](shopping.md)** — buyer's checklist for each tier with vendor links.
4. **[Hardware survey](survey.md)** — reference: every firmware × chip combination, every community tool, decision tree, API gotchas.
5. **[Credits & acknowledgments](credits.md)** — every maintainer, project, and vendor in the ecosystem this guide leans on.

## Who this is for

- **New players** asking "what hardware should I buy?" → start at [Newcomer onramp](onramp.md) Step 1, then [Shopping list](shopping.md) for the specific items.
- **Players moving past their phone** → jump to [Newcomer onramp](onramp.md) Step 3 and [Shopping list](shopping.md) Tier 3a/3b.
- **People comparing firmwares** → [Hardware survey](survey.md) §2-§4 has the matrix.
- **Developers writing a feeder** → read [gungnir](https://github.com/HiroAlleyCat/gungnir) (Python transport) and LOCOSP's [WatchDogsGo `plugins/wardrive_upload.py`](https://github.com/LOCOSP/WatchDogsGo/blob/main/plugins/wardrive_upload.py) side by side — between them they cover the auth header, HMAC construction, retry/cooldown, and the slot-typed payload shape.
- **Maintainers and crediting people** → [Credits](credits.md) for who built what.

## What this is not

- Not affiliated with LOCOSP. WDGoWars is theirs. This is a community-maintained orientation guide.
- Not a CLI tutorial for the feeders themselves. Each feeder's own README is the source of truth for its options.
- Not a complete API reference — that lives at [wdgwars.pl/help](https://wdgwars.pl/help/). This guide covers the gotchas working integrations trip over, not the surface itself.

## Source and contributions

[Source on GitHub](https://github.com/HiroAlleyCat/wdgo-onramp). Issues and pull requests welcome — especially for product facts that have drifted (versions, retired releases, changed behavior) or new community tools worth listing.

[MIT licensed](https://github.com/HiroAlleyCat/wdgo-onramp/blob/main/LICENSE).
