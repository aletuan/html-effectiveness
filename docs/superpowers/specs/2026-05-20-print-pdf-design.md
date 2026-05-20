# Print / PDF support for the html-output skill — design

**Date:** 2026-05-20
**Status:** Approved (canonical block locked; template baking + SKILL.md guidance pending)
**Scope:** Print/PDF for document artifacts. This is **phase 2**, following the dark-mode work
(phase 1, PR #6). Independent of phase 1 — separate branch (`aletuan/add-print-styles`) and PR.

## Problem

The `html-output` artifacts are meant to be shared and read — including saved as PDF. But no
template carries a print stylesheet (verified: zero uses of `@media print` across the gallery).
"Save as PDF" today prints the warm ivory page background (wasted ink), keeps interactive chrome
(sticky bars, buttons) that is meaningless on paper, and lets cards and tables split across page
breaks. Generated document artifacts inherit all of this.

## Goals

- Document-shaped artifacts **"Save as PDF" cleanly**: white page, no interactive chrome, no
  mid-element page breaks.
- **Keep meaningful color** on paper (diff red/green, severity dots, accent) — high fidelity.
- Make print support **travel with the template `<style>` block** the skill copies verbatim, so
  newly-generated documents are print-ready without the agent reconstructing rules each time.
- Preserve every existing invariant (self-contained, system fonts, tokens in `:root`, license
  header, hairlines, sparing color).

## Non-goals

- **No print CSS on editor (`18`–`20`) or prototype (`07`–`08`) templates** — printing them is
  meaningless (their value is interaction + a copy button).
- **No slide-deck (`09`) one-slide-per-page treatment** — a distinct, more involved layout;
  deferred to a possible later phase.
- **No ink-economy B&W mode** — the chosen behavior keeps all color.

## Decisions (confirmed with user)

1. **Placement — bake into the document templates (Option A).** A single canonical `@media print`
   block lives in `design-tokens.md`; it is dropped into the `<style>` of the 10 document-shaped
   templates so "copy the style block verbatim" carries print support automatically. Rejected:
   convention-only (agent reconstructs each time, less reliable) and hybrid/one-file.
2. **Scope — the 10 document-shaped templates:** `03` code-review, `04` code-understanding,
   `11` status, `12` incident, `14`/`15` explainers, `16` plan, `17` writeup, `21` notes,
   `22` spike.
3. **Color — keep all color** (`print-color-adjust: exact`); only the page background drops to
   white. Highest fidelity, accepts higher ink use.

## The canonical block (locked in `design-tokens.md`)

```css
@media print {
  * { -webkit-print-color-adjust: exact; print-color-adjust: exact; }
  html, body { background: #fff; }
  @page { margin: 16mm 14mm; }

  .no-print { display: none !important; }
  button { display: none !important; }
  .no-print-sticky { position: static !important; }

  table, tr, thead, figure, pre, blockquote, svg { break-inside: avoid; }
  h1, h2, h3 { break-after: avoid; }
  p, li { orphans: 3; widows: 3; }

  a[href^="http"]::after {
    content: " (" attr(href) ")";
    font-size: 0.82em; color: var(--gray-500); word-break: break-all;
  }
}
```

Two utility hooks: **`.no-print`** (screen-only chrome — toolbars, buttons, demo controls, nav)
and **`.no-print-sticky`** (a pinned element that should print but flow inline).

`design-tokens.md` is the canonical source for this block; the copy above is a reference.

## Deliverables

### 1. Canonical Print block in `design-tokens.md` — DONE (this commit)

A "Print / PDF" section was added with the block above, the two hooks, the per-file integration
steps, and the dark-mode interaction note.

### 2. Bake the block into the 10 document templates — TO DO

For each of the 10 templates **and its `templates/` copy** (per the `CLAUDE.md` mirror rule):

1. Drop the canonical block into the `<style>` verbatim.
2. Append that file's card/panel class to the `break-inside: avoid` rule (e.g.
   `.stat-card`, `.chart-panel`, `.carryover` for the status report).
3. Tag screen-only chrome with `.no-print` and pinned bars with `.no-print-sticky` (only
   `04`, `12`, `14`, `15`, `17` have sticky/fixed; `14`, `15` have buttons).
4. If the file sets large body padding, reset it under print (`body { padding: 0 }`).

This is ~10 files × 2 (top-level + `templates/`) = ~20 edits.

### 3. `SKILL.md` guidance — TO DO

Short subsection: document artifacts include the print block (from `design-tokens.md`); tag
screen-only chrome `.no-print`; the block is already in the document templates so copying the
`<style>` carries it. Add an Output-checklist item: *for a document artifact, the print block
is present and screen-only chrome is tagged `.no-print`.*

### 4. Reference — DONE

`reports/print-preview-status-report.html` — a status-report build with the block plus a demo
`.no-print` toolbar and external links. Verified under emulated print media: toolbar/banner
hidden, page background white, external-link URLs appended, blocks don't split.

## Verification

Per integrated template, under emulated `media: print`:

- Page background is white; diff/severity/accent colors still render.
- Elements tagged `.no-print` (and all `button`s) are `display: none`; `.no-print-sticky`
  elements are `position: static`.
- The file's cards/tables/figures carry `break-inside: avoid`.
- External (`http`) links show their URL via `::after`.
- File remains self-contained with the license header intact.
- **Mirror parity:** the top-level file and its `templates/` copy are identical
  (`diff <top-level> .claude/skills/html-output/templates/<same>` is empty).

## Risks

- **Mirror drift.** Editing 20 files by hand risks the top-level / `templates/` copies diverging,
  which `CLAUDE.md` warns degrades the skill silently. Mitigation (default approach): integrate
  each top-level file, then copy it over its `templates/` pair so both are identical by
  construction — followed by a `diff` parity check per pair in the verification step.
- **Dark-artifact printing.** The `background: #fff` line assumes a light artifact. Captured in
  the design-tokens note: for a dark variant, drop/adjust that line and keep the dark surface.

## Out of scope / follow-up

- Slide deck (`09`) one-slide-per-page print layout.
- Reference/diagram pages (`01`, `02`, `05`, `06`, `10`, `13`) — could get the block later if
  print demand appears; excluded now to keep the change bounded.
