# High-Low Scorecard — Yellowstone Country Club

An interactive **High-Low golf betting scorecard** for Yellowstone Country
Club (Billings, Montana). Tracks handicaps, the money bet on each hole, and a
running dollar total of who's up and who's down.

## Use it

Open [`index.html`](index.html) in any browser — phone, tablet, or laptop.
No install, no internet required after loading. Your round is saved
automatically on the device.

## What it does

- **2–6 players, two teams.** **＋ Add player** asks for the name (typed in, and
  saved to the roster); remove players (✕), switch anyone's team (⇄), and rename
  teams. Holes run across the top in the classic scorecard grid
  with OUT / IN / TOT columns; players sit down the left under the team bands.
  Entering a score **auto-advances** to the next player on that hole (then to the
  next hole), so you can rattle through scores without tapping each box.
- **Saved roster ("the Canes").** Open the **Roster** panel to set each player's
  name and handicap index once (Cody, Chris, Darrin, Pat, Bryan come pre-listed;
  add your own too). Then quick-pick a name from the dropdown in any player slot
  and it pulls in their index automatically (the dropdown even shows it, e.g.
  "Cody (8)"). The roster is remembered across rounds.
- **Net scoring — low man plays off scratch.** The lowest handicap in the group
  plays off 0 and everyone else gets the difference (a 5 and a 10 become 0 and
  5). Those strokes land on the hardest holes by stroke index — shown as gold
  cells with • dots; the small grey number in each box is the net score.
- **High-Low, 6 points per hole.** Low (2 pts) to the team with the lower
  *best* net, High (2 pts) to the team with the lower *worst* net, Greenie
  (1 pt, tap the **Greenie** row to award to the **player** who won it — their
  team scores the point), and Birdie (1 pt, a natural gross birdie — no strokes). Ties push.
  Win all six outright and the hole **sweeps** — doubling to 12.
- **Money, presses & auto-press.** Base stake defaults to **$1 per point**;
  raise the bet on any hole and the higher stake carries through the rest of the
  round. From hole 10 the stake **auto-presses to the hole-9 value + $1** (the
  back nine is always at least **$2**). The dollar move = (Team 1 − Team 2
  points) × that hole's bet; the running **Total** shows who owes whom.
- **Course data.** Blue tees, par 72, 72.2/145 — per-hole par, stroke index and
  yardage from the club's published men's Blue rating (31-Mar-2026). Edit any
  Yards / Par / S.I. cell if it ever changes.

## WAD (side putting game)

A separate, individual game tracked in its own panel. The pot **starts at $5 or
$10** and grows **$1 every time someone sinks an 8-foot putt** — that player
"takes the wad." Whoever holds the wad at the end (the **last** person to make a
wad putt) **wins the pot**. Pick the **Hole**, then tap **Wad! 8′** next to
whoever holed it — the hole is logged, so you can see **which hole each person
wadded on** (e.g. "Cody — 2 wads · H3, H12") plus the full order. The current
holder is highlighted, and there's **undo** and **reset**. Players come from the
scorecard lineup.

## Nassau (optional side game)

A classic **Nassau** you can switch on: three separate **net** bets — **front
nine, back nine, and all 18** — each set to **$5 or $10**. Score it **by team**
(**total** net or **best-ball**, i.e. each team's best member net; lower wins
the segment) or **individually** (the low-net player wins the segment). Each segment settles once it's fully
scored, and the money rolls into the results, ledger, and share summary. The
Nassau tab shows a **per-player win/loss table** — each player's Front, Back,
Total, and combined Nassau $ — plus the segment winners. The **Results** panel
also shows front / back / total **net** scores per player and team, plus a
**Net (full)** reference column — each player's net off their full course
handicap vs par — to see how close to par they finished.

## History, ledger & sharing

- **Share round.** In Results, tap **📤 Share round** to send a plain-text
  summary (final money, each player's Hi-Lo + WAD + total, the WAD winner) via
  your phone's share sheet, or copied to the clipboard.
- **Archive & career ledger.** Tap **🏁 Finish & archive** to save a finished
  round. The **History & career ledger** panel then shows a running **$ total
  per person** across every archived round. Each player carries the **full** team
  Hi-Lo result, and the **WAD winner collects the full pot from every other
  player**. A live **Current round** preview shows the in-progress totals before
  you archive, and the ledger reflects the current settlement rules even for
  previously-archived rounds. Each archived round can be re-shared or removed.
- **Export / import.** Back up everything — rounds, roster, ledger — as a JSON
  file (or copy it) and restore it on another device, or hand it over to review.

## How High-Low works

Points scale with team size. Each team's **net** scores are sorted and compared
**ball by ball** (rank by rank), each ball worth 2 points, plus a greenie and a
**natural** (gross) birdie:

| Point | Value | Won by the team with the… |
|-------|:-----:|----------------------------|
| **Low ball**    | 2 | lower *best* net |
| **Middle ball** | 2 | lower *middle* net — **3v3 only** |
| **High ball**   | 2 | lower *worst* net |
| **Greenie**     | 1 | player who got it (awarded in the **Greenie** row) |
| **Birdie**      | 1 | a **natural (gross) birdie** — no strokes (the other team has none) |

So a **2v2 hole is worth 6 points, a 3v3 hole is worth 8** (uneven teams use low
+ high only). Ties push. **Win every point outright and the hole sweeps —
doubling (12 in 2v2, 16 in 3v3).** The dollar move = (Team 1 − Team 2 points) ×
that hole's bet, from Team 1's perspective.

**Presses:** raise the bet on any hole and that higher stake stays for the rest
of the round. There's an automatic press on hole 10 — from there the stake is
the hole-9 value plus $1 per point.

**Net handicaps:** the lowest handicap in the group plays off scratch and
everyone else plays off the difference (so a 5 and a 10 become 0 and 5).

## Tech

A single, dependency-free `index.html` (HTML + CSS + vanilla JS). See
[`CLAUDE.md`](CLAUDE.md) for the internal structure.
