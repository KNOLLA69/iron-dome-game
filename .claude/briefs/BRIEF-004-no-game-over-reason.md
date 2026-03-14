# BRIEF-004: Game Over Screen Doesn't Explain Why the Player Lost

## Metadata
- **Filed by:** UX Tester Agent
- **Date:** 2026-03-14
- **Priority:** P2
- **Category:** [FEEDBACK] [EMOTION]
- **Assigned to:** Designer
- **Related audit items:** A7, I5
- **Status:** OPEN

---

## Problem Statement
When the game ends, the "GAME OVER" screen shows the player's score, wave reached, and max combo, but never explains WHY the game ended. A first-time player may not understand that the city's endurance was depleted, or even what the endurance bar represented during gameplay.

## Steps to Reproduce
1. Start a new game
2. Allow missiles to hit the city until game over
3. Observe the game over screen

## Expected Behavior
The game over screen should include context about why the game ended, such as:
- "Nahariya was destroyed!" (or whichever city was lost)
- A brief city endurance summary
- Something that connects the loss to the city defense mechanic

## Actual Behavior
Game over screen shows:
- "GAME OVER" (red text)
- Score (large number)
- "BEST: 202"
- "Wave: 1 | Max combo: 3"
- Rating ("Rookie", etc.)
- "Play Again" button + leaderboard icon

No indication of which city was lost or that city endurance was depleted.

## Evidence
- **Viewport:** Desktop 1280x800
- **Game state:** Game over after wave 1
- **Language:** English
- **Screenshots:** Game over screens captured with score 0 and score 8

---

## Suggested Fix
Add a line between the wave/combo stats and the rating that says something like:
- "Nahariya was destroyed" (using city name from the wave where the player lost)
- Or show the endurance bars for all cities attempted with their final state

## Impact Assessment
- **Players affected:** All players on every game over
- **Frequency:** Every game over
- **Severity:** Minor friction — player doesn't learn from the loss

## Related Decisions
None.

---

## Resolution
- **Resolved by:** Designer Agent
- **Date:** 2026-03-14
- **What was done:** Added "[CityName] was destroyed" in red (#ff6666) with glow between wave/combo stats and rating in drawGameOver(). Added i18n key `cityDestroyed` in all 3 languages. Shifted rating and buttons down to make room.
- **Verified by UX Tester:** Pending
- **Resolved by:**
- **Date:**
- **What was done:**
- **Verified by UX Tester:** Pending
