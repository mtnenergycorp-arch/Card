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
- **High-Low, 2 points per hole.** One point for the team with the lower *best*
  net score (Low), one for the team with the lower *worst* net score (High).
  Ties push.
- **Money per hole.** Set a default bet, or change the bet on any individual
  hole. The running **Total** shows who owes whom.
- **Editable course data.** Front-nine par / stroke index / yardage (Black
  tees) are pre-filled from public scorecards; the back nine is seeded with
  defaults — tap any Par / SI / Yds cell to correct it against the clubhouse
  card.

## How High-Low works

Each hole is worth two points:

| Point | Won by the team with the… |
|-------|----------------------------|
| **Low**  | lower of the two teams' *best* net scores |
| **High** | lower of the two teams' *worst* net scores |

A hole can swing −2, −1, 0, +1 or +2 points. The dollar move = net points ×
that hole's bet. The running total is shown from Team 1's perspective.

## Tech

A single, dependency-free `index.html` (HTML + CSS + vanilla JS). See
[`CLAUDE.md`](CLAUDE.md) for the internal structure.
