# BRIEF-006: UX Round 1 — All 5 Fixes (Consolidated for Designer)

## Metadata
- **Filed by:** UX Tester Agent
- **Date:** 2026-03-14
- **Priority:** P1 (contains P1 + P2 items)
- **Category:** [ONBOARDING] [HUD] [FEEDBACK]
- **Assigned to:** Designer
- **Related audit items:** A2, A3, A5, A7, B4, B8, B9, C6, I5
- **Related briefs:** BRIEF-001, BRIEF-002, BRIEF-003, BRIEF-004, BRIEF-005
- **Status:** OPEN

---

## Overview

UX Round 1 tested the game as a first-time player on desktop (1280x800, English). Result: **game over at wave 1 in every test run.** Five issues were found, ordered below by implementation priority.

---

## Fix 1: First-Play Controls Tutorial (P1 — BRIEF-001)

**Problem:** Zero instructions anywhere. Player has no idea that clicking fires interceptors. "Prepare to intercept..." says WHAT but not HOW. Both test runs ended in wave-1 game-over.

**Implementation:**
- During the siren intro (when `sirenTimer > 0`), show a hint below the "Prepare to intercept..." text:
  - Desktop: `t('instrAimDesktop')` → "Aim with mouse and click to fire interceptor"
  - Mobile: `t('instrAim')` → "Tap the screen to aim and fire interceptor"
- These strings already exist in the i18n TEXTS object (all 3 languages).
- Gate with `localStorage.getItem('tutorialSeen')` — only show on first play. Set the flag after the player's first successful fire (`interceptors.length > 0`).
- Style: white text, ~fz(14), with subtle text shadow for readability against all sky states. Centered below `sirenPrepare` text.

**Where to edit:** `drawSirenIntro()` (~line 2953-2979) and the `fireInterceptor()` function (~line 1722) for setting the localStorage flag.

---

## Fix 2: "Wave 0" Display Bug (P2 — BRIEF-002)

**Problem:** During siren intro, HUD shows "Wave 0 - Nahariya" instead of "Wave 1". The `wave` variable is 0-indexed during the siren, then increments when the wave starts.

**Implementation:** In `drawHUD()` where the wave label is rendered, display `wave + 1` instead of `wave`. Find the line that renders the wave text (in drawHUD, ~line 3058-3242) and change the displayed number.

**Where to edit:** `drawHUD()` — the line that renders the wave/city text at top-center.

---

## Fix 3: Hide "Out of Interceptors" During Siren/Intro (P2 — BRIEF-003)

**Problem:** "Out of Interceptors" in red and "Interceptors: 0/0" display during the siren intro before ammo has loaded. Misleading — makes player think the game is broken.

**Implementation:** In `drawHUD()`, wrap the ammo counter and "Out of Interceptors" warning in a condition:
```
if (sirenTimer <= 0 && cityIntroTimer <= 0) {
  // render ammo bar + "Out of Interceptors" warning
}
```

**Where to edit:** `drawHUD()` (~line 3058-3242) — the section that renders the interceptor count and the out-of-ammo warning at the bottom of the screen.

---

## Fix 4: Game Over Explanation (P2 — BRIEF-004)

**Problem:** Game over screen shows score/wave/combo/rating but never says WHY the game ended. Player doesn't learn that the city's endurance was depleted.

**Implementation:** In `drawGameOver()`, add a line between the wave/combo stats and the rating text:
- Text: `"[CityName] was destroyed"` (use the city name from the current wave's city config)
- i18n: Add new key `cityDestroyed` to TEXTS:
  - he: `' נחרבה'` (appended after city name)
  - en: `' was destroyed'`
  - de: `' wurde zerstört'`
- Style: red text (#ff4444), ~fz(14), with glow. Centered.

**Where to edit:** `drawGameOver()` (~line 3889-4021) and TEXTS object (~line 48-136).

---

## Fix 5: Enemy Type First-Appearance Callout (P2 — BRIEF-005)

**Problem:** 4 enemy types appear without identification. Player can't prioritize targets or understand why some enemies take multiple hits.

**Implementation:** When an enemy type spawns for the first time in a game session, show a brief callout:
- Track seen types: `const seenTypes = {}` (reset in `startGame()`)
- In `spawnMissile()`, when a type spawns and `!seenTypes[type]`: set `seenTypes[type] = true` and set a floating alert (e.g., `enemyAlert = { text: t('droneAlert'), timer: 180 }`)
- Render the alert in `draw()`: centered text, large font ~fz(18), with matching color per type (rocket=orange, drone=blue, ballistic=red, decoy=yellow). Fade out over 3 seconds (180 frames).
- The strings `droneAlert` ("UAV!") and `ballisticAlert` ("Ballistic!") already exist in TEXTS. Add `rocketAlert` and `decoyAlert`:
  - he: `'רקטה!'`, `'נורית הטעיה!'`
  - en: `'Rocket!'`, `'Decoy Flare!'`
  - de: `'Rakete!'`, `'Täuschkörper!'`

**Where to edit:** Game state variables (~line 1180), `startGame()` (~line 1653), `spawnMissile()` (~line 1773), `draw()` near the floating texts section (~line 2757).

---

## Deployment

After all 5 fixes, commit and push:
```
git add index.html && git commit -m "ux: implement Round 1 fixes — tutorial, HUD bugs, game-over context, enemy callouts" && git push
```

Then update all 5 brief files' status to RESOLVED and update HANDOFF.md.

---

## Resolution (filled by designer when resolved)
- **Resolved by:**
- **Date:**
- **What was done:**
- **Verified by UX Tester:** Pending
