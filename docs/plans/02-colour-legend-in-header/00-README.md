# 02 — Colour legend in the header

Master plan: none yet (no `docs/PLAN.md`) · Builds on: [`../01-see-live-sessions/00-README.md`](../01-see-live-sessions/00-README.md) (same page, no shared decisions)

## Goal
- Anyone looking at the page sees what each avatar colour means without scrolling the left panel (today the legend is the last card, below the fold in your screenshot).
- The legend always matches the colours actually drawn on avatars.

## Decisions (already made; do not revisit)
| # | Decision |
| --- | --- |
| W1 | **Placement.** A slim always-visible strip `<div id="legendBar" class="legendbar" role="list">` directly after `</header>` and before `#banner` in `public/index.html`; full width, one row on desktop. No collapse or hide toggle. |
| W2 | **Chips.** Each chip is `<span role="listitem" tabindex="0" class="chip" title="<full sentence>">` holding a 10px coloured dot and a short label. The full sentence is also the chip's `aria-label`. Order and wording (state → label → title): `working` → "working" → "It is doing something right now"; `thinking` → "thinking" → "It is deciding what to do next"; `walking` → "walking in" → "It just arrived at a desk"; `blocked` → "needs you" → "It stopped and is waiting for an answer"; `done` → "finished" → "It wrote its report"; `idle` → "standing by" → "No news for a while — it is waiting for you to do something"; `error` → "problem" → "It stopped with a problem". The old "on a break" row is dropped (the `coffee` state is not what a quiet session shows). |
| W3 | **One source of colour.** Export `stateColor` is already in `public/iso.js`; the chips are built by a new `public/legend.js` (`export function renderLegend(host)`) that reads colours with `stateColor(state)`, so the hard-coded hex values in `index.html` disappear. `client.js` imports it and calls `renderLegend($('legendBar'))` once in `init()`. |
| W4 | **Left card.** In `index.html` the `<section class="card legend">` keeps only its `<p class="hint">` (the "Sprinting…" and "bar under each name" explanation), retitled `<h2>Reading the floor</h2>`; the colour `<ul>` is removed. The `.legend ul/li/i` rules in `styles.css` are replaced by the `.legendbar`/`.chip` rules. |
| W5 | **Look and responsiveness.** `.legendbar { display:flex; flex-wrap:wrap; gap:6px 14px; align-items:center }`, chips 11.5px, `color: var(--muted)`, dot `border-radius:3px`, a leading label "Colours:" in `var(--dim)`. Wraps onto more lines at narrow widths; never causes horizontal page scroll. Chip gets a visible focus outline. |
| W6 | **Out of scope.** Dorm bed colours ("on shift"), an in-canvas legend, hiding the strip, translations. |

## Status
- [ ] 01 — Header legend strip, `legend.js`, left-card trim, README note
- [ ] 02 — Acceptance

## Dependencies and parallel tracks
| Slice | Prerequisites | Parallel with | Model | Files touched |
| --- | --- | --- | --- | --- |
| 01 | none | none | Sonnet | `public/index.html`, `public/legend.js`, `public/client.js`, `public/styles.css`, `README.md` |
| 02 | 01 | none | Haiku | `docs/plans/02-colour-legend-in-header/` |

**Rules for every slice:** zero dependencies, no build step, plain words on the page (no "state", "hook", "session" in chip labels or titles).

## Acceptance checklist
- [ ] The strip with seven chips is visible under the header on load, with no scrolling, and no hex colours remain in `index.html`.
- [ ] Each chip's dot colour equals `stateColor(<its state>)` for that state.
- [ ] Hovering or focusing a chip shows its full sentence.
- [ ] The left panel no longer lists colours but still explains "sprinting" and the progress bar.
- [ ] At 800px width the strip wraps and the page has no horizontal scroll.
