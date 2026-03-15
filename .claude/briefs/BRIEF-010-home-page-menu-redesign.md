# BRIEF-010: Home Page / Menu Screen Needs Visual Overhaul

## Metadata
- **Filed by:** UX Tester Agent
- **Date:** 2026-03-14
- **Priority:** P2
- **Category:** [MENU]
- **Assigned to:** Designer
- **Related audit items:** E1, F2
- **Status:** OPEN

---

## Problem Statement
The home page / menu screen is the player's first impression of the game, but currently feels unfinished and visually flat. Black void letterboxing surrounds the canvas on most viewports, the atmospheric skyline is almost entirely hidden behind a dark overlay, menu content floats high with dead space below, and desktop users get zero hover feedback on buttons. The overall effect is a dark rectangle floating on a black void — not the polished, atmospheric experience the game deserves.

## Issues Identified

### Issue A: Black void letterboxing (P2)
**What:** `body { background: #000 }` creates ugly black bars around the canvas on any viewport that doesn't exactly match the 1100:700 aspect ratio. On wider monitors, black appears on sides. On taller viewports, black appears on top/bottom.

**Evidence:**
- CSS line 14: `background: #000`
- Canvas resize (line ~5028): `Math.min(vw / W, vh / H)` fits canvas within viewport maintaining aspect ratio, leaving visible bars
- Desktop 1280x800 screenshot: black bars visible on sides

### Issue B: Menu backdrop too dark / atmosphere hidden (P2)
**What:** The menu overlay at `rgba(0,5,15,0.88)` — nearly 90% opaque — hides the game's atmospheric skyline and sky gradient. The city buildings, water, and sky that the game renders beneath the menu are barely visible. This makes the menu feel like a flat dark panel instead of an atmospheric game screen.

**Evidence:**
- `drawMenu()` line ~4387: `ctx.fillStyle = 'rgba(0,5,15,0.88)'` full-screen overlay
- The game draws sky gradient, clouds, city skyline, water beneath the menu — but 88% opacity blocks nearly all of it

### Issue C: Menu content not vertically centered (P3)
**What:** Menu content starts at a fixed `fz(75)` from top. On taller viewports, this leaves significant dead space at the bottom. The layout feels top-heavy rather than balanced.

**Evidence:**
- `drawMenu()` lines ~4400-4420: fixed Y positioning starting at `fz(75)`
- Desktop 1280x800 screenshot: content clustered in upper portion

### Issue D: No cursor:pointer on canvas buttons (P3)
**What:** Canvas-rendered buttons (New Game, Leaderboard, etc.) provide zero hover feedback on desktop. No cursor change, no brightness shift, no visual indication that elements are clickable. Players must click blindly to discover interactivity.

**Evidence:**
- Canvas CSS: `cursor: crosshair` always
- `drawMenuButton()`: static gradient rendering, no hover/pressed state
- No mousemove handler checking button rects for hover state
- Related to BRIEF-009 (which also identified this for in-game buttons)

---

## UX/UI Suggestions

The following are observations and starting points — **the Designer has full creative freedom** to interpret these and decide the right visual approach:

1. **Eliminate the black void** — whether through a matching CSS gradient, filling the canvas to the viewport, or another technique
2. **Let the game's atmosphere breathe** — the skyline, sky, and water beneath the menu are beautiful assets being wasted. Consider how much to reveal
3. **Improve vertical balance** — center or distribute menu content so it doesn't feel top-heavy
4. **Add hover feedback on interactive elements** — cursor change, brightness, glow, or other visual cue for desktop users
5. **Consider a bolder redesign** — these are incremental suggestions, but the Designer should weigh whether the home page would benefit from a more significant visual rethink (layout, visual hierarchy, animation, particle effects, etc.) rather than just patching the current issues

**Note:** The Designer should weigh in on the right visual direction. What fits the game's mood? What makes the best first impression? A polished, atmospheric home page that makes players want to click "New Game" is the goal — how to get there is a design decision.

## Impact Assessment
- **Players affected:** All players, every session (menu is always the first screen)
- **Frequency:** Every single session starts here
- **Severity:** Frustrating — first impression feels unpolished, undermines confidence in game quality

## Related Decisions
- BRIEF-009 also identified the button hover feedback issue (P3 portion)

---

## Resolution (filled by designer when resolved)
- **Resolved by:**
- **Date:**
- **What was done:**
- **Verified by UX Tester:** Pending
