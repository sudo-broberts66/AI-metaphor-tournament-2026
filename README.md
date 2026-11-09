# Tournament of AI Metaphors

*March Madness Meets Academic Analogies*

Interactive website for tracking the final project for XIDS2002: Your Brain and AI during the Fall 2026 semester at the University of West Georgia. As the final project for the course,
students are challenged with picking and explaining a metaphor that could be used to help someone else understand Generative AI.

A single-page bracket site for updating and following the class final project. Students' metaphors for AI compete in a
single-elimination bracket. Votes are cast on paper each class; the instructor types the totals
into **scorekeeper mode** and publishes them live on the projector.

The site and paper ballots are built using Claude Code, with of course a lot of planning and prompting. 
Paper Ballots are designed by Claude Code using the Canva mcp in order to rapidly 
design and print for voting at the beginning of each class.

- Everything is in `index.html` (no build step, no backend). Data lives in `data/`.
- Hosted on GitHub Pages.
- No student names anywhere. Not in the data, not on the page.

## Addresses

| Who | Address | What it does |
|---|---|---|
| Students | `https://<user>.github.io/<repo>/` | Bracket (or Hall of Fame once the main final is decided). Uses the repo's `data/` files. |
| Instructor | `…/index.html?scorekeeper` | Same page plus the scorekeeper panel. Saves its work on that computer. |
| Practice | `…/index.html?test=9&bonus=3&scorekeeper` | Placeholder entries; nothing is saved. Add `&played=all` (or `&played=2`) to fill in rounds with made-up votes. |

## Instructor workflow

1. **Before Round 1:** replace `data/metaphors.csv` with the real file and push. Row order = seed
   order (first `main` row is seed 1).
2. **In class:** open `?scorekeeper`, press start, type each bucket count, then
   **Publish this matchup** (one at a time) or **Publish round** (all ready matchups, one after another).
   Equal counts ask *"Tie! Pick the winner."* **Undo last publish** puts the numbers back in the boxes.
3. **After class:** click **Export votes.csv**, replace `data/votes.csv` in the repo with it, and
   push. Now everyone's link is current.
4. **Late entries:** add rows with `bracket = bonus` before Round 2, push, and use
   **Upload metaphors CSV** in scorekeeper mode. Main-bracket scores are kept as long as the
   main entries didn't change.

The classroom computer keeps its scores in the browser (badge: *"Using local scores on this
computer"*). **Reset to repo data** clears that and reloads `data/*.csv`. Export first if you
want to keep the scores.

## Data files

### `data/metaphors.csv`

```
id,bracket,metaphor,summary,clarifies,confuses,relationship
1,main,AI as an Autocomplete on Steroids,"It predicts the next word…","Shows that…","Can make AI sound…","Positions the human as…"
```

- `bracket` is `main` or `bonus`. Bonus entries never join the main bracket: 2+ get their own
  Bonus Bracket, exactly 1 gets a showcase card.
- Fields may contain commas, quotes and line breaks if the field is wrapped in double quotes (Excel and
  Google Sheets do this when saving as CSV).
- Blank explanation fields show "Explanation coming soon".

### `data/votes.csv`

```
id,bracket,round,votes,tiebreak_win
8,main,Play-In,14,
9,main,Play-In,9,
```

- One row per metaphor per matchup it played; byes get no row.
- Winners are computed by comparing the two counts. On a tie, the winner's row has `yes` in
  `tiebreak_win`.
- `round` is the round name shown on the page (`Play-In`, `Round of 16`, `Elite 8`, `Final Four`,
  `Championship`; matched loosely) or a number (1 = first round shown).
- You normally never edit this by hand: scorekeeper mode exports it.

## Bracket rules (built in)

- Size = next power of two ≥ number of entries; byes go to the top seeds; standard seeding.
- When most first-round games would be byes (e.g. 9 entries), only the real games are drawn as a
  compact **Play-In**.
- Works for 2–32 entries (and beyond).
- Nobody "loses": every metaphor keeps full colour and earns a neutral finish badge
  (Play-In, Round of 16, Elite 8, Final Four, Finalist, Champion, Bonus Champion).

## Publishing on GitHub Pages

Repo **Settings → Pages → Build and deployment → Source: Deploy from a branch**, pick the branch
that holds `index.html` and the folder `/ (root)`, then **Save**. The site appears at
`https://<user>.github.io/<repo>/` within a minute or two. After each push, give it a minute and
refresh (Ctrl/Cmd + Shift + R).

## Accessibility

On phones and narrow windows the bracket shows one round at a time: swipe sideways, or use the
sticky **◀ / ▶** round bar (and its dots) to jump between rounds. The bar also works with the
keyboard (Tab to it, then ← / →) and announces the round to screen readers.

Keyboard-operable throughout (every metaphor is a button, focus shows its path), proper dialog
for the pop-up card (focus moves in, is trapped, and returns), heading structure, WCAG AA
contrast (checked with axe-core), and `prefers-reduced-motion` swaps movement, confetti and
scanlines for simple fades. Sound effects are off unless switched on in the scorekeeper panel.
