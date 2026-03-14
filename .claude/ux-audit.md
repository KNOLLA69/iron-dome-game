# Iron Dome Game — UX/UI Audit

> This is the **UX tester agent's work queue**. Every user experience element and flow is cataloged here with an evaluation status, findings, and priority.
> After testing an element, update its status and link to any briefs filed.
> This document is owned by the UX Tester agent. Other agents may reference it but should not edit it.

---

## Status Key

| Status | Meaning |
|--------|---------|
| UNTESTED | Not yet evaluated |
| PASS | No significant issues found |
| ISSUES | Problems found, briefs filed |
| BLOCKED | Cannot test (dependency or bug) |

## Priority Levels

| Priority | Label | Definition |
|----------|-------|------------|
| P0 | Blocker | Player cannot complete a core flow |
| P1 | Critical | Player confused/frustrated, likely to quit |
| P2 | Major | Noticeable UX degradation |
| P3 | Minor | Small polish issue |
| P4 | Enhancement | Opportunity to delight |

---

## A. Onboarding / First Play Experience

| # | Test Case | Status | Priority | Findings / Brief |
|---|-----------|--------|----------|------------------|
| A1 | Menu clarity — Can player identify primary action within 3 seconds? | PASS | — | Green "New Game ►" button is clearly primary. Good color hierarchy (green > blue > gray). Note: top ~40% of viewport is empty black space on desktop. |
| A2 | Game controls discovery — Does the player know how to aim and fire? | ISSUES | P1 | **Zero instructions anywhere.** "Prepare to intercept..." tells WHAT but not HOW. Player must discover click-to-fire by trial and error. → BRIEF-001 |
| A3 | First wave difficulty — Is wave 1 forgiving enough for learning? | ISSUES | P2 | Nahariya has only 5 endurance. Without knowing controls, a first-time player will lose the city in wave 1. Game over at wave 1 occurred in both test runs. Siren intro + city intro consume time while "Wave 0" and "Out of Interceptors" display confusingly. |
| A4 | Enemy type introduction — Are new types introduced with explanation? | ISSUES | P2 | No introduction of enemy types. Rockets, drones, ballistics, decoys appear without labels, tooltips, or callouts. Player cannot prioritize targets. → BRIEF-005 |
| A5 | Tutorial/instruction presence — Any onboarding beyond "figure it out"? | ISSUES | P1 | Complete absence of any tutorial, tooltips, or instructions. Biggest UX gap in the game. → BRIEF-001 |
| A6 | Language selection discovery — Can players find and change language? | PASS | — | Flag buttons (🇮🇱 🇺🇸 🇩🇪) visible above the title on all screens. Active language highlighted with border. Language persists across reloads via localStorage. |
| A7 | First death experience — Does the player understand why they failed? | ISSUES | P2 | Game over screen shows score, wave, max combo, rating ("Rookie"), and "Play Again". But does NOT explain WHY the game ended — no "city destroyed" message, no endurance summary. Leaderboard icon lacks text label. → BRIEF-004 |

---

## B. Core Gameplay Loop

| # | Test Case | Status | Priority | Findings / Brief |
|---|-----------|--------|----------|------------------|
| B1 | Firing feedback — Click/tap produces immediate visual + audio response? | PASS | — | Interceptors launch visibly from battery toward aim point. Audio could not be verified via preview tools (hidden tab). Visually, interceptor trails are visible. |
| B2 | Hit feedback — Destroying a missile is visually satisfying and clear? | PASS | P3 | Explosions appear on hit with particles. Score increments (score went 0→8 in test). Could not fully evaluate satisfaction due to frame-driving testing method. Minor: no floating score text observed during hits. |
| B3 | Miss feedback — Interceptor missing a target is understood? | ISSUES | P3 | Interceptors that miss simply disappear — no distinct visual or audio feedback for a miss vs. the interceptor still being in flight. Player may not know if their shot connected. |
| B4 | City damage feedback — Player knows when city takes damage and severity? | ISSUES | P2 | Endurance bar in HUD changes (green→yellow→red) but is small and in the corner. "Out of Interceptors" message displays during siren intro before ammo loads — confusing. → BRIEF-003 |
| B5 | Combo system clarity — Is the combo mechanic self-explanatory? | ISSUES | P2 | Max combo of 3 achieved in testing. No visible combo indicator was observed during gameplay screenshots. The combo system exists (shown on game over screen) but is not self-explanatory during play. |
| B6 | Power-up discovery — Does the player know what power-ups do? | UNTESTED | — | Did not reach combo x5 milestone needed to spawn power-ups in testing. Deferred to Round 2. |
| B7 | Power-up collection — Is picking up power-ups intuitive? | UNTESTED | — | Deferred to Round 2. |
| B8 | Ammo awareness — Does the player notice ammo decreasing and know when low? | ISSUES | P2 | "Interceptors: 80/80" shown at bottom-left with green bar. Text is small. The "Out of Interceptors" warning appears during siren intro before ammo loads — misleading. → BRIEF-003 |
| B9 | Out-of-ammo experience — What happens when ammo hits 0? | ISSUES | P2 | "Out of Interceptors" in red at bottom center. However, same message shows during siren intro (before ammo loads), diluting its meaning during actual gameplay. → BRIEF-003 |
| B10 | Wave pacing — Do waves feel appropriately tense without overwhelming? | PASS | P3 | Waves progress at a reasonable pace once the player knows controls. City destruction at wave 1 is more an onboarding problem (A2/A5) than a pacing problem. Minor: waves feel fast for new players but appropriate for experienced ones. |

