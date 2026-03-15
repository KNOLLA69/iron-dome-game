# BRIEF-009: Ammo Counter Text Too Small + No Button Feedback

## Metadata
- **Filed by:** UX Tester Agent
- **Date:** 2026-03-14
- **Priority:** P2/P3
- **Category:** [HUD] [FEEDBACK]
- **Assigned to:** Designer
- **Related audit items:** C3, C10, F2
- **Status:** OPEN

---

## Problem Statement (P2 — Ammo text)
The ammo counter text "Interceptors: 10/32" is rendered at fz(9) = 9px — below comfortable reading size during active gameplay. While the full-width ammo BAR itself communicates level adequately through color coding (green→yellow→red), the precise count requires reading tiny text. At low ammo, the red text is more urgent but still very small.

## Problem Statement (P3 — Button feedback)
Canvas-rendered buttons (New Game, Play Again, Resume, Leaderboard, etc.) have zero hover or press feedback. On desktop, there is no visual indication that the cursor is over a clickable element. No color change, no scale, no cursor:pointer. Players must click blindly to confirm interactivity.

## Steps to Reproduce
### Ammo text:
1. Start a game, advance to gameplay
2. Look at bottom-left area: "Interceptors: 32/32" text
3. Observe: text is 9px — very small, especially when focused on incoming missiles

### Button feedback:
1. On the menu screen, hover cursor over "New Game" button
2. Observe: no visual change, no cursor change
3. Click the button — it works but gave no pre-click feedback

## Evidence
- **Ammo text font:** `bold ${fz(9)}px Arial` = 9px
- **Ammo bar:** full-width with color gradient — this part works well
- **Buttons:** `drawMenuButton()` uses static gradients, no hover/pressed state
- **Canvas cursor:** default cursor throughout, no `cursor: pointer` on interactive areas

---

## Suggested Fix
### Ammo text:
1. Increase ammo text from fz(9) to fz(11-12)
2. Consider showing ammo as fraction with larger font for the current count

### Button feedback:
1. Track mouse position against button rects
2. On hover: slightly brighten button gradient or add glow border
3. Set `canvas.style.cursor = 'pointer'` when mouse is over a button, reset to default otherwise
4. Optional: subtle scale on hover (1.02x)

## Impact Assessment
- **Ammo text:** All players, constant during gameplay. Minor but contributes to "too much tiny text" problem.
- **Button feedback:** All desktop players, every menu interaction. Small polish issue.

## Related Decisions
None.

---

## Resolution (filled by designer when resolved)
- **Resolved by:**
- **Date:**
- **What was done:**
- **Verified by UX Tester:** Pending
