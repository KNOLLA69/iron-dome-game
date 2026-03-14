# BRIEF-002: "Wave 0" Displayed During Siren Intro Instead of "Wave 1"

## Metadata
- **Filed by:** UX Tester Agent
- **Date:** 2026-03-14
- **Priority:** P2
- **Category:** [HUD]
- **Assigned to:** Coder
- **Related audit items:** A3, C6
- **Status:** OPEN

---

## Problem Statement
When the game starts and the siren intro plays, the wave indicator at the top of the screen displays "Wave 0 - Nahariya" instead of "Wave 1 - Nahariya". This is confusing — there is no "Wave 0" in a game with waves 1-15.

## Steps to Reproduce
1. Start a new game
2. During the siren intro (red pulsing "RED ALERT! RED ALERT!" screen), look at the top-center HUD
3. Observe: "Wave 0 - Nahariya" is displayed

## Expected Behavior
The wave indicator should read "Wave 1 - Nahariya" during the siren intro for the first wave.

## Actual Behavior
Displays "Wave 0 - Nahariya". The wave counter appears to be zero-indexed during the siren, then increments to 1 when the actual wave starts.

## Evidence
- **Viewport:** Desktop 1280x800
- **Game state:** Siren intro (state=playing, sirenTimer=210, wave=0)
- **Language:** English
- **Screenshot:** Captured during siren phase showing "Wave 0 - Nahariya" in HUD

---

## Suggested Fix
The HUD wave display should show `wave + 1` during the siren phase, or the wave variable should be incremented before the siren starts rather than after it ends.

## Impact Assessment
- **Players affected:** All players
- **Frequency:** Every game start and potentially every new city siren
- **Severity:** Minor friction — confusing but not game-breaking

## Related Decisions
None.

---

## Resolution (filled by designer/coder when resolved)
- **Resolved by:** Designer Agent
- **Date:** 2026-03-14
- **What was done:** In drawHUD(), added `displayWave = waveState === 'siren_intro' ? wave + 1 : wave` so HUD shows "Wave 1" during siren intro instead of "Wave 0".
- **Verified by UX Tester:** Pending
