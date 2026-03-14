# BRIEF-003: "Out of Interceptors" Shown During Siren Intro Before Ammo Loads

## Metadata
- **Filed by:** UX Tester Agent
- **Date:** 2026-03-14
- **Priority:** P2
- **Category:** [HUD] [FEEDBACK]
- **Assigned to:** Coder
- **Related audit items:** B8, B9
- **Status:** OPEN

---

## Problem Statement
When the game starts and the siren intro plays, the bottom of the screen displays "Out of Interceptors" in red text and "Interceptors: 0/0" before any ammo has been loaded. This is misleading — a first-time player may think the game is broken or that they have no ammo to play with.

## Steps to Reproduce
1. Start a new game
2. During the siren intro, look at the bottom of the screen
3. Observe: "Out of Interceptors" in red text, "Interceptors: 0/0"

## Expected Behavior
During the siren intro, either:
- Hide the interceptor count entirely (it's not relevant yet)
- Show the ammo that WILL be available for the upcoming wave (e.g., "Interceptors: 60")
- Or at minimum, don't display the alarming red "Out of Interceptors" warning

## Actual Behavior
"Out of Interceptors" is displayed in red text at the bottom center, and "Interceptors: 0/0" at the bottom left. This appears from the moment the siren starts until ammo is actually loaded at wave start.

## Evidence
- **Viewport:** Desktop 1280x800
- **Game state:** Siren intro (state=playing, wave=0, ammo=0, sirenTimer=210)
- **Language:** English
- **Screenshot:** Captured showing red "Out of Interceptors" text during siren phase

---

## Suggested Fix
Don't render the "Out of Interceptors" warning or the ammo counter while `sirenTimer > 0` or `cityIntroTimer > 0`. Alternatively, pre-load the ammo at game start so it displays correctly during the siren.

## Impact Assessment
- **Players affected:** All players
- **Frequency:** Every game start and city transition
- **Severity:** Minor friction — confusing for new players

## Related Decisions
None.

---

## Resolution (filled by designer/coder when resolved)
- **Resolved by:**
- **Date:**
- **What was done:**
- **Verified by UX Tester:** Pending
