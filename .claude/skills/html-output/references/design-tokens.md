# Shared design tokens

Every template in `templates/` uses the same palette and type system. When generating output, declare these tokens in `:root` and reuse them — don't invent new colors or fonts mid-document.

## Palette

```css
:root {
  /* Surfaces */
  --ivory:    #FAF9F5;  /* page background */
  --paper:    #FFFFFF;  /* cards, panels */
  --oat:      #E3DACC;  /* subtle fills, avatars */

  /* Text */
  --slate:    #141413;  /* primary text, headings */
  --gray-700: #3D3D3A;  /* body text */
  --gray-500: #87867F;  /* secondary / meta */
  --gray-300: #D1CFC5;  /* borders */
  --gray-200: #E6E3DA;  /* light borders, dividers */
  --gray-150: #F0EEE6;  /* hover, table stripes */
  --gray-100: #F0EEE6;  /* alias used in some files */

  /* Accents */
  --clay:     #D97757;  /* primary accent, links, eyebrows */
  --clay-d:   #B85C3E;  /* clay pressed/dark */
  --olive:    #788C5D;  /* success, additions, positive */
  --rust:     #B04A3F;  /* warning, deletions, danger */
}
```

## Typography

```css
:root {
  --serif: ui-serif, Georgia, "Times New Roman", Times, serif;
  --sans:  system-ui, -apple-system, "Segoe UI", Roboto, Helvetica, Arial, sans-serif;
  --mono:  ui-monospace, "SF Mono", Menlo, Monaco, Consolas, monospace;
}
```

**Role conventions:**
- **Serif** — page title (h1) and major section titles. Weight 500 (never bold). Letter-spacing slightly negative for large sizes.
- **Sans** — body, UI chrome, buttons, table cells.
- **Mono** — eyebrows (uppercase 12px with 0.12em letter-spacing), IDs, code, file paths, dates, tag labels.

**Sizes (rough scale):**
- Hero h1: `clamp(38px, 5.4vw, 62px)`, line-height ~1.06
- Doc h1: 28–32px serif weight 500
- Section h2: 18–22px serif or sans (depends on density)
- Body: 15–16.5px sans
- Meta / eyebrow: 12–13px mono

## Layout

- Page background: `var(--ivory)`
- Content max-width: 920–1120px depending on density (reports tighter, galleries wider)
- Padding: `48px 24px 80px` for documents, `80px 0 56px` masthead style for landing pages
- Cards: `1.5px solid var(--gray-300)`, `border-radius: 12px`, background `var(--paper)`, internal padding 24–32px

## Visual rules

- Hairlines are **1.5px**, not 1px, almost everywhere — gives the warm-paper feel.
- Border-radius is **10–12px** on cards, **6–8px** on inline chips/buttons. Never sharp corners, rarely pill-shaped.
- Use color sparingly: a single clay underline or olive dot does more work than a filled-in button. Most surfaces stay ivory/paper.
- Diff red/green uses `--rust` / `--olive`, not pure red/green.
- Avoid drop shadows. Use borders + subtle background tints instead.
- Avoid gradients except where a template explicitly uses them (rare).

## When the user wants a different palette

If the user explicitly asks for "dark mode", "their brand colors", "blue theme", etc., adapt the tokens but **keep the structural rules** (hairline widths, border-radius, serif/sans/mono roles, layout density). The look-and-feel comes from the system, not just the colors.
