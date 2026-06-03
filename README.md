# wdgo-onramp

A leveled onramp for new players coming to [WDGoWars](https://wdgwars.pl) — the community wardriving game by LOCOSP. Five steps from "I have an Android phone" to "I run a multi-source capture lab," with every product fact cited from a primary source.

Sits next to the sibling feeders ([adsb-to-wdgwars](https://github.com/HiroAlleyCat/adsb-to-wdgwars), [meshcore-to-wdgwars](https://github.com/HiroAlleyCat/meshcore-to-wdgwars), [wigle-to-wdgwars](https://github.com/HiroAlleyCat/wigle-to-wdgwars), [gungnir](https://github.com/HiroAlleyCat/gungnir)). The feeders are the tools; this repo is the orientation.

## Two ways to read it

**On the web** → [GitHub Pages site](https://hiroalleycat.github.io/wdgo-onramp/) — vanilla markdown, Mermaid flow diagram, no Obsidian required.

**In Obsidian** → clone this repo and open the root as an Obsidian vault. The `.canvas` flow diagram is interactive, internal `[[wikilinks]]` resolve.

```bash
git clone https://github.com/HiroAlleyCat/wdgo-onramp.git
# then in Obsidian: Open folder as vault → select wdgo-onramp/
```

## Contents

| File | What it is |
|---|---|
| [wdgo-newcomer-progression.md](wdgo-newcomer-progression.md) | The five-step onramp. Start here. |
| [wdgo-capture-flow.canvas](wdgo-capture-flow.canvas) | The same progression as an Obsidian canvas flow diagram. |
| [wardriving-hardware-survey.md](wardriving-hardware-survey.md) | Reference: full firmware × chip support matrix, all WDGoWars community tools, decision tree, API gotchas. |
| [wdgo-feeder-spec.md](wdgo-feeder-spec.md) | Language-agnostic implementation spec for the signed-JSON envelope, with a verified golden test vector. For people writing their own feeder. |

The `docs/` directory contains vanilla-markdown versions of the same content, served as a static site via GitHub Pages. The `flow.md` page renders the flow diagram as Mermaid for browsers that don't support `.canvas`.

## What this is not

- **Not affiliated with LOCOSP.** WDGoWars is theirs. This is a community-maintained orientation guide.
- **Not a tutorial in using the feeders themselves.** Each feeder's own README is the source of truth for its CLI; this guide tells you when you'd reach for which one.
- **Not a complete API reference.** That lives in [wdgwars.pl/help](https://wdgwars.pl/help/). This repo covers the gotchas working integrations trip over, not the surface itself.

## License

[MIT](LICENSE) — use freely, no warranty.

## Contributing

Issues + pull requests welcome, especially:
- A product fact that's drifted (a version bumped, a release retired, a price changed)
- A community tool that should be listed in the survey
- A gotcha worth adding to the newcomer-pitfalls section

Please cite a primary source (release page, repo commit, official wiki) in any pull request that adds or modifies a claim.
