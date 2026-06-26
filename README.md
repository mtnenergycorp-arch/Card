# High-Low Scorecard — Yellowstone Country Club

An interactive **High-Low golf betting scorecard** for Yellowstone Country
Club (Billings, Montana). Tracks handicaps, the money bet on each hole, and a
running dollar total of who's up and who's down.

## Use it

Open [`index.html`](index.html) in any browser — phone, tablet, or laptop.
No install, no internet required after loading. Your round is saved
automatically on the device.

## What it does

- **2–6 players, two teams.** Add or remove players (✕), switch anyone's team
  (⇄), and rename teams. Holes run across the top in the classic scorecard grid
  with OUT / IN / TOT columns; players sit down the left under the team bands.
- **Net scoring — low man plays off scratch.** The lowest handicap in the group
  plays off 0 and everyone else gets the difference (a 5 and a 10 become 0 and
  5). Those strokes land on the hardest holes by stroke index — shown as gold
  cells with • dots; the small grey number in each box is the net score.
- **High-Low, 6 points per hole.** Low (2 pts) to the team with the lower
  *best* net, High (2 pts) to the team with the lower *worst* net, Greenie
  (1 pt, tap the **Greenie** row to award — it shows the team's name), and Net
  birdie (1 pt, best net under par). Ties push. Win all six outright and the
  hole **sweeps** — doubling to 12.
- **Money, presses & auto-press.** Set a base bet; raise the bet on any hole
  and the higher stake carries through the rest of the round. From hole 10 the
  stake **auto-presses to the hole-9 value + $1**. The dollar move =
  (Team 1 − Team 2 points) × that hole's bet; the running **Total** shows who
  owes whom.
- **Editable course data.** Blue tees, par 72. Par and stroke index are
  verified for the front nine (and are the same from any tee); per-hole Blue
  yardages are approximate — edit any Yards / Par / S.I. cell to match the
  clubhouse card.

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

**Presses:** raise the bet on any hole and that higher stake stays for the rest
of the round. There's an automatic press on hole 10 — from there the stake is
the hole-9 value plus $1 per point.

**Net handicaps:** the lowest handicap in the group plays off scratch and
everyone else plays off the difference (so a 5 and a 10 become 0 and 5).

## Tech

A single, dependency-free `index.html` (HTML + CSS + vanilla JS). See
[`CLAUDE.md`](CLAUDE.md) for the internal structure.
