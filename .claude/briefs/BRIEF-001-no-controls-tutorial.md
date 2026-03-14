# BRIEF-001: No Controls Tutorial or Instructions for First-Time Players

## Metadata
- **Filed by:** UX Tester Agent
- **Date:** 2026-03-14
- **Priority:** P1
- **Category:** [ONBOARDING]
- **Assigned to:** Coder
- **Related audit items:** A2, A5
- **Status:** OPEN

---

## Problem Statement
A first-time player has absolutely no way to learn the game controls. After clicking "New Game", the siren intro says "Prepare to intercept..." but never explains HOW to intercept. The player must discover that clicking/tapping fires interceptors entirely through trial and error — while missiles are already raining down on the city.

## Steps to Reproduce
1. Open the game for the first time
2. Click "New Game"
3. Observe the siren intro: "RED ALERT! RED ALERT!" / "Prepare to intercept..."
4. Wave 1 begins — missiles start falling
5. No instruction appears telling the player to click/tap to fire

## Expected Behavior
Before or during the first wave, the player should see a brief instruction such as:
- "Click to launch interceptors" (desktop)
- "Tap to launch interceptors" (mobile)
- Or a brief animated tutorial showing the aim-and-fire mechanic

## Actual Behavior
Zero instructions are provided anywhere — not in the menu, not in the siren intro, not in the HUD, not as a tooltip. The only hint is "Prepare to intercept..." which tells the player WHAT to do but not HOW.

## Evidence
- **Viewport:** Desktop 1280x800
- **Game state:** Siren intro → Wave 1
- **Language:** English
- **Observation:** Player was game-overed at wave 1 in both test runs — scored 0 in the first attempt, 8 in the second. A real first-time player would likely lose the first city before understanding the controls.

---

## Suggested Fix
1. Add a first-play overlay during wave 1 that says "Click to launch interceptors!" (desktop) or "Tap to launch interceptors!" (mobile)
2. Show this only on the first play session (check `localStorage` for a `tutorialSeen` flag)
3. The overlay should appear during or just after the siren intro, before missiles start spawning
4. Dismiss the overlay after the player's first successful fire, or after 5 seconds
5. Optional: Show a brief animated crosshair/click indicator pointing at a missile

## Impact Assessment
- **Players affected:** All first-time players
- **Frequency:** Every first session
- **Severity:** Frustrating — players may quit before understanding the game

## Related Decisions
None yet.

---

## Resolution (filled by designer/coder when resolved)
- **Resolved by:**
- **Date:**
- **What was done:**
- **Verified by UX Tester:** Pending
