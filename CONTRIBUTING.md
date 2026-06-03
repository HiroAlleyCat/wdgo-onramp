# Contributing

Small repo. Practical guide. Read before opening an issue or PR.

## What's welcome

| Contribution | Notes |
|---|---|
| **Fact corrections** | A version bumped, a release retired, a chip family added, a price drifted. Must come with a primary-source link. |
| **New community tools** | Tools that demonstrably upload to wdgwars.pl. Link the actual upload code path, not just the README. |
| **Newcomer pitfalls** | Real footguns you watched someone hit. Specific, not theoretical. |
| **Diagram improvements** | Better layout, clearer labels, missing edges. |
| **Spelling, clarity, formatting** | Always welcome. |

## What's not

- "Add my tool" without a verifiable upload code path
- Stylistic rewrites with no content reason
- Translations (single-language for now)
- AI-generated content that hasn't been fact-checked
- Anything that adds emoji clutter

## Before opening a PR

### Fact changes

A primary source must be cited inline. Acceptable sources:

- A repo's release page URL (e.g. `https://github.com/X/Y/releases/tag/vN`)
- A specific commit or line-numbered code link
- An official wiki page
- A vendor product page (with a date stamp — prices drift)
- An RFC or standards doc

Not acceptable on their own:

- A blog post
- A forum thread
- A Discord screenshot
- "I remember reading"

If you have field-tested knowledge you can't cite, mark the affected line explicitly: *"field-tested but not citable from public docs."* The reader knows what they're trusting.

### New community tools

Open a PR adding the tool to [`wardriving-hardware-survey.md`](wardriving-hardware-survey.md) §3b (third-party feeders). Include:

| Required | Value |
|---|---|
| Repo URL | `https://github.com/...` |
| Maintainer | GitHub username |
| What it does | One sentence |
| Endpoint(s) hit | `/api/upload-csv`, `/api/upload/`, etc. |
| Code path linking to the upload | `link/to/the/file.py` |
| Last commit date | YYYY-MM-DD |
| Vetting depth | "README only", "code path verified", "driven end-to-end" |

If the tool runs on-device (capture firmware that uploads directly), it goes in §2 instead, with the same fields. If it's a HiroAlleyCat sibling repo, it goes in §3a.

### Diagram changes

The flow diagram exists in **two formats** that must stay in sync:

| File | Format | Rendered by |
|---|---|---|
| [`wdgo-capture-flow.canvas`](wdgo-capture-flow.canvas) | Obsidian Canvas (JSON) | Obsidian |
| [`docs/flow.md`](docs/flow.md) | Mermaid in a fenced code block | GitHub repo browser + GitHub Pages |

A change to one must include the matching change to the other. Diagrams drifting apart is worse than one being slightly outdated.

### Content changes

Each doc exists in **two flavors**:

| Root (Obsidian) | docs/ (vanilla) |
|---|---|
| [`wdgo-newcomer-progression.md`](wdgo-newcomer-progression.md) | [`docs/onramp.md`](docs/onramp.md) |
| [`shopping-list.md`](shopping-list.md) | [`docs/shopping.md`](docs/shopping.md) |
| [`wardriving-hardware-survey.md`](wardriving-hardware-survey.md) | [`docs/survey.md`](docs/survey.md) |
| [`CREDITS.md`](CREDITS.md) | [`docs/credits.md`](docs/credits.md) |

Process:

1. Edit the **root** file first (Obsidian flavor, `[[wikilinks]]` allowed).
2. Re-derive the `docs/` version by running this from the repo root. The script strips frontmatter AND prepends Jekyll-specific frontmatter (title + description) in one pass — fill in the title/desc per file:

   ```bash
   # title + description are colon-separated after the dst slug
   for pair in "wdgo-newcomer-progression:onramp:WDGoWars onramp:Five-step progression from WiGLE on your phone to advanced multi-source capture" \
               "shopping-list:shopping:Shopping list:Buyer's checklist for each tier of the WDGoWars onramp" \
               "wardriving-hardware-survey:survey:Hardware survey:Firmware × chip support matrix, community tools catalog, decision tree, API gotchas" \
               "CREDITS:credits:Credits & acknowledgments:The maintainers, projects, and vendors that make the WDGoWars ecosystem possible"; do
     IFS=: read -r src dst title desc <<< "$pair"
     out="docs/${dst}.md"
     { echo "---"; echo "title: ${title}"; echo "description: ${desc}"; echo "---"; echo; \
       awk '/^---$/{c++; next} c>=2' "${src}.md" | sed -E \
         -e 's|\[\[wdgo-newcomer-progression\]\]|[Newcomer onramp](onramp.md)|g' \
         -e 's|\[\[wardriving-hardware-survey\]\]|[Hardware survey](survey.md)|g' \
         -e 's|\[\[wdgo-capture-flow\]\]|[Capture flow diagram](flow.md)|g' \
         -e 's|\[\[shopping-list\]\]|[Shopping list](shopping.md)|g' \
         -e 's|\[\[CREDITS\]\]|[Credits](credits.md)|g'; \
     } > "$out"
   done
   ```

3. Commit both changes together.

Verify with `grep -rn '\[\[' docs/` after running — output should be `clean` (no leftover wikilinks).

## PR style

- **Subject line:** short, imperative. `Fix Marauder release tag`, not `Fixed the Marauder release tag in section 3`.
- **Body:** what changed + the primary-source link. One sentence is fine.
- **One concern per PR.** Easier to review, easier to revert if one piece is wrong.
- **No `Co-Authored-By: Claude` trailer.** This repo's commits stay attributed to the human author.
- **No force-pushes to `main`.** Open a PR from a branch.

## Issues

| Issue type | Include |
|---|---|
| Fact drift | What's wrong + the primary source showing the current truth |
| New community tool to list | Repo URL + the line where it uploads to wdgwars |
| Newcomer pitfall worth flagging | Specific scenario, not abstract |
| Diagram bug | Screenshot + which file (canvas, mermaid, or both) |

## Code of Conduct

Brief version:

- Be civil. The wardriving community is small enough that being a jerk burns visible bridges.
- Don't share API keys, location data, or other people's PII in issues, PRs, or commits.
- Don't open issues that boil down to "your tool isn't listed and that's an insult." This is a curated guide, not a directory.
- If you disagree with a curation choice, the conversation is welcome. Tone matters more than being right.

## Maintainer notes

If you're maintaining this repo (current maintainer or future):

- **Re-verify citations periodically.** Versions bump. Repos get archived. The frontmatter's `last-verified` date is the contract — if you touch a section, bump that date.
- **Treat the `survey.md` §A editorial-notes section as load-bearing.** It's the audit trail of what readers commonly get wrong. Don't silently delete entries when they get resolved — append the resolution.
- **Mermaid + canvas drift is the most common silent bug.** When editing the flow, do both at once.
