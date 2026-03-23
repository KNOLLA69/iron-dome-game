# BRIEF-012: David's Sling (Stunner) Projectile Visual Redesign

## Metadata
- **Filed by:** Mechanic Agent
- **Date:** 2026-03-23
- **Priority:** P1
- **Category:** [GAMEPLAY]
- **Assigned to:** Designer
- **Related audit items:** N/A (new defense system visual)
- **Status:** OPEN

---

## Problem Statement
The David's Sling interceptor projectile looks ugly and doesn't feel native to the game. It's currently a scaled-up Tamir (ctx.scale 1.8×) with an orange exhaust, which makes it look chunky and lazy rather than impressive. The player earns this weapon through skill (5 consecutive hits for a token) — it should FEEL like a reward when it fires.

## Current Implementation
- Location: ~line 4454 in the `draw()` function, inside the defense system rendering block
- Approach: `ctx.scale(1.8, 1.8)` wrapping exact Tamir body code + orange exhaust colors
- Trail: orange dot trail from battery to projectile

## What It Should Be
The real Stunner interceptor (קלע דוד) is a two-stage, longer, sleeker missile. In-game it should:
- Feel like the Tamir's big brother — same design language but clearly more powerful
- Be longer and sleeker (not just fatter)
- Have a distinctive visual signature so the player instantly knows "that's my Sling"
- Orange exhaust trail is good for distinguishing from Tamir's blue — keep that
- Should feel like a REWARD — when this thing flies, you feel powerful

## Design References
- The existing Tamir interceptor (line ~4164) is the gold standard for the game's missile visual language: white/grey cylinder, dark seeker dome, canard + tail fins, specular highlight, engine glow
- The real Stunner is ~4.6m long vs Tamir's ~3m — proportionally longer and thinner
- It has a two-stage booster separation look (could be shown as a body color break)

## Technical Notes
- Rendered per-frame in Canvas 2D (no sprites)
- Already positioned/rotated via ctx.translate + ctx.rotate before body drawing
- The projectile tracks toward a target missile (ballistics only)
- Active for ~2-3 seconds per target, max 2 kills per activation
- Must look good against all sky backgrounds (day/sunset/night)

## Impact Assessment
- **Players affected:** All (appears when defense token is spent on Sling)
- **Frequency:** Several times per game session
- **Severity:** Polish — but high-impact polish since it's tied to a reward moment

## Related Decisions
See DECISION-040 (defense systems), BRIEF-011 (tip jar)

---

## Resolution (filled by designer when resolved)
- **Resolved by:**
- **Date:**
- **What was done:**
- **Verified by UX Tester:** Pending
