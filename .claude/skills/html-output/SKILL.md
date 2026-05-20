---
name: html-output
description: Produces a polished, self-contained .html file styled in a warm ivory/clay/oat palette for documents like PR reviews, code walkthroughs, status reports, incident reports, design system pages, slide decks, implementation plans, research explainers, flowcharts, and small interactive editor UIs. Use whenever the user asks to export, render, save, write up, or produce output as HTML for any of those document types — including phrases like "review this PR and export to HTML", "as an HTML report", "save the writeup as html", "make me a status page", "draft an incident postmortem", "explain this concept", "implementation plan", "slide deck". Also use when the user asks for a "nice-looking" or "shareable" version of work that fits one of the bundled artifact types. The skill ships 20 reference templates plus a design-token sheet; pick the matching template, copy its `<style>` block and structure, swap placeholder content for the real task output, and write one standalone .html file with no external dependencies.
---

# HTML output

A library of 20 reference templates that demonstrate HTML as a flexible output format. When a user wants the result of some other task (a PR review, a status update, a postmortem) rendered as a standalone webpage, this skill points you at the right template, gives you the design system, and tells you how to produce a single self-contained `.html` file that looks like it belongs in the same family.

The templates live in `templates/` next to this file. They are full, populated examples — not stripped-down skeletons. Treat them as **styling and structure references**, not literal templates: read the one that matches the artifact, copy the `<style>` block essentially as-is, mirror the section structure, and replace the placeholder content (Acme, PR #247, etc.) with the actual task output.

## Pick the template

| Artifact the user wants | Template |
|---|---|
| Compare 2–4 code approaches side by side | `01-exploration-code-approaches.html` |
| Compare 2–4 visual design directions | `02-exploration-visual-designs.html` |
| **PR review / code review summary** | `03-code-review-pr.html` |
| Walkthrough of how some code works (auth flow, request lifecycle) | `04-code-understanding.html` |
| Design system reference page (tokens, components) | `05-design-system.html` |
| Component variant matrix (states, sizes, themes) | `06-component-variants.html` |
| Animation / micro-interaction prototype | `07-prototype-animation.html` |
| Interaction prototype (drag, hover, focus flow) | `08-prototype-interaction.html` |
| **Slide deck / weekly update slides** | `09-slide-deck.html` |
| SVG illustrations (header art, hero graphics) | `10-svg-illustrations.html` |
| **Weekly / sprint status report** | `11-status-report.html` |
| **Incident report / postmortem** | `12-incident-report.html` |
| Flowchart with annotations (pipelines, state machines) | `13-flowchart-diagram.html` |
| Explainer for a product feature | `14-research-feature-explainer.html` |
| Explainer for a CS / engineering concept | `15-research-concept-explainer.html` |
| **Implementation plan / technical design doc** | `16-implementation-plan.html` |
| **PR write-up / PR description for a finished change** | `17-pr-writeup.html` |
| Triage / kanban-style board | `18-editor-triage-board.html` |
| Feature flag editor / config UI | `19-editor-feature-flags.html` |
| Prompt-tuning / LLM-evaluation editor | `20-editor-prompt-tuner.html` |
| **Implementation notes — running log of decisions, deviations, tradeoffs, and open questions captured while implementing a spec** | `21-implementation-notes.html` |
| **Spike report — investigation of tech debt or a new feature, comparing 2–4 options with effort estimates, timeline, and a recommendation** | `22-spike-report.html` |
| *…any of the above, in **dark mode*** (user asked for dark) | keep the chosen template; swap to the **Dark variant** tokens → see [Dark mode](#dark-mode) |
| *…any of the above, **printed / saved as PDF*** | the 10 document templates already carry the print block → see [Print / PDF](#print--pdf) |

The last two rows are **modifiers**, not separate picks: choose a template by structure first, then layer dark mode and/or print on top per those sections.

If the request straddles two categories, pick the one whose **structure** matches best, not the one whose subject matter matches. A "code review of PR #312" is a code review (03), even if it's about a frontend feature. A "writeup of PR #312 I just merged" is a PR writeup (17). Code review = feedback before merge; PR writeup = explaining a finished change.

The spec-implementation chain has four stages: **spike (22)** is the *investigation* — comparing options, estimating effort, and arriving at a recommendation, written *before* a direction has been chosen; **implementation plan (16)** is the design doc with mockups, written once a direction is picked but *before* code; **implementation notes (21)** are kept *during* the build as a running log of decisions/tradeoffs/open questions; **PR writeup (17)** explains what shipped *after*. If the user says "investigate how we should X" or "compare options for X with effort estimates" → 22. If they say "plan how we'd build X" → 16. If they say "implement X and keep a running log of decisions" → 21. If they say "write up what we just merged" → 17.

Note that 22 *converges* (here's what I recommend) while `01-exploration-code-approaches` *diverges* (here are options to react to, no verdict). If the user wants a recommendation, effort estimate, or timeline, pick 22 — not 01.

If nothing fits well, pick the closest structural match and adapt — don't force a poor fit. Common adaptation: `11-status-report.html` is a strong fallback for any "summary of recent work" artifact.

## Workflow

1. **Identify the artifact type** from the user's request and pick a template from the table above.
2. **Read the template file** with the Read tool. You need both the `<style>` block (the look) and the body markup (the structure — section ordering, what gets a card, what gets a table, where the eyebrow labels sit).
3. **(Optional) Read `references/design-tokens.md`** if you need a quick refresher on the palette/typography without re-reading a template just for the tokens.
4. **Generate one self-contained `.html` file:**
   - Copy the template's `<style>` block essentially verbatim. Only add new rules if the real content needs a section type the template didn't have.
   - Mirror the section structure. Keep the same heading hierarchy, the same use of cards vs. tables vs. inline blocks, the same eyebrow labels.
   - Replace placeholder content (Acme, fictional PR numbers, made-up names) with the real content from the task. Do not invent data — if a piece of information isn't available, omit that section or leave a clearly-marked TODO rather than fabricating it.
   - Keep it a **single file**. No external CSS, no CDN scripts, no `<img src="https://...">`. Inline SVGs are fine. Inline data: URIs are fine. The output should open correctly when double-clicked, with no network.
   - Use the design tokens from `references/design-tokens.md`. Don't introduce a new palette unless the user asked for one.
5. **Pick a filename** that reflects the artifact. Good: `pr-247-review.html`, `incident-2026-05-19-postmortem.html`, `auth-walkthrough.html`. Bad: `output.html`, `report.html`.
6. **Write the file** to the user's working directory (or wherever they specified).
7. **Tell the user** where the file is and what they can do with it (open in browser, share as an attachment, etc.). Don't dump the full HTML into the chat — the file is the deliverable.

## What "use the template" means in practice

The templates are real, polished pages. The job is to produce something that looks like it belongs in the same gallery, not a generic webpage with the palette applied.

- **Copy the structure, not just the styles.** If template 03 puts the PR title in a serif h1 inside a bordered card with a mono "repo · branch" line above and an author chip below, your output should follow that exact layout — that's the whole point. Generic Bootstrap-shaped output styled with these colors is not the deliverable.
- **Keep the density right.** These templates are information-dense without feeling cramped — comfortable line-height, generous padding inside cards, hairline borders. Don't space everything out into a marketing page, and don't cram things together.
- **Voice matches the artifact.** Status reports are matter-of-fact. Incident reports are blameless and chronological. PR reviews are direct and specific. Match the tone of the template, not just the visuals.
- **Length matches the content, not the template.** If the template has 12 sections and the real work only justifies 4, ship 4 well-built sections. Don't pad to match the template's length.

## Dark mode

Artifacts default to **light**. Produce a dark version **only when the user explicitly asks** ("dark dashboard", "dark deck", a dark screenshot to match, etc.) — never pre-emptively. When they do:

- Use the **Dark variant** token values from `references/design-tokens.md` in `:root` instead of the light ones. Token *names* stay identical, so the rest of the template's CSS is unchanged — it's a palette swap, not a redesign.
- Add `color-scheme: dark;` to `:root` **and** `<meta name="color-scheme" content="dark">` in `<head>`. This declares the mode so the file is self-evident to a later reader, human or agent, without parsing CSS.
- Keep everything else identical to the light version: same structure, 1.5px hairlines, radius, serif/sans/mono roles, sparing color.

`reports/dark-palette-preview.html` is a rendered reference for the dark tokens in context.

## Print / PDF

Document artifacts include the `@media print` block from `references/design-tokens.md`, so copying a document template's `<style>` carries print support automatically — the 10 document templates (03, 04, 11, 12, 14, 15, 16, 17, 21, 22) already have it. When generating, tag any screen-only chrome you add with `.no-print` (and pinned bars with `.no-print-sticky`). Editor (18–20) and prototype (07–08) templates do not get it — printing them is meaningless.

## When the user wants something the templates don't cover

If the user asks for an artifact type that isn't in the table (e.g., a job posting, a recipe, a contract summary):

- Pick the template with the closest **structural** shape — usually `11-status-report.html` (sectioned document), `14-research-feature-explainer.html` (long-form prose with sidebars), or `05-design-system.html` (reference page with many small panels).
- Tell the user briefly which template you adapted from and why, in case they'd rather you reach for a different one.

Do not invent a new visual language. The whole value of this skill is consistency with the gallery.

## Output checklist

Before declaring the output done, verify:

- [ ] One single `.html` file, self-contained (no external requests when opened).
- [ ] Uses the shared palette tokens (`--ivory`, `--slate`, `--clay`, etc.) from `references/design-tokens.md`.
- [ ] If the user asked for dark mode: `:root` sets `color-scheme: dark`, `<head>` has `<meta name="color-scheme" content="dark">`, and only Dark variant token values are used.
- [ ] Uses the serif/sans/mono trio in their conventional roles (serif headings, sans body, mono for IDs/code/eyebrows).
- [ ] Real content has replaced all placeholder names, IDs, dates, and figures.
- [ ] Page renders correctly when opened directly in a browser (no missing fonts triggering layout shift — system fonts only).
- [ ] Filename is descriptive of the artifact, not generic.
- [ ] For a document artifact: the @media print block is present and any screen-only chrome is tagged .no-print.
