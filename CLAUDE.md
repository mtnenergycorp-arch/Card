# CLAUDE.md

Guidance for Claude Code (and other AI assistants) working in this repository.

## Repository overview

- **Name:** `card`
- **Owner:** `mtnenergycorp-arch`
- **What it is:** An interactive **High-Low golf betting scorecard** for
  **Yellowstone Country Club** (Billings, Montana). It tracks two to six players
  on two teams, applies match-play net scoring (low handicap plays off scratch),
  takes a per-hole money bet with presses, and shows a running dollar total of
  who is up and who is down. It also includes **WAD**, a separate individual
  putting side-game.

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
  yardage, **Blue tees**, par 72, 72.2/145), verified from the club's published
  men's Blue rating (31-Mar-2026). Editable inline. Bump `COURSE_VERSION` when
  these change so `load()` refreshes the course on already-saved rounds.
- **`betSchedule(base)`** — builds the per-hole bet array: front nine at the
  base stake, back nine auto-pressed to the **hole-9 value + 1** (the automatic
  press on hole 10), floored at **$2**. Default base stake is **$1**. Used by
  `defaultState`, the base-bet input, and "New round".
- **`defaultRoster()` / `state.roster` / `renderRoster()`** — the saved “Canes”
  roster (Cody, Chris, Darrin, Pat, Bryan), each `{name, hcp}`. A collapsible
  **Roster panel** (`#rosterEditor`) edits names/indexes directly
  (`data-rname` / `data-rhcp` / `data-rdel`, `＋ Add player to roster`);
  changing an index live-updates any matching player in the lineup. Each player
  slot also has a quick-pick `<select>` (`data-pick`, `change` listener) that
  fills the name + remembered index. The roster persists across rounds and
  survives "Reset everything".
- **`defaultState()` / `state`** — the full app state (2–6 `players` each with
  `{name, hcp, team}`, team names, per-hole bets, scores, course data). Persisted
  to `localStorage` under the key `yccHighLow_v2`; `load()` migrates older saves
  (adds `team`, aligns each score row to the player count, and refreshes the
  course when `courseVersion` is stale).
- **`playingHcaps()`** — converts course handicaps to match-play strokes: the
  lowest index in the group plays off scratch and everyone else gets the
  difference. `calc()` and `recompute()` use these (not raw handicaps) for nets
  and stroke marks.
- **`strokesFor(hcp, si)`** — allocates (non-negative) playing-handicap strokes
  to a hole by stroke index. Supports handicaps above 18 (multiple strokes).
- **`addPlayer()` / `removePlayer(idx)`** — grow/shrink the field (2–6), keeping
  every hole's score row aligned; new players auto-balance to the smaller team.
  `addPlayer()` prompts for a name, remembers a brand-new name in the roster,
  and pulls a saved handicap when the typed name matches a roster entry.
- **`calc()`** — the scoring engine. Computes net scores, the four point
  categories (low / high / greenie / net birdie), the sweep doubling, money
  won/lost, and the running total.
- **Greenie** — a tap-to-cycle cell (none → each player → none) via a delegated
  `click` on `[data-greenie]`; `state.greenie[hole]` holds the awarded **player
  index** and `calc()` credits that player's team. Old team-based saves are
  converted once via the `greenieByPlayer` flag.
- **Rounds & ledger (`state.archive`, `roundResults()`, `archiveRound()`,
  `ledger()` / `renderHistory()`)** — “Finish & archive” snapshots the round's
  per-player money (each player carries the full team Hi-Lo result; the WAD
  winner collects the full pot from every other player). The **History & career
  ledger** panel totals each person across rounds. `ledger()` recomputes each
  archived round's money from its stored facts under the *current* rules
  (`roundLedgerRows`), so settlement-rule changes apply retroactively; a live
  **Current round** preview (`renderCurrentRound`, refreshed in `recompute`)
  shows the in-progress round before it's archived, and the archive button
  guards against double-archiving.
  `roundSummaryText()` + `shareText()` drive “Share round” (Web Share API →
  clipboard → prompt); `exportData()` / `importData()` back up or restore the
  full `state` as JSON.
