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
| Exploration | code approaches, visual designs, spike report |
| Code | review, understanding, design systems, component variants |
| Prototyping | animation, interaction |
| Communication | slide deck, status report, incident report, PR write-up, implementation notes |
| Diagrams & research | flowchart, feature/concept explainers |
| Custom editing UIs | triage board, feature flags, prompt tuner |

## Example prompts

Prompts that get good results from Claude Code, drawn from the
[accompanying blog post](https://claude.com/blog/using-claude-code-the-unreasonable-effectiveness-of-html).
Each maps to one or more templates in the gallery above. Use them as-is or
adapt the framing to your own task.

### Specs, planning, and exploration &rarr; `01`, `02`, `16`, `21`, `22`

> I'm not sure what direction to take the onboarding screen. Generate 6
> distinctly different approaches — vary layout, tone, and density — and lay
> them out as a single HTML file in a grid so I can compare them side by side.
> Label each with the tradeoff it's making.

> Create a thorough implementation plan in an HTML file. Make some mockups,
> show data flow, and add important code snippets I might want to review.
> Make it easy to read and digest.

The third doc in this group is `21-implementation-notes.html` — a *running
log* kept while building, so the spec → plan → implementation chain ends with
a document that captures judgment calls instead of hiding them in the diff.
Prompt (from [@trq212](https://x.com/trq212/status/2017024445244924382)):

> Implement &lt;SPEC&gt;. As you work maintain a running
> `implementation-notes.html` file that captures anything I should know about
> how the implementation diverges from or interprets the spec, including:
>
> - **Design decisions**: choices you made where the spec was ambiguous
> - **Deviations**: places where you intentionally departed from the spec, and
>   why
> - **Tradeoffs**: alternatives you considered and why you picked what you did
> - **Open questions**: anything you'd want me to confirm or revise

A concrete one-liner using the same pattern — useful for trying the template
on something small:

> Implement a small spec for an exponential-backoff helper and keep a running
> `implementation-notes.html` as you go.

Earlier in the chain, before you commit to a plan, sits `22-spike-report.html`
— the investigation that produces a recommendation. Where template 01 fans
out options to react to, a spike report **converges** to a recommendation
with effort estimates and a timeline. Sample prompt:

> I want to investigate how to deliver real-time updates for the activity
> feed. Look at polling, SSE, and WebSockets. For each: effort estimate,
> risk, timeline within Q2, pros/cons. End with a comparison matrix, a
> recommendation, and the open questions that still need to be resolved
> before we commit. Output as a single HTML spike report.

### Code review and understanding &rarr; `03`, `04`

> Help me review this PR by creating an HTML artifact that describes it. I'm
> not very familiar with the streaming/backpressure logic, so focus on that.
> Render the actual diff with inline margin annotations, color-code findings
> by severity, and whatever else might be needed to convey the concept well.

### Design and prototypes &rarr; `05`, `06`, `07`, `08`, `10`

> I want to prototype a new checkout button — when clicked it does a play
> animation and then turns purple quickly. Create an HTML file with several
> sliders and options for me to try different variants. Give me a copy button
> to copy the parameters that worked well.

### Reports, research, and learning &rarr; `09`, `11`, `12`, `13`, `14`, `15`, `17`

> I don't understand how our rate limiter actually works. Read the relevant
> code and produce a single HTML explainer page: a diagram of the token-bucket
> flow, the 3–4 key code snippets annotated, and a "gotchas" section at the
> bottom. Optimize it for someone reading it once.

### Custom editing interfaces &rarr; `18`, `19`, `20`

This category is the "throwaway editor" pattern: a single HTML file
purpose-built for **one** piece of data the user is wrangling — not a reusable
tool. The UI handles parts that are awkward to describe in text — dragging,
toggling, comparing visually — but the input/output between you and Claude
stays text. You stay in the loop at the step where human judgment matters,
then hand the result back to Claude as structured text and the loop continues.

> I need to reprioritize these 30 Linear tickets. Make me an HTML file with
> each ticket as a draggable card across Now / Next / Later / Cut columns.
> Pre-sort them by your best guess. Add a "copy as Markdown" button that
> exports the final ordering with a one-line rationale per bucket.

> Here's our feature flag config. Build a form-based editor for it. Group
> flags by area, show dependencies between them, and warn me if I enable a
> flag whose prerequisite is off. Add a "copy diff" button that gives me just
> the changed keys.

> I'm tuning this system prompt. Make a side-by-side editor: editable prompt
> on the left with the variable slots highlighted, three sample inputs on the
> right that re-render the filled template live. Add a character/token
> counter and a copy button.

The trick that ties the editor prompts together: end with an **export** — a
"copy as JSON" or "copy as prompt" button — so whatever you did in the UI
flows back to Claude Code or a file you can commit.

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
