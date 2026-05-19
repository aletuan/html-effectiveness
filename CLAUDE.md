# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this repo is

A gallery of 20 standalone HTML examples accompanying a blog post on using HTML as a flexible output format. The same files also ship as a Claude Code skill (`.claude/skills/html-output/`) so the gallery can be reused as a template library.

The repo is marked **"Sample code. Not maintained and not accepting contributions."** in the README — treat changes as illustrative rather than ongoing product work.

## Commands

There is no build system, no package manager, no test runner, and no linter. Each `.html` file is the deliverable.

- **Preview a file:** `open <file>.html` (macOS) — opens in the default browser. No server needed.
- **Browse the gallery:** `open index.html`.

Anything that looks like it needs `npm install` or a dev server doesn't belong here.

## Repository structure — the spine

The repo has two halves that share content on purpose:

```
html-effectiveness/
├── index.html                          # categorized gallery — entry point
├── 01-…-20-*.html                      # the 20 self-contained example artifacts
├── README.md / LICENSE / SECURITY.md
└── .claude/skills/html-output/
    ├── SKILL.md                        # template-picker table + workflow
    ├── templates/                      # COPIES of the 20 numbered files
    └── references/design-tokens.md     # shared palette + typography
```

- The **top-level numbered files** are the human-facing gallery, linked from `index.html`.
- The **`templates/` copies** are what the `html-output` skill reads when Claude generates a new artifact.
- These two sets are duplicated **by design**. If you edit a numbered file at the top level, mirror the change to `.claude/skills/html-output/templates/` (or vice versa) — drift between them will degrade the skill silently.

## Hard rules for every `.html` file

These constraints define what makes a file "fit" in this repo. Violating them is the most common way to break the gallery's invariants:

1. **Self-contained.** No `<link>` to external CSS, no CDN scripts, no `<img src="https://…">`. Inline `<style>` block, inline SVG, inline data: URIs only. Opening the file directly in a browser must work with zero network requests.
2. **System fonts only.** Use the `--serif` / `--sans` / `--mono` stack from `references/design-tokens.md`. No webfonts — they trigger layout shift and break the "double-click to read" promise.
3. **Tokens at the top.** Every file declares the shared palette and type tokens in `:root` (`--ivory`, `--slate`, `--clay`, `--oat`, `--olive`, `--rust`, gray scale, font stacks). Don't invent new colors or fonts mid-document. See `.claude/skills/html-output/references/design-tokens.md` for the canonical set and the role conventions (serif headings, sans body, mono for eyebrows/IDs/code).
4. **License header.** Every file starts with `<!-- Copyright 2026 Anthropic PBC · SPDX-License-Identifier: Apache-2.0 -->`. Preserve it.
5. **Fictional sample data is the convention.** Product names ("Acme"), PR numbers, dates, metrics, and people are placeholders. Don't replace them with real-looking data that could be mistaken for facts.

## Visual rules worth knowing before editing

Pulled from `references/design-tokens.md` — these are what make the gallery feel coherent, and they're easy to violate accidentally:

- **Hairlines are 1.5px**, not 1px. Cards use `1.5px solid var(--gray-300)` with `border-radius: 10–12px`.
- **Color used sparingly.** A single clay underline or olive dot does more work than a filled button. Most surfaces stay ivory/paper.
- **Diff red/green is `--rust` / `--olive`**, not pure red/green.
- **No drop shadows, no gradients** — use borders and subtle background tints.
- **Serif weight is 500**, never bold. Letter-spacing slightly negative on large sizes.

## How the skill picks a template

`SKILL.md` contains a table mapping user intents to template filenames. Two distinctions matter:

- **Code review (03) vs. PR write-up (17):** review = feedback *before* merge; write-up = explaining a *finished* change.
- **Structure beats subject matter.** A "code review of a frontend PR" is still template 03, not a design template. Pick by document shape, not topic.

When something doesn't fit the table, `11-status-report.html` is the canonical fallback for any "summary of recent work."

## When generating a new artifact (e.g., via the skill)

The workflow lives in `SKILL.md`, but the things that go wrong most often:

- **Copy the `<style>` block essentially verbatim** from the chosen template. Only add new rules when the real content needs a section the template didn't have.
- **Mirror the section structure**, not just the colors. Generic Bootstrap-shaped markup styled with these tokens isn't the deliverable.
- **Don't fabricate data.** If a section's input is missing, omit it or leave a clearly-marked TODO — never invent numbers, dates, or names.
- **Length matches content, not template.** If the template has 12 sections and the work only justifies 4, ship 4.
- **Filename reflects the artifact** (`pr-247-review.html`, `incident-2026-05-19-postmortem.html`), not generic names like `output.html`.