- **`renderCard()` / `recompute()`** — DOM rendering. The scorecard is the
  **classic grid** (holes across the top, OUT/IN/TOT columns, players down a
  sticky left column under editable team-name bands). `renderCard()` builds the
  inputs once; `recompute()` only fills the computed cells (per-nine totals,
  stroke highlights, nets, points, money, running total) so typing never loses
  focus. A single delegated `input` listener keyed off `data-*` drives edits;
  player/team **name** edits update state + summary only (no rebuild) to keep
  the focused field alive.
- **`advanceScore(hole, pl)`** — after a score is typed, moves the cursor to the
  next player's box on that hole (then to the next hole). Digits 2–9 advance
  immediately; a leading "1" waits ~450ms (via `advTimer`) so 10–12 can be typed.

- **WAD (`state.wad`, `renderWad()`)** — a separate individual putting game:
  the pot starts at **$5 or $10** and grows **$1 per 8′ "wad" putt**.
  `wad.log` is the chronological list of `{p, hole}` entries (player index +
  the hole it happened on, set via the `#wadHole` picker / `state.wad.hole`);
  the pot is `start + log.length`, the holder is the last valid entry, and that
  holder wins the pot. Each player's wad holes and the full order are displayed.
  Controls: `data-wad` (record a putt on the current hole), `data-wadstart`,
  `#wadHole`, and undo/reset buttons. The log is realigned in `removePlayer`
  and cleared on "New round".

- **Nassau (optional) (`state.nassau`, `nassauResults()`, `renderNassau()`)** —
  three **net** side bets (front / back / total), each **$5 or $10**, scored
  **team total** (teams compared by summed net), **team best-ball** (each team
  uses its best member's net) — both credit every player the full team result —
  or **individual** (lowest net collects each stake from every other player). A
  segment settles once it's fully scored (`netByNine`). The per-player money is
  folded into `roundResults` (a `nassau` field), so it flows to the current-round
  preview, career ledger, and share text.
- **Net-by-nine** — the **Results** summary shows each player's and team's
  **Net F / Net B / Net** (front, back, total net) from `netByNine()`, plus a
  **Net (full)** reference column — net off each player's *full* course handicap
  vs par for holes played (`netByNine(fullHcaps)`), a reference only, not used
  for scoring.

## How the game is scored

- **Teams:** each player carries a `team` (0 = Team 1, 1 = Team 2); `team(p)`
  reads it. 2–6 players, switchable in the card, so teams may be uneven — Low/High
  use each team's best/worst net regardless of size.
- **Net (low man off scratch):** the lowest handicap plays off 0; the others
  receive the difference, allocated by stroke index (`playingHcaps`).
- **Points per hole scale with team size:** each team's nets are sorted and
  compared **rank-by-rank**, one **ball** per rank worth 2 pts (2v2 = low/high,
  3v3 = low/middle/high; uneven teams fall back to low+high). Plus **Greenie**
  (1 pt, awarded in the `Greenie` row to a **player** — their team scores it) and
  **Net birdie** (1 pt, best net under par, head-to-head). So **2v2 = 6 points,
  3v3 = 8** (`balls`/`maxPts` in `calc()`). Ties push.
- **Sweep:** if one team wins every point outright (`p === maxPts`), the hole
  doubles (`maxPts*2` — 12 in 2v2, 16 in 3v3).
- **Money:** (Team 1 points − Team 2 points) × that hole's bet. The running
  **Total** is from Team 1's perspective (positive = Team 1 is up).
- **Presses:** raising a hole's bet cascades that stake forward to the end of
  the round (the `data-bet` handler writes `bets[i..17]`). The back nine
  **auto-presses to the hole-9 value + 1** (see `betSchedule`).
- A hole only counts once every player has a score (and both teams are
  non-empty). Greenie is the only manual input (a per-hole `state.greenie`
  value: `null`/`0`/`1`).

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
