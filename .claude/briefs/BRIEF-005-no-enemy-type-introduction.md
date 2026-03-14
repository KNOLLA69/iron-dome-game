# BRIEF-005: Enemy Types Not Introduced or Explained

## Metadata
- **Filed by:** UX Tester Agent
- **Date:** 2026-03-14
- **Priority:** P2
- **Category:** [ONBOARDING] [GAMEPLAY]
- **Assigned to:** Coder
- **Related audit items:** A4, B5
- **Status:** OPEN

---

## Problem Statement
The game has 4 distinct enemy types (Rocket, Drone/UAV, Ballistic, Decoy Flare) with different speeds, HP, hitbox sizes, and point values, but none of them are introduced or explained to the player. A first-time player sees various incoming threats but cannot distinguish between types or prioritize targets.

## Steps to Reproduce
1. Start a new game
2. Play through waves 1-3
3. Observe: different enemy types appear (rockets with trails, small angular drones, large slow ballistics)
4. No label, tooltip, or introduction explains what each type is or how they differ

## Expected Behavior
- When a new enemy type appears for the first time, a brief callout or label should identify it (e.g., "New threat: UAV Drone — fast & agile")
- Or provide a "Know Your Enemy" section accessible from the menu
- Or show a brief tooltip when the first instance of each type appears

## Actual Behavior
All enemy types appear without any identification or explanation. The player must discover through trial and error that:
- Some enemies are faster than others
- Ballistic missiles take 3 hits
- Decoy flares give fewer points
- Different types have different hitbox sizes

## Evidence
- **Viewport:** Desktop 1280x800
- **Game state:** Waves 1-3
- **Language:** English
- **Observation:** In wave 2 gameplay screenshot, rockets and what appear to be drones were visible with no distinguishing labels

---

## Suggested Fix
Option A: First-appearance callout — when a new enemy type spawns for the first time in a game session, show a brief label/popup (e.g., "Drone Detected!" with 1-line description). Auto-dismiss after 3 seconds.

Option B: Enemy guide in menu — add a "Threats" button on the menu showing all 4 enemy types with their visual appearance, name, and key property (speed, HP, etc.).

Option C: Kill feed already exists — enhance the kill feed to show the enemy type name on kills, helping the player learn types through play.

## Impact Assessment
- **Players affected:** All first-time players
- **Frequency:** Every first session
- **Severity:** Minor friction — player can still play but misses strategic depth

## Related Decisions
None.

---

## Resolution
- **Resolved by:** Designer Agent
- **Date:** 2026-03-14
- **What was done:** Added seenTypes tracking (reset in startGame), first-spawn callout in spawnMissile(), and alert rendering in draw(). Each enemy type shows a colored callout on first appearance (rocket=orange, drone=blue, ballistic=red, decoy=yellow) that fades over 3 seconds. Added rocketAlert/decoyAlert i18n strings in all 3 languages.
- **Verified by UX Tester:** Pending
- **What was done:**
- **Verified by UX Tester:** Pending
