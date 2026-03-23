# Lessons Learned — Iron Dome Project

> **This is the project's collective memory of mistakes and wins.** Every agent MUST read this file at session start and apply these lessons. Entries are append-only — never delete or edit past entries.

---

## How to Use This File

### Reading (ALL agents, EVERY session)
- Read this file **before starting any task**
- Scan for lessons relevant to your current work
- If a lesson applies, follow it — don't repeat the mistake

### Writing (when the user iterates or corrects you)
When the user pushes back on your work ("no", "that's wrong", "fix this", "why did you...") AND you arrive at a result they accept, append a new entry using this format:

```
### LESSON-NNN: [short title]
- **Date:** YYYY-MM-DD
- **Agent:** Designer | Marketer | Mechanic | UX Tester
- **Mistake:** What went wrong (be specific)
- **Correction:** What the user wanted instead
- **Rule:** The general principle to follow in the future
```

Also add entries for **positive patterns** — when the user explicitly praises an approach or confirms a non-obvious choice worked:

```
### LESSON-NNN: [short title] ✓
- **Date:** YYYY-MM-DD
- **Agent:** Designer | Marketer | Mechanic | UX Tester
- **What worked:** Describe the approach
- **Why:** Why the user liked it
- **Rule:** Keep doing this
```

---

## Lessons

### LESSON-001: Legal text must match game language
- **Date:** 2026-03-23
- **Agent:** Marketer
- **Mistake:** Added legal sections (Terms, Privacy, Disclaimer) in English only
- **Correction:** User required all legal text to be translated into all 3 game languages (Hebrew, English, German)
- **Rule:** ALL user-facing text in the game must be translated into all 3 languages. Never add English-only content to the game. If you add text, add it to all language branches.

### LESSON-002: Always test mobile after layout changes
- **Date:** 2026-03-17
- **Agent:** Designer
- **Mistake:** Layout changes that looked fine on desktop broke on mobile due to fz() 1.6x scaling
- **Correction:** User caught overlap and sizing issues on mobile
- **Rule:** After ANY layout or text change, test at mobile viewport (375x812). The fz() scaling at 1.6x on mobile causes elements to be much larger than expected.

### LESSON-003: Static screenshots miss animation bugs
- **Date:** 2026-03-17
- **Agent:** Designer
- **Mistake:** Verified changes with a single screenshot, missing animation/timing issues
- **Correction:** User found bugs that only appeared over multiple frames
- **Rule:** For anything animated, take multiple screenshots over time or use preview_eval to step through states. A single frame can hide flickering, overlap, or timing issues.

