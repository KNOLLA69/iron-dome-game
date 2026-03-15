# BRIEF-008: Crosshair Nearly Invisible Against Day Sky

## Metadata
- **Filed by:** UX Tester Agent
- **Date:** 2026-03-14
- **Priority:** P2
- **Category:** [HUD] [GAMEPLAY]
- **Assigned to:** Designer
- **Related audit items:** C11
- **Status:** OPEN

---

## Problem Statement
The green crosshair (dashed circle + crosshair lines) is extremely hard to see against the bright day sky. With rgba(100,255,150,0.3) for the outer circle and 0.6 for inner lines at only 1px width, the crosshair nearly disappears against the light blue background. Day sky is the first time-of-day state players encounter (waves 1, 4, 7, 10, 13 — 5 of 15 waves).

## Steps to Reproduce
1. Start a new game, advance to wave 1 gameplay (day sky)
2. Move cursor to the open sky area above the buildings
3. Observe: crosshair is barely visible — thin green lines against bright blue

## Expected Behavior
The crosshair should be clearly visible at all times regardless of background, as it's the player's primary aiming indicator.

## Actual Behavior
Crosshair uses rgba(100,255,150,0.3-0.6) at 1px line width with a dashed circle (radius 18). Against day sky, these thin semi-transparent green elements wash out. Against sunset and night skies, contrast is better but still low.

## Evidence
- **Viewport:** Desktop 1280x800
- **Game state:** Wave 1, day sky (timeOfDay=0)
- **Crosshair:** Green rgba, 1px lines, 18px radius dashed circle
- **Screenshot:** Day sky screenshot shows crosshair barely perceptible at center

---

## Suggested Fix
1. Add a dark outline/shadow to crosshair elements (1-2px dark stroke behind the green lines)
2. Increase line width from 1px to 1.5-2px
3. Increase opacity from 0.3/0.6 to 0.5/0.8
4. Consider a subtle drop shadow (ctx.shadowColor/shadowBlur) on the crosshair for contrast against any background
5. Alternative: use white crosshair with dark outline (works against all backgrounds)

## Impact Assessment
- **Players affected:** All players on day-sky waves
- **Frequency:** 5 of 15 waves use day sky
- **Severity:** Impairs aiming precision — player can't see where they're targeting

## Related Decisions
None.

---

## Resolution (filled by designer when resolved)
- **Resolved by:**
- **Date:**
- **What was done:**
- **Verified by UX Tester:** Pending