---

## C. HUD Readability & Information Hierarchy

| # | Test Case | Status | Priority | Findings / Brief |
|---|-----------|--------|----------|------------------|
| C1 | Score visibility — Can player read score during gameplay? | UNTESTED | — | — |
| C2 | Score against all sky states — Readable on day/sunset/night? | UNTESTED | — | — |
| C3 | Ammo bar clarity — Is ammo level instantly readable at a glance? | UNTESTED | — | — |
| C4 | Endurance bar clarity — Does player understand city health at a glance? | UNTESTED | — | — |
| C5 | Combo display — Is combo visible without distracting from gameplay? | UNTESTED | — | — |
| C6 | Wave/city indicator — Does player know which wave and city they are on? | ISSUES | P2 | "Wave 0 - Nahariya" displayed during siren intro (should be Wave 1). After siren, displays correctly as "Wave 1 - Nahariya" with missile count. → BRIEF-002 |
| C7 | Pause button discoverability — Can player find pause without instructions? | UNTESTED | — | — |
| C8 | Kill feed readability — Is the kill feed readable and useful? | UNTESTED | — | — |
| C9 | Active power-up indicator — Player knows when power-up is active + timer? | UNTESTED | — | — |
| C10 | Information overload check — Is the HUD too cluttered? | UNTESTED | — | — |
| C11 | Crosshair visibility — Is crosshair visible against all backgrounds? | UNTESTED | — | — |

---

## D. Mobile Experience

| # | Test Case | Status | Priority | Findings / Brief |
|---|-----------|--------|----------|------------------|
| D1 | Touch target sizes — All interactive elements >= 44x44px? | UNTESTED | — | — |
| D2 | HUD scaling — HUD elements properly scaled via fz() on mobile? | UNTESTED | — | — |
| D3 | Aim and fire on touch — Tap-to-aim-and-fire works in one gesture? | UNTESTED | — | — |
| D4 | Accidental UI taps — Pause/HUD not overlapping with gameplay area? | UNTESTED | — | — |
| D5 | Mobile keyboard name input — Virtual keyboard works smoothly? | UNTESTED | — | — |
| D6 | Viewport fitting — No content cut off or overflowing at 375px? | UNTESTED | — | — |
| D7 | Orientation handling — Game works in both portrait and landscape? | UNTESTED | — | — |
| D8 | Text readability at mobile size — All text remains legible? | UNTESTED | — | — |
| D9 | Share buttons tappability — Share buttons have adequate spacing? | UNTESTED | — | — |

---

## E. Menu / Navigation Flow

| # | Test Case | Status | Priority | Findings / Brief |
|---|-----------|--------|----------|------------------|
| E1 | Menu-to-game flow — Smooth transition from menu to gameplay? | UNTESTED | — | — |
| E2 | Game-over-to-menu flow — Can player easily return to menu? | UNTESTED | — | — |
| E3 | Leaderboard navigation — Open, browse, close leaderboard works? | UNTESTED | — | — |
| E4 | Legal overlay navigation — Tabs, scrolling, close all function? | UNTESTED | — | — |
| E5 | Share functionality — All 4 share buttons produce expected result? | UNTESTED | — | — |
| E6 | Contact button — Opens email client correctly? | UNTESTED | — | — |
| E7 | Language switching — Changing language updates all visible text? | UNTESTED | — | — |
| E8 | Name input to leaderboard flow — Submit name, see updated board? | UNTESTED | — | — |
| E9 | Victory screen flow — All actions on victory screen work? | UNTESTED | — | — |
| E10 | Back navigation — Can player always go "back" from any screen? | UNTESTED | — | — |

---

## F. Feedback & Responsiveness