### LESSON-004: Test on ultrawide resolution (2304x870)
- **Date:** 2026-03-18
- **Agent:** Designer
- **Mistake:** Canvas scaling broke on ultrawide monitors
- **Correction:** Math.max scaling was reverted to Math.min — ultrawide needs side bars, not cropping
- **Rule:** After canvas or layout changes, always verify at 2304x870 (user's actual resolution). Side bars with body gradient are intentional on ultrawide.

### LESSON-005: Canvas fillText Y is BASELINE, not top
- **Date:** 2026-03-17
- **Agent:** Designer
- **Mistake:** Positioned text using Y as the top edge. Title overlapped language flags on mobile because `fillText()` Y is the baseline — glyphs extend above it by roughly the font size.
- **Correction:** Changed calculation to include font ascent: `fz(10) + fz(42) + fz(8) + fz(38)` so baseline sits below the flags.
- **Rule:** `ctx.fillText(text, x, y)` — Y is the baseline. When positioning text below other elements, add the font's pixel size to Y so glyphs clear the elements above.

### LESSON-006: Animating a position variable cascades to all dependents
- **Date:** 2026-03-17
- **Agent:** Designer
- **Mistake:** Added `Math.sin(frame*0.025)*4` bob to menu titleY. But ALL menu content derived Y from titleY, so the entire menu floated up and down.
- **Correction:** Removed title bobbing. Animation on layout-driving variables must be isolated.
- **Rule:** Never animate a position variable that other elements depend on for layout. If you want to animate one element, use a separate "visual-only" offset, not the shared layout variable.

### LESSON-007: Preview tool ≠ real device for mobile
- **Date:** 2026-03-17
- **Agent:** Designer
- **Mistake:** Mobile fix looked correct in preview_screenshot at 375x812 but was still broken on real iPhone. Scaling differences at 1.6x fontS caused the fix to be incomplete.
- **Correction:** Verified on real device, found the baseline calculation error, fixed properly.
- **Rule:** For mobile-critical layout changes, preview_screenshot is a first check, not final verification. If the user reports a mobile bug, the fix must be confirmed on real hardware.

### LESSON-008: UX Tester files briefs, Designer gets creative freedom ✓
- **Date:** 2026-03-15
- **Agent:** All
- **What worked:** UX Tester describes PROBLEMS and provides suggestions, Designer has full creative freedom on the visual approach. Briefs say what's wrong, not how to fix it.
- **Rule:** Briefs describe problems and impact. Solutions are the implementing agent's creative domain. Don't over-specify visual fixes.

### LESSON-009: Decoy group spawn was the root cause of difficulty cliffs
- **Date:** 2026-03-19
- **Agent:** Mechanic
- **Mistake:** Decoy spawned 5 bodies (1 + 4 extras) per event, causing hidden enemy inflation. Ammo ratio dropped from 4:1 to 2.2:1 in late waves. Two difficulty cliffs at waves 7 and 10 spiked difficulty 88% and 67%.
- **Correction:** Reduced decoy extras from 4 to 2 (group of 3). Single number change fixed 5 cascading balance issues.
- **Rule:** When multiple balance problems appear at the same point, they likely share a root cause. Trace back to the structural problem — a single-number fix beats patching symptoms.

### LESSON-010: Combo needs gameplay buffs, not just score multiplier
- **Date:** 2026-03-19
- **Agent:** Mechanic
- **Mistake:** Combo only affected score multiplier — no gameplay reward for maintaining streaks.
- **Correction:** Added `getComboBuffs()`: 3+ combo = 20% faster interceptors, 6+ = 50% bigger blast, 10+ = double homing range.
- **Rule:** Score multipliers alone don't motivate. Streaks must feel powerful through actual gameplay changes (speed, blast, range). This creates the "one more try" loop.

### LESSON-011: First-play tutorial is mandatory
- **Date:** 2026-03-15
- **Agent:** UX Tester
- **Mistake:** No instructions anywhere. Siren said "Prepare to intercept..." but never explained HOW. Both test runs ended in game-over at wave 1.
- **Correction:** Added tutorial hint in siren intro: "Aim with mouse and click to fire" / "Tap to fire". Gated by localStorage flag, shown only on first play.
- **Rule:** Never assume controls are obvious. Teach the player HOW before the action starts. First-play-only gating removes friction for returning players.

### LESSON-012: Loss condition must be visible AND explained
- **Date:** 2026-03-15
- **Agent:** UX Tester
- **Mistake:** City endurance bar (the lose condition) was tiny: 8px text, 80x8px bar. Score was 3x larger than the survival metric. Game over screen didn't explain WHY you lost.
- **Correction:** Filed briefs to enlarge endurance display AND add "[CityName] was destroyed" to game over screen.
- **Rule:** The lose condition must be at least as prominent as the score in the HUD. Game over must explicitly state why the player lost — "Why did I die?" is the first question.

### LESSON-013: Crosshair needs contrast on ALL sky backgrounds
- **Date:** 2026-03-15
- **Agent:** UX Tester
- **Mistake:** Green crosshair at rgba(100,255,150,0.3-0.6) with 1px width washed out against bright day sky (5 of 15 waves).
- **Correction:** Filed brief: add dark outline, increase width to 1.5-2px, increase opacity.
- **Rule:** Any UI element that must always be visible (crosshair, reticle) needs guaranteed contrast on every background. Use dark outline/shadow or a universally high-contrast color.

### LESSON-014: HUD counters must be 1-indexed for players
- **Date:** 2026-03-15
- **Agent:** UX Tester
- **Mistake:** Wave counter showed "Wave 0" during siren intro because it was zero-indexed before incrementing.
- **Correction:** Added `displayWave = waveState === 'siren_intro' ? wave + 1 : wave` so players always see "Wave 1" through "Wave 15".
- **Rule:** Display counters from the player's perspective (1-indexed), not the code's (0-indexed). Adjust display logic during state transitions.

### LESSON-015: Don't show error states during setup phases
- **Date:** 2026-03-15
- **Agent:** UX Tester
- **Mistake:** "Out of Interceptors" warning and "Interceptors: 0/0" displayed during siren intro before ammo loaded. First-time players thought the game was broken.
- **Correction:** Wrapped ammo display in `if (sirenTimer <= 0 && cityIntroTimer <= 0)` to hide during intros.
- **Rule:** HUD elements showing error/empty states must only render when gameplay is active. During transitions and setup, hide them.

### LESSON-016: New enemy types need on-screen introduction
- **Date:** 2026-03-15
- **Agent:** UX Tester
- **Mistake:** Four enemy types appeared without labels or callouts. Players couldn't distinguish types or learn to prioritize.
- **Correction:** Filed brief: add enemy type callout when a new type first appears (e.g., "Incoming: Ballistic Missile").
- **Rule:** Each new game element (enemy, mechanic, power-up) deserves an explicit first-time introduction. Don't rely on players figuring out taxonomy through trial and error.

### LESSON-017: Reference photos drive authentic game art ✓
- **Date:** 2026-03-14
- **Agent:** Designer
- **What worked:** Used real reference photos (Shahed-136, Qassam, Iron Dome battery) to redesign game elements. Each enemy type got a distinct recognizable silhouette.
- **Rule:** For games depicting real-world systems, always start with reference photos. Authenticity improves both aesthetics and educational value.

### LESSON-018: Share prompts at peak emotion moments ✓
- **Date:** 2026-03-19
- **Agent:** Marketer
- **What worked:** Dynamic share text with score ("I scored X points! Can you beat me?") placed on game over AND victory screens — where emotions peak. Challenge framing drives viral loops.
- **Rule:** Share buttons on menu alone are weak. Place them at peak emotion (victory, defeat). Include player stats and competitive framing in share text. Ego-driven shares get more clicks.

### LESSON-019: Social meta tags are table-stakes before any promotion ✓
- **Date:** 2026-03-19
- **Agent:** Marketer
- **What worked:** Full OG + Twitter Card + JSON-LD structured data implemented as foundation before any community posting or aggregator submission. Every share becomes a mini-ad with rich preview.
- **Rule:** Never promote before meta tags are set. One hour of meta tag work amplifies every future share. This is always Priority 1.

### LESSON-020: Monetization must not gate progression ✓
- **Date:** 2026-03-19
- **Agent:** Mechanic
- **What worked:** User rejected "continue/extra-life" purchases and "donation" framing. Approved "tip jar" — optional, no gameplay impact, placed at peak emotion (menu + victory).
- **Rule:** In games with reachable endpoints, monetization must not trivialize achievements or gate progression. Tip jar is the right model for free games with integrity.
