# BRIEF-007: City Endurance Bar & Labels Too Small to Read

## Metadata
- **Filed by:** UX Tester Agent
- **Date:** 2026-03-14
- **Priority:** P2
- **Category:** [HUD]
- **Assigned to:** Designer
- **Related audit items:** C4, C10
- **Status:** OPEN

---

## Problem Statement
The city endurance bar — the single most important survival metric — is nearly invisible during gameplay. The city name label is 8px, the damage count "0/5" is 8px, and the bar itself is only 80x8px. During combat, a player focused on aiming at missiles is unlikely to notice their city being destroyed until the game over screen appears.

## Steps to Reproduce
1. Start a new game, advance to wave 1 gameplay
2. Look at the top-left HUD area (after the pause button)
3. Observe: "Nahariya" text at ~8px, green bar at 80x8px, "0/5" at ~8px
4. Compare to the score display at ~26px in the top-right

## Expected Behavior
City health should be at least as prominent as the score. It's the "lose condition" — players need to see it declining to feel urgency and to learn that protecting the city is the goal.

## Actual Behavior
City endurance is the smallest, least visible HUD element. Text labels at fz(8) are below minimum readable size for gameplay. The bar is functional but easy to overlook when focused on incoming missiles.

## Evidence
- **Viewport:** Desktop 1280x800
- **Font sizes:** City name = fz(8), damage count = fz(8), bar = 80x8px
- **Comparison:** Score = fz(26) with glow — 3x larger than endurance info

---

## Suggested Fix
1. Increase city name to fz(10-11) minimum
2. Increase damage count to fz(10) minimum
3. Widen endurance bar to fz(100-120) and increase height to fz(10-12)
4. Consider adding a subtle pulse/flash when city takes damage (visual feedback that draws eye to the bar)
5. Consider showing a brief screen-edge red flash when endurance drops

## Impact Assessment
- **Players affected:** All players, especially first-timers
- **Frequency:** Constant during gameplay
- **Severity:** Players don't notice city damage accumulating → surprised by game over → feels unfair

## Related Decisions
None.

---

## Resolution (filled by designer when resolved)
- **Resolved by:**
- **Date:**
- **What was done:**
- **Verified by UX Tester:** Pending