| # | Test Case | Status | Priority | Findings / Brief |
|---|-----------|--------|----------|------------------|
| F1 | Click-to-fire latency — Under 100ms from click to interceptor launch? | UNTESTED | — | — |
| F2 | Button press feedback — Buttons show visual press/hover state? | UNTESTED | — | — |
| F3 | Screen shake appropriateness — Shake enhances, not annoys? | UNTESTED | — | — |
| F4 | Sound-visual sync — Audio cues match visual events timing? | UNTESTED | — | — |
| F5 | Transition smoothness — Fades/reveals are smooth, not jarring? | UNTESTED | — | — |
| F6 | Loading states — Leaderboard loading has indicator? | UNTESTED | — | — |
| F7 | Error handling — What if Firebase/network fails? Graceful? | UNTESTED | — | — |

---

## G. Edge Cases & Error States

| # | Test Case | Status | Priority | Findings / Brief |
|---|-----------|--------|----------|------------------|
| G1 | Rapid clicking during transitions — No crash or state corruption? | UNTESTED | — | — |
| G2 | Empty name submission — Prevented? Error shown? | UNTESTED | — | — |
| G3 | Double-click prevention — No duplicate actions from fast clicks? | UNTESTED | — | — |
| G4 | Pause during wave transition — Behaves correctly? | UNTESTED | — | — |
| G5 | Tab visibility change — Game pauses when browser tab hidden? | UNTESTED | — | — |
| G6 | Resize during gameplay — Canvas handles window resize? | UNTESTED | — | — |
| G7 | Leaderboard with no data — Graceful empty state? | UNTESTED | — | — |
| G8 | Audio context suspended — Game handles autoplay restrictions? | UNTESTED | — | — |

---

## H. Accessibility

| # | Test Case | Status | Priority | Findings / Brief |
|---|-----------|--------|----------|------------------|
| H1 | Color contrast — Text meets WCAG AA against backgrounds? | UNTESTED | — | — |
| H2 | Color-only information — Any info conveyed by color alone? | UNTESTED | — | — |
| H3 | Text size minimums — No text below 12px equivalent on mobile? | UNTESTED | — | — |
| H4 | Touch target minimums — 44x44px for all interactive on mobile? | UNTESTED | — | — |
| H5 | Keyboard navigation — Escape to pause works; other keyboard support? | UNTESTED | — | — |
| H6 | Screen reader metadata — Page title, lang attribute correct? | UNTESTED | — | — |
| H7 | Motion sensitivity — Can screen shake be disabled? | UNTESTED | — | — |

---

## I. Emotional Arc

| # | Test Case | Status | Priority | Findings / Brief |
|---|-----------|--------|----------|------------------|
| I1 | Tension buildup — Does difficulty progression feel dramatic? | UNTESTED | — | — |
| I2 | Satisfaction moments — Are kills, combos, power-ups rewarding? | UNTESTED | — | — |
| I3 | Stress vs overwhelm — Is late-game intense without being unfair? | UNTESTED | — | — |
| I4 | Victory payoff — Does winning feel earned and celebrated? | UNTESTED | — | — |
| I5 | Defeat grace — Does losing feel fair, not arbitrary? | UNTESTED | — | — |
| I6 | City connection — Does defending named real cities create stakes? | UNTESTED | — | — |
| I7 | Replay motivation — After game over, does the player want to retry? | UNTESTED | — | — |

---

## Test Rounds

| Round | Focus | Tests | Completed | Date |
|-------|-------|-------|-----------|------|
| Round 1 | Cold Start + Core Loop | A1-A7, B1-B10 | 15/17 | 2026-03-14 |
| Round 2 | HUD + Feedback | C1-C11, F1-F7 | 1/18 | — |
| Round 3 | Mobile | D1-D9 | 0/9 | — |
| Round 4 | Navigation + Edge Cases | E1-E10, G1-G8 | 0/18 | — |
| Round 5 | Accessibility + Emotion | H1-H7, I1-I7 | 0/14 | — |
| **Total** | | **76 test cases** | **16/76** | — |

---

## Briefs Filed

| Brief | Title | Priority | Assigned | Status |
|-------|-------|----------|----------|--------|
| BRIEF-001 | No Controls Tutorial or Instructions | P1 | Coder | OPEN |
| BRIEF-002 | "Wave 0" During Siren Intro | P2 | Coder | OPEN |
| BRIEF-003 | "Out of Interceptors" During Siren | P2 | Coder | OPEN |
| BRIEF-004 | Game Over Doesn't Explain Why | P2 | Designer | OPEN |
| BRIEF-005 | Enemy Types Not Introduced | P2 | Coder | OPEN |
