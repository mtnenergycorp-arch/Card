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
  yardage, Black tees). The **front nine is verified** from public scorecards;
  the **back nine is seeded with defaults** and is editable inline in the table.
- **`defaultState()` / `state`** — the full app state (players, handicaps, team
  names, per-hole bets, scores, course data). Persisted to `localStorage`
  under the key `yccHighLow_v1`.
- **`strokesFor(hcp, si)`** — allocates handicap strokes to a hole by stroke
  index. Supports course handicaps above 18 (multiple strokes) and plus
  handicaps (strokes given back from the easiest holes).
- **`calc()`** — the scoring engine. Computes net scores, the **Low** and
  **High** points per hole, money won/lost, and the running total.
- **`render*()` / `recompute()`** — DOM rendering. Inputs are built once;
  `recompute()` updates only the computed cells (nets, results, money,
  summary) so typing never loses focus. Driven by a single delegated `input`
  listener keyed off `data-*` attributes.

## How the game is scored

- **Teams:** players 1–2 = Team 1, players 3–4 = Team 2 (`team(p)` helper).
- **Two points per hole:** the **Low** point goes to the team with the lower
  *best* net score; the **High** point goes to the team with the lower *worst*
  net score. Ties push (no money).
- **Money:** net points on a hole × that hole's bet. The running **Total** is
  from Team 1's perspective (positive = Team 1 is up).
- A hole only counts once all four scores are entered.

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
