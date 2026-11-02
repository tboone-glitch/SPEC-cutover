# Cutover: Game Spec

## Purpose
A small browser game that acts out Devin's legacy data migration use case. The player moves legacy warehouse tables to the cloud before the old platform's renewal date. Every migrated table has to reconcile before it counts.

## Gameplay
- The board is a 4x6 grid of 24 table tiles. Each tile shows a table name (for example CUSTOMER, ORDERS, INVOICES, CLAIMS, INVENTORY, GL_ENTRIES) and starts grey ("Legacy").
- The player clicks or taps a grey tile to dispatch a Devin.
  - The tile shows a blue progress ring for 1 second ("Converting").
  - Then it runs a reconciliation check:
    - 80% of the time it passes and turns green ("Migrated").
    - 20% of the time it turns amber ("Row counts don't match"). The player clicks it again to send a Devin to fix it: 1 second, then green.
- A tile with a Devin already working on it can't be clicked. Clicking a green tile does nothing and costs nothing.
- Each dispatch, including a fix, costs 1 ACU. The player starts with 32 ACUs.

## Clock and game over
- The round lasts 90 seconds, displayed as "Days to renewal", counting down from 90 to 0.
- "Cutover complete": all 24 tiles are green (win).
- "Renewal hit": time runs out first.
- "Budget exhausted": ACUs reach 0 with tiles still not green.

## Scoring (shown live at the top)
- Tables migrated (out of 24)
- ACUs spent
- Mismatches caught and fixed
- Days to renewal remaining

## End screen
- A headline with the game-over reason.
- The four scores. On a win, add: "Migrated 24 tables with [X] days to spare on [Y] ACUs."
- A "Play again" button that fully resets the game.

## Look and feel
- Dark background (#0f172a). Legacy tiles grey (#475569), converting blue (#3b82f6), migrated green (#22c55e), mismatch amber (#f59e0b).
- Title "Cutover" at the top. Clean, modern, readable at a glance.
- Works on desktop and phone: tap to dispatch, and the grid fits a phone screen without scrolling.

## Technical constraints
- One file: index.html with inline vanilla JavaScript and CSS.
- No frameworks, no build step, no backend, no external assets.
- Logic in clearly named functions (dispatchDevin, reconcile, fixMismatch, endGame) so the code is easy to review.

## Acceptance criteria
- [ ] Clicking a grey tile costs 1 ACU and converts it after 1 second.
- [ ] About 20% of conversions turn amber and can be fixed for 1 ACU.
- [ ] All three game-over conditions work and show the right headline.
- [ ] Scores update live and are correct on the end screen.
- [ ] "Play again" fully resets the game.
- [ ] Plays correctly in desktop Chrome and at phone width (375px).

## Process for Devin
1. Read this spec, show me your plan, and wait for my approval before writing code.
2. Build the game in the repo cutover.
3. Test it in your browser against every acceptance criterion, including phone width.
4. Open a pull request with a short summary, a screenshot and the acceptance checklist ticked.
5. After I merge, deploy to GitHub Pages and send me the link.

## Out of scope
- Sound, leaderboards, accounts, difficulty levels and table dependencies.
