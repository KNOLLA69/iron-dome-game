# BRIEF-012: Create OG Social Preview Image (1200×630)

## Metadata
- **Filed by:** Marketer Agent
- **Date:** 2026-03-15
- **Priority:** P1
- **Category:** [MENU]
- **Assigned to:** Designer
- **Related audit items:** Marketing M1, M2, M5
- **Status:** OPEN

---

## Problem Statement
When players share the game URL on WhatsApp, Discord, X, Facebook, or LinkedIn, the social preview card shows no image. OG meta tags are now in place and reference `og-image.png`, but the file doesn't exist yet. A compelling preview image is critical — it's the difference between a link people click and one they scroll past.

## Requirements
- **File:** `og-image.png` in the repo root (served at `https://iron-dome-game.com/og-image.png`)
- **Dimensions:** 1200×630 pixels (standard OG image size)
- **Content suggestions:**
  - Game title "Iron Dome" prominently displayed
  - Night sky with city skyline (the game's visual identity)
  - Missiles + interceptor trails (action/excitement)
  - Possibly the Iron Dome battery
  - Keep text minimal — the OG title/description handle the copy
- **Style:** Match the game's visual style (dark sky gradient, city glow, particle effects)
- **Avoid:** Small text that won't render at thumbnail size. Most previews show this at ~300×157px.

## Impact Assessment
- **Players affected:** Everyone who shares or sees a shared link
- **Frequency:** Every single share across all platforms
- **Severity:** High — social shares without images get 2-3x fewer clicks

## Related Decisions
See DECISION-041 (OG meta tags implementation)

---

## Resolution (filled by Designer when resolved)
- **Resolved by:**
- **Date:**
- **What was done:**
- **Verified by Marketer:** Pending
