# wdgwars-onramp

A leveled onramp for new players coming to [WDGWars](https://wdgwars.pl) — the community wardriving game by LOCOSP. Five steps from "I have an Android phone" to "I run a multi-source capture lab," with every product fact cited from a primary source.

Sits next to the sibling feeders ([adsb-to-wdgwars](https://github.com/Yggdrasil-AI-labs/adsb-to-wdgwars), [meshcore-to-wdgwars](https://github.com/Yggdrasil-AI-labs/meshcore-to-wdgwars), [wigle-to-wdgwars](https://github.com/Yggdrasil-AI-labs/wigle-to-wdgwars), [gungnir](https://github.com/Yggdrasil-AI-labs/gungnir)). The feeders are the tools; this repo is the orientation.

## Two ways to read it

**On the web** → [GitHub Pages site](https://hiroalleycat.github.io/wdgwars-onramp/) — styled to match the [Muninn](https://github.com/Yggdrasil-AI-labs/adsb-to-wdgwars) and [Heimdall](https://github.com/Yggdrasil-AI-labs/meshcore-to-wdgwars) sibling sites (Orbitron + Share Tech Mono, CRT scanlines), Mermaid flow diagram, no Obsidian required.

**In Obsidian** → clone this repo and open the root as an Obsidian vault. The `.canvas` flow diagram is interactive, internal `[[wikilinks]]` resolve.

```bash
git clone https://github.com/HiroAlleyCat/wdgwars-onramp.git
# then in Obsidian: Open folder as vault → select wdgwars-onramp/
```

## Contents

| File | What it is |
|---|---|
| [wdgwars-newcomer-progression.md](wdgwars-newcomer-progression.md) | The five-step onramp. Start here. |
| [wdgwars-capture-flow.canvas](wdgwars-capture-flow.canvas) | The same progression as an Obsidian canvas flow diagram. |
| [shopping-list.md](shopping-list.md) | Buyer's checklist for each tier — hardware, software, and where to get it. Vendor links, no fabricated prices. |
| [wardriving-hardware-survey.md](wardriving-hardware-survey.md) | Reference: full firmware × chip support matrix, the wider firmware catalog (§3e — everything that wardrives, including the firmwares with no WDGWars uploader), maturity signals for the long-running repos (§10), all WDGWars community tools, decision tree, API gotchas. |
| [CREDITS.md](CREDITS.md) | Acknowledgments for the maintainers, projects, and vendors that make this whole ecosystem possible. Also the comprehensive repo + vendor index, the YouTube channel and video list, and the shops. |

For developers writing a feeder in a new language, the signed-JSON envelope shape is implemented in [gungnir](https://github.com/Yggdrasil-AI-labs/gungnir) (Python) and mirrored by LOCOSP's reference uploader at [`plugins/wardrive_upload.py`](https://github.com/LOCOSP/WatchDogsGo/blob/main/plugins/wardrive_upload.py) in [WatchDogsGo](https://github.com/LOCOSP/WatchDogsGo). Read both — between them they cover the auth header, HMAC construction, retry/cooldown, and the slot-typed payload shape.

The `docs/` directory contains vanilla-markdown versions of the same content, served as a static site via GitHub Pages. The `flow.md` page renders the flow diagram as Mermaid for browsers that don't support `.canvas`.

## What this is not

- **Not affiliated with LOCOSP.** WDGWars is theirs. This is a community-maintained orientation guide.
- **Not a tutorial in using the feeders themselves.** Each feeder's own README is the source of truth for its CLI; this guide tells you when you'd reach for which one.
- **Not a complete API reference.** That lives in [wdgwars.pl/help](https://wdgwars.pl/help/). This repo covers the gotchas working integrations trip over, not the surface itself.

## License

[MIT](LICENSE) — use freely, no warranty.

## Contributing

Issues + pull requests welcome, especially:
- A product fact that's drifted (a version bumped, a release retired, a price changed)
- A community tool that should be listed in the survey
- A gotcha worth adding to the newcomer-pitfalls section

See [CONTRIBUTING.md](CONTRIBUTING.md) for the full guide — what's welcome, what's not, primary-source citation rules, the sync process for the dual-flavor (Obsidian + vanilla) docs, and the dual-format (canvas + Mermaid) diagram.
