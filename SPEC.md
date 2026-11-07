# Tournament of AI Metaphors — Build Spec

*Think March Madness Meets Academic Analogies*

## How to use this file with Claude Code

Put this file and the `data/` folder in an empty project folder, open Claude Code there, and start with:

> Read SPEC.md. Build Phase 1 only, then stop so I can review it in the browser.

Then ask for one phase at a time (see **Build phases** at the bottom).

---

## 1. What this is

This is a single-page web app for a class final project. Each student submits a metaphor that explains AI. The metaphors compete in a single-elimination bracket. Students vote by hand each class (paper slips in a bucket), and the instructor types the totals into the site and publishes them live on the projector, which animates the results.

- **Output:** one `index.html` with all HTML, CSS and JS inline. The only exception is the `data/` folder of CSVs. No frameworks, no build step, no backend.
- **Hosting:** GitHub Pages. The page must work when opened from the repo URL.
- **Audience:** first-semester college students viewing on phones and laptops, and the instructor projecting in class.
- **No student names anywhere.** Not in the data, not on the page.

## 2. Data files

### `data/metaphors.csv`
```
id,bracket,metaphor,summary,clarifies,confuses,relationship
1,main,Metaphor 1,,,,
```
| Column | Meaning |
|---|---|
| `id` | A unique number. **Row order within a bracket = seed order** (first row = seed 1). |
| `bracket` | `main` or `bonus` (see §4). |
| `metaphor` | The metaphor's name or title. |
| `summary` | 2–3 sentence summary. |
| `clarifies` | How the metaphor clarifies what AI is. |
| `confuses` | How the metaphor could confuse or mislead. |
| `relationship` | What it implies about the human–AI relationship. |

- Blank text fields show "Explanation coming soon" in the pop-up.
- Parse properly quoted CSV. Fields will contain commas, quotes and line breaks. Write a small, correct parser, or inline a tiny one. Don't split on commas.

### `data/votes.csv`
```
id,bracket,round,votes,tiebreak_win
```
- There is one row per metaphor per matchup it played. Winners are **computed** by comparing the two vote counts in each matchup. Do not store bracket structure.
- `tiebreak_win` is blank normally. On a tie, the winner's row gets `yes`.
- A bye gets no row.

## 3. Main bracket rules

- The bracket is single elimination. The size is the next power of two ≥ the number of `main` entries, and byes go to the top seeds. Use standard seeding (1 vs 16, 8 vs 9, …).
- **When most of Round 1 would be byes** (for example 9 entries in a 16-slot bracket), draw it compactly as a **Play-In Round**: show only the real matchups feeding into an 8-slot bracket. Don't show a wall of empty bye slots.
- A metaphor with a bye shows "Advances on a bye," with no vote count.
- Round names are based on how many remain: Play-In / Round of 16 / Elite 8 / Final Four / Championship.
- The engine must work for any count from 2 to 32. Test with 2, 9, 13, 15, 16 and 17.

## 4. Late entries → Bonus Bracket

- Entries with `bracket = bonus` are late submissions. **They never join the main tournament.**
- If there are **2 or more** bonus entries, show a separate **Bonus Bracket** section below the main bracket. It uses the same rules and the same voting and publishing flow, but is visually distinct (for example a "BONUS STAGE" label and a different neon accent color).
- If there is **exactly 1** bonus entry, show it as a "Bonus Entry" showcase card. It's clickable and has no matchup.
- Late entries are accepted up to Round 2 only. The app doesn't need to enforce this, because the instructor controls the CSV.

## 5. Tone rule: no losers

The metaphors that don't advance are still good work, so the design must never frame them as losers.

- **Never** use "knocked out," "eliminated," "lost," red X's, strike-throughs, greyed-out or heavily dimmed entries.
- A metaphor that doesn't advance stays full color and clickable. The winner gets a glowing outline and an **"ADVANCES ▶"** banner as it slides to the next slot.
- Each metaphor earns a neutral badge for its finish: "Play-In," "Round of 16," "Elite 8," "Final Four," "Finalist," "Champion." The bonus bracket uses "Bonus Champion."

## 6. Visual direction: retro arcade × March Madness

