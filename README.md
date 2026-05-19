# The unreasonable effectiveness of HTML — examples

> **Sample code. Not maintained and not accepting contributions.**

A gallery of standalone HTML examples that accompany the blog post on using HTML
as a flexible output format. Each file is a self-contained `.html` page (no build
step, no dependencies) demonstrating a different use case — from code review and
design systems to slide decks, status reports, and small interactive editors.

Open [`index.html`](index.html) for the full, categorized index, or open any
numbered file directly in a browser.

## Contents

| Category | Examples |
|---|---|
| Exploration | code approaches, visual designs |
| Code | review, understanding, design systems, component variants |
| Prototyping | animation, interaction |
| Communication | slide deck, status report, incident report, PR write-up |
| Diagrams & research | flowchart, feature/concept explainers |
| Custom editing UIs | triage board, feature flags, prompt tuner |

## Running

There is nothing to install or build. Clone the repo and open `index.html` (or
any individual file) in a web browser.

## Using these templates as a Claude Code skill

This repo also ships a [Claude Code](https://claude.com/claude-code) skill at
`.claude/skills/html-output/` that turns the gallery into a reusable output
format. When Claude is asked to "review this PR and export to HTML", "draft an
incident postmortem", "write up a status report", etc., the skill activates,
picks the matching template, and produces a single self-contained `.html` file
styled like the rest of the gallery.

The skill auto-loads for any Claude Code session inside this repo. To use it
from anywhere, copy the folder to your user skills directory:

```sh
cp -r .claude/skills/html-output ~/.claude/skills/
```

## A note on sample data

All product names, data, and scenarios in these examples are fictional and used
only for illustration. The placeholder brand "Acme" and any figures shown are
not real.

## Security

See [SECURITY.md](SECURITY.md) for how to report a vulnerability.

## License

Released under the [Apache License 2.0](LICENSE).
