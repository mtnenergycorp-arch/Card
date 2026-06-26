# High-Low Scorecard — Yellowstone Country Club

An interactive **High-Low golf betting scorecard** for Yellowstone Country
Club (Billings, Montana). Tracks handicaps, the money bet on each hole, and a
running dollar total of who's up and who's down.

## Use it

Open [`index.html`](index.html) in any browser — phone, tablet, or laptop.
No install, no internet required after loading. Your round is saved
automatically on the device.

## What it does

- **4 players, 2 teams.** Enter names and each player's **course handicap**.
- **Net scoring by stroke index.** Strokes are given on the hardest holes
  automatically; a green net number means a stroke was received on that hole.
- **High-Low, 6 points per hole.** Low (2 pts) to the team with the lower
  *best* net, High (2 pts) to the team with the lower *worst* net, Greenie
  (1 pt, awarded by hand in the **Grn** column), and Net birdie (1 pt, best net
  under par). Ties push. Win all six outright and the hole **sweeps** —
  doubling to 12.
- **Money per hole.** Set a default bet, or change the bet on any individual
  hole. The dollar move = (Team 1 − Team 2 points) × bet; the running **Total**
  shows who owes whom.
- **Editable course data.** Front-nine par / stroke index / yardage (Black
  tees) are pre-filled from public scorecards; the back nine is seeded with
  defaults — tap any Par / SI / Yds cell to correct it against the clubhouse
  card.

## How High-Low works

Six points are in play on every hole:

| Point | Value | Won by the team with the… |
|-------|:-----:|----------------------------|
| **Low**        | 2 | lower of the two teams' *best* net scores |
| **High**       | 2 | lower of the two teams' *worst* net scores |
| **Greenie**    | 1 | closest to the pin (awarded by hand in the **Grn** column) |
| **Net birdie** | 1 | best net score under par (the other team's isn't) |

Ties push (no points). **Win all six outright and the hole sweeps — the total
doubles to 12.** The dollar move = (Team 1 points − Team 2 points) × that
hole's bet, and the running total is shown from Team 1's perspective.

## Tech

A single, dependency-free `index.html` (HTML + CSS + vanilla JS). See
[`CLAUDE.md`](CLAUDE.md) for the internal structure.