- **Title:** "Tournament of AI Metaphors". **Subtitle:** "Think March Madness Meets Academic Analogies".
- **Arena look:** a dark navy/black background, neon court lines (cyan, magenta, yellow), subtle CRT scanlines and glow.
- **Type:** a pixel font (Press Start 2P from Google Fonts, falling back to monospace) for the title, round labels and scores only. All explanation text uses a clean, readable sans-serif at comfortable sizes. Pixel fonts are hard to read in paragraphs.
- **Start screen:** the title with a blinking **PRESS START**. Clicking it, or pressing Enter/Space, drops into the bracket with an animated entrance.
- **Bracket layout:** two halves feeding a center championship slot with a trophy. On narrow screens, switch to horizontally scrollable columns, one per round.
- **Vote counts:** LED scoreboard digits that tick upward when published.
- **Vote bars:** arcade "power meters" filling side by side. Never health bars.
- **Hover/focus on a metaphor:** light up its entire path through the bracket, segment by segment like a neon tube switching on. Other paths soften slightly but stay readable.
- **Click a metaphor:** opens a "character select" style pop-up card. It shows the metaphor name, the four explanation sections with clear headings, the finish badge, and a vote history row ("Play-In: 14–9 · Elite 8: 11–12"). It closes with an X, Escape, or a click outside.
- **Placeholders:** before the real file is uploaded, entries simply read "Metaphor 1," "Metaphor 2," …
- **Champion moment:** pixel confetti, a "NEW HIGH SCORE"–style fanfare banner, and the trophy glowing. Sound is off by default. If you add an optional sound toggle, make it muted to start.

### Accessibility (required)
- Honor `prefers-reduced-motion`: replace movement with simple fades and drop the confetti and scanlines.
- Every metaphor is a real `<button>`, keyboard-focusable. Focus triggers the same path highlight as hover.
- The pop-up is a proper dialog: focus moves into it, is trapped while open, and returns to the clicked metaphor afterward.
- Text contrast is at least WCAG AA against the dark background. Color is never the only signal (winners also get the banner or badge).
- Use proper heading structure: one `<h1>` for the title, `<h2>` for the Main and Bonus brackets, and so on.

## 7. End of tournament: Hall of Fame

- When the main championship is decided, show the Hall of Fame screen automatically after the champion animation.
- **Hall of Fame:** a grid with every metaphor (main and bonus) as an equal tile. Each shows its name and finish badge, and a click opens its pop-up card. The champion's tile can have a small crown, but every tile is the same size.
- A clearly visible toggle button switches between **Hall of Fame** and **Completed Bracket**, in both directions, at any time after the tournament ends, so the class can review either one later.

## 8. Scorekeeper mode (instructor only)

- This mode opens with the URL `?scorekeeper` (for example `…/index.html?scorekeeper`). Students on the normal URL never see the controls.
- The scorekeeper panel includes:
  1. **Upload metaphors CSV:** replaces the entries and resets the bracket after a confirmation.
  2. **Enter scores:** each matchup that is ready (both entries known, not yet published) shows two number inputs. Type the bucket counts. Nothing animates yet.
  3. **Tie handling:** if the two counts are equal, show "Tie! Pick the winner" with two buttons. That sets `tiebreak_win`.
  4. **Publish round:** animates all newly scored matchups for that round (count-up, power meters, winners advancing), one matchup after another so the class can watch each reveal. There should also be a "Publish this matchup" option per matchup for dramatic one-at-a-time reveals.
  5. **Undo last publish:** for typos made live in class.
  6. **Export votes.csv:** downloads the full votes file, ready to commit to the repo's `data/` folder.
  7. **Upload votes.csv:** an alternative to typing.
  8. **Reset to repo data:** clears local data and reloads `data/*.csv`.
- **Persistence:** scorekeeper changes are saved in `localStorage`, wrapped in try/catch, so a refresh on the classroom computer doesn't lose anything. Show a small "Using local scores on this computer" badge with the reset option whenever local data is in use.
- **Load order:** students' devices always use `data/metaphors.csv` + `data/votes.csv` from the repo. The scorekeeper uses localStorage if present, otherwise the repo files.

## 9. Instructor workflow (for reference)

1. Before Round 1, replace `data/metaphors.csv` with the real file (row order = seeds) and push.
2. In class, open `?scorekeeper`, type the bucket totals, and click **Publish**. Students watch it animate.
3. After class, click **Export votes.csv**, replace `data/votes.csv` in the repo, and push. Now everyone's link is current.
4. For late entries, add rows with `bracket = bonus` before Round 2, push, and upload them in scorekeeper mode.

## 10. Build phases

1. **Static bracket:** start screen, layout and visual style using `data/metaphors.csv` (15 placeholders). No voting yet.
2. **Bracket engine:** seeding, byes, compact play-in, round names, computing winners from `votes.csv`. Test with 2, 9, 13, 15, 16 and 17 entries.
3. **Pop-up card + hover path highlight**, with the accessibility requirements.
4. **Scorekeeper mode:** score entry, ties, publish, undo, export, upload, localStorage.
5. **Animations:** count-up, power meters, advancing, champion moment, reduced-motion fallbacks.
6. **Bonus bracket:** test with `data/test-metaphors-with-bonus.csv` (9 main + 3 bonus).
7. **Hall of Fame + bracket toggle.**
8. **Final check:** run a full mock tournament from start to champion in both brackets, on a phone-sized and a projector-sized screen, then publish to GitHub Pages.
