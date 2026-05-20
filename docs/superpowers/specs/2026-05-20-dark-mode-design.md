# Dark mode for the html-output skill — design

**Date:** 2026-05-20
**Status:** Approved (palette locked; SKILL.md guidance pending)
**Scope:** Dark mode only. Print/PDF (`@media print`) is a separate follow-up spec.

## Problem

The `html-output` skill produces self-contained HTML artifacts in a warm ivory/clay/oat
palette. Every one of the 22 gallery files is light-only — no template uses
`prefers-color-scheme`, `@media print`, or `color-scheme` (verified). When a user wants a
dark artifact ("a dark dashboard", "dark slide deck"), there is no canonical dark palette,
so the agent would improvise dark values, producing inconsistent results across runs.

## Goals

- Give the skill a **single canonical dark palette** so dark artifacts look consistent.
- Make the chosen mode **self-declaring** so a later reader (human or agent) can tell which
  mode a file is in without parsing CSS.
- Preserve every existing invariant: self-contained, system fonts only, tokens in `:root`,
  license header, 1.5px hairlines, sparing color, no drop shadows/gradients.

## Non-goals

- **No `prefers-color-scheme` auto-switching.** Files do not adapt to the viewer's OS.
- **No in-page theme toggle / JavaScript.** Document templates stay pure HTML/CSS.
- **No conversion of the 22 gallery files to dark.** The gallery stays the canonical light set;
  dark is produced on demand only. This avoids touching 22 files (× 2 with skill copies) and
  the drift risk that carries.
- **No print/PDF work** in this spec — that is the agreed phase 2.

## Decision: fixed mode, generated on demand (Option C)

The gallery defaults to **light**. A dark artifact is generated **only when the user explicitly
asks**. Each file carries exactly one palette baked in at generation time. Token **names stay
identical** (`--ivory`, `--slate`, …); only the *values* change, so all structural CSS carries
over unchanged.

Options considered and rejected:

- **Auto (`prefers-color-scheme`)** — would require both palettes in every file via media
  queries; touches all 22 templates and adds maintenance surface. Rejected: more invasive than
  the need.
- **Auto + in-page toggle** — requires inline JS in the 19 document templates that are currently
  pure HTML/CSS. Rejected: breaks the no-JS character of the document templates; the choice
  doesn't round-trip anyway.

### How "which mode" is resolved (answers the driving question)

The mode is never inferred at runtime by guesswork. It is resolved at three distinct moments:

1. **Generation time** — the agent picks the mode from the user's request. Default light;
   explicit dark request → dark. This rule lives in `SKILL.md`.
2. **Render time** — there is no runtime decision; the single baked-in palette renders as-is.
3. **Read-back time** — the file self-declares via `<meta name="color-scheme" content="dark">`
   plus `:root { color-scheme: dark; }`. Any agent re-reading the file learns the mode from one
   tag, no CSS parsing.

## Deliverables

### 1. Canonical dark palette in `design-tokens.md` — DONE (this commit)

A "Dark variant" section was added to
`.claude/skills/html-output/references/design-tokens.md` with the full dark token set, the
3-step usage rule (dark tokens + `color-scheme` + meta tag, keep structure), and the diff-tint
guidance. The bottom "When the user wants a different palette" note now points dark-mode
requests at this canonical set. Locked values:

| Token | Dark | Light (was) | Role |
|---|---|---|---|
| `--ivory` | `#1A1916` | `#FAF9F5` | page background |
| `--paper` | `#24221E` | `#FFFFFF` | cards, panels |
| `--oat` | `#3A352D` | `#E3DACC` | subtle fills |
| `--slate` | `#ECEAE3` | `#141413` | primary text |
| `--gray-700` | `#C9C6BD` | `#3D3D3A` | body text |
| `--gray-500` | `#9C988D` | `#87867F` | secondary / meta |
| `--gray-300` | `#3D3A33` | `#D1CFC5` | borders |
| `--gray-200` | `#332F29` | `#E6E3DA` | dividers |
| `--gray-150` / `--gray-100` | `#211F1B` | `#F0EEE6` | hover, stripes |
| `--white` | `#24221E` | `#FFFFFF` | alias → paper |
| `--clay` | `#E08A6B` | `#D97757` | accent, links |
| `--clay-d` | `#C9714F` | `#B85C3E` | clay pressed |
| `--olive` | `#9DB07E` | `#788C5D` | success, additions |
| `--rust` | `#CE6B5B` | `#B04A3F` | danger, deletions |

Principles applied: surfaces are warm near-black (not pure black), text is warm off-white
(not pure white), accents keep their hue but are lifted ~12% for contrast on dark.

### 2. `SKILL.md` guidance — TO DO

Add a short subsection to `.claude/skills/html-output/SKILL.md` stating: default light; on an
explicit dark request, use the Dark variant tokens, set `color-scheme: dark` in `:root` and add
`<meta name="color-scheme" content="dark">`, and keep all structure/hairlines/roles identical to
the light version. Extend the Output checklist with a conditional item: *if dark was requested,
the file declares the color-scheme meta tag and uses only Dark variant token values.*

### 3. Reference example — DECIDED

The already-built `reports/dark-palette-preview.html` serves as the reference: it shows
swatches + an in-context card and demonstrates the meta-tag convention, giving the agent a
concrete dark page to copy rather than only a token table. No new file is added, and per
`CLAUDE.md` none goes at root (a stray unnumbered file reads as a 23rd template). If a fuller
example is wanted later, it lives in `reports/`.

## Verification

A generated dark artifact is correct when:

- It opens with zero network requests (self-contained), with the license header intact.
- `<head>` contains `<meta name="color-scheme" content="dark">` and `:root` sets
  `color-scheme: dark;`.
- It uses only the locked Dark variant token values — no improvised dark colors. Spot-check:
  `grep -o '#[0-9A-Fa-f]\{6\}'` against the Dark variant set.
- Structure, hairline widths, radius, and serif/sans/mono roles are identical to the light
  template it was based on.
- Body text on surface meets a comfortable contrast. The locked values were chosen to target
  ≥ ~7:1 for body on `--ivory`/`--paper` — this is a property of the palette, checked once at
  lock time, not a per-artifact gate.

## Risks

- **Drift with the light palette.** If the light tokens are ever re-tuned, the Dark variant must
  be re-derived. Mitigation: both sets live in the same `design-tokens.md`, side by side.
- **Agent skips the meta tag.** Mitigated by the Output-checklist item in deliverable 2.

## Out of scope / follow-up

- **Print/PDF (`@media print`)** — agreed phase 2, separate spec. Unlike dark mode it is
  orthogonal to palette and likely applies to all artifacts regardless of mode.
