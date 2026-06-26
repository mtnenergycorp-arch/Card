# CLAUDE.md

Guidance for Claude Code (and other AI assistants) working in this repository.

## Repository overview

- **Name:** `card`
- **Owner:** `mtnenergycorp-arch`
- **What it is:** An interactive **High-Low golf betting scorecard** for
  **Yellowstone Country Club** (Billings, Montana). It tracks four players on
  two teams, applies each player's course handicap (net scoring by stroke
  index), takes a per-hole money bet, and shows a running dollar total of who
  is up and who is down.

## Codebase structure

This is a **zero-dependency, single-file static web app** — no build step, no
framework, no package manager.

```
.
├── index.html   # the entire app: HTML + CSS + vanilla JS in one file
├── README.md    # usage notes
└── CLAUDE.md    # this file
```

Everything lives in `index.html`:

- **`DEFAULT_COURSE`** — the 18-hole Yellowstone CC data (par / stroke index /
  yardage, **Blue tees**, par 72, 71.5/142). **Par and stroke index are
  tee-independent and the front nine is verified** from public scorecards;
  per-hole **Blue yardages are approximate** (not publicly retrievable) and are
  editable inline in the table.
- **`betSchedule(base)`** — builds the per-hole bet array: front nine at the
  base stake, back nine auto-pressed to **double** (the automatic press on
  hole 10). Used by `defaultState`, the base-bet input, and "New round".
- **`defaultState()` / `state`** — the full app state (players, handicaps, team
  names, per-hole bets, scores, course data). Persisted to `localStorage`
  under the key `yccHighLow_v1`.
- **`strokesFor(hcp, si)`** — allocates handicap strokes to a hole by stroke
  index. Supports course handicaps above 18 (multiple strokes) and plus
  handicaps (strokes given back from the easiest holes).
- **`calc()`** — the scoring engine. Computes net scores, the four point
  categories (low / high / greenie / net birdie), the sweep doubling, money
  won/lost, and the running total.
- **Greenie** — a tap-to-cycle cell (none → Team 1 → Team 2 → none) handled by
  a delegated `click` listener on `[data-greenie]`.
- **`renderCard()` / `recompute()`** — DOM rendering. The scorecard is the
  **classic grid** (holes across the top, OUT/IN/TOT columns, players down a
  sticky left column under editable team-name bands). `renderCard()` builds the
  inputs once; `recompute()` only fills the computed cells (per-nine totals,
  stroke highlights, nets, points, money, running total) so typing never loses
  focus. A single delegated `input` listener keyed off `data-*` drives edits;
  player/team **name** edits update state + summary only (no rebuild) to keep
  the focused field alive.

## How the game is scored

- **Teams:** players 1–2 = Team 1, players 3–4 = Team 2 (`team(p)` helper).
- **Six points per hole:** **Low** (2 pts, lower *best* net), **High** (2 pts,
  lower *worst* net), **Greenie** (1 pt, manually awarded in the `Grn` column),
  and **Net birdie** (1 pt, best net under par, head-to-head). Ties push, an
  unawarded greenie scores nothing.
- **Sweep:** if one team wins all six points outright, the hole doubles to
  **12** (`sweep` / `t1`/`t2` in `calc()`).
- **Money:** (Team 1 points − Team 2 points) × that hole's bet. The running
  **Total** is from Team 1's perspective (positive = Team 1 is up).
- **Presses:** raising a hole's bet cascades that stake forward to the end of
  the round (the `data-bet` handler writes `bets[i..17]`). The back nine
  **auto-presses to double** the front-nine base (see `betSchedule`).
- A hole only counts once all four scores are entered. Greenie is the only
  manual input (a per-hole `state.greenie` value: `null`/`0`/`1`).

## Development workflow

- **Run it:** open `index.html` directly in any browser — that's it. No server
  or install required. (To serve over HTTP for local testing:
  `python3 -m http.server` then visit the printed URL.)
- **No build / bundle / transpile step.**
- **No test suite or linter** is configured. When changing scoring logic,
  sanity-check `strokesFor`/`calc` by extracting the math into a small Node
  snippet (`node --check` validates the embedded script syntax).
- **Data correctness:** treat the back-nine course defaults as approximate.
  Front-nine par/SI/yardage are the verified values — keep them intact.

## Conventions

- **Vanilla everything.** No external libraries or CDNs; keep the app a single
  self-contained `index.html` that works offline.
- **Plain ES (`"use strict"`).** State is one mutable `state` object; persist
  via `save()` after mutations.
- **Re-render discipline:** update computed cells in `recompute()` rather than
  rebuilding inputs, to preserve input focus while typing on a phone.
- **Mobile-first:** this is used on the course on a phone — keep the layout
  responsive and the scorecard horizontally scrollable.
- Always escape user-entered strings rendered into HTML via `escapeHtml()`.

## Git & branch workflow

- **Develop on a feature branch.** Current working branch:
  `claude/claude-md-docs-nszxoz`. Branch off the default branch for new work.
- **Push with upstream tracking:** `git push -u origin <branch-name>`.
- **Never push to a different branch** than the one you were asked to work on
  without explicit permission.
- **Do not open a pull request** unless explicitly requested.
- Write clear, descriptive commit messages.

## Notes for AI assistants

- Keep this file current as the app grows. If the project gains a build step,
  tests, or splits into multiple files, update the structure and workflow
  sections to match.
- Prefer documenting things a newcomer couldn't easily discover over restating
  the obvious.
