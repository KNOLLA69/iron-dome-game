# Iron Dome Game — Mechanics Audit

> This is the **Game Mechanic agent's work queue**. Focused on: Is this number right? Is this fun? Is this fair?
> **NOT** about: Can the player see it? (Designer) Does the player understand it? (UX Tester)
> This document is owned by the Mechanic agent. Other agents may reference but should not edit.

---

## Rating Scale

| Rating | Meaning |
|--------|---------|
| UNAUDITED | Not yet evaluated |
| ⚠️ BROKEN | Buggy, non-functional, or contradictory |
| 🔶 NEEDS TUNING | Numbers don't produce the right fun/difficulty feel |
| ✅ TUNED | Feels right, balanced, fun |
| ⭐ NAILED IT | Deeply satisfying, tight, perfect |

---

## A: Difficulty Curve (6 elements)

_Is the game's difficulty progression smooth, fair, and exciting?_

**Verified code values (line 2001-2008):** `missiles: 6+w*2`, `speed: 0.9+w*0.06`, `interval: max(40, 110-w*4)`

| # | Element | Current Value | Rating | Notes |
|---|---------|--------------|--------|-------|
| A1 | Wave 1-3 difficulty (tutorial zone) | 8/10/12 enemies, rockets only, speed 0.96-1.08, interval 106-98f | ✅ TUNED | 30 total enemies across 3 waves, 4:1 ammo. 87% intercept needed against 5 HP. Gentle, learnable. Slow rockets with long spacing give time to learn controls. |
| A2 | Wave 4-6 difficulty (first challenge) | 14/16/18 enemies, rocket 70%/drone 20%/ballistic 10%, speed 1.14-1.26 | ✅ TUNED | 48 base enemies, 4:1 ammo, 90% intercept vs 6 HP. Two new types arrive but rockets still dominate (70%). Step from 12→14 enemies (+17%) is gentle. Ballistics at 10% = ~1-2 per wave, enough to notice without overwhelming. |
| A3 | Wave 7-9 difficulty (spike zone) | 20/22/24 base + ~8-10 decoy extras each = ~28-34 effective, speed 1.32-1.44 | 🔶 NEEDS TUNING | **DIFFICULTY CLIFF.** Effective enemies jump from 48 total (city 2) to ~90 (city 3) = 88% increase. Ammo ratio drops from 4:1 to 2.9:1 due to decoy multiplication. City HP only +1 (6→7, +17%). A single unintercepted decoy group = 5 hits = 71% of city HP. Playtest confirmed: wave 7 with no defense = game over halfway through wave. See DECISION-037. |
| A4 | Wave 10-12 difficulty (mastery test) | 26/28/30 base + ~20-24 decoy extras each = ~46-54 effective, speed 1.50-1.62 | 🔶 NEEDS TUNING | **SECOND CLIFF.** Decoy weight doubles (10%→20%). Effective total ~150 vs 8 HP = 95% intercept needed. Ammo 2.2:1 vs effective enemies. Jump from city 3 (~90) to city 4 (~150) = 67% increase. HP only +1 (7→8). See DECISION-037. |
| A5 | Wave 13-15 difficulty (final gauntlet) | 32/34/36 base + ~24-28 decoy extras = ~56-64 effective, speed 1.68-1.80 | ✅ TUNED | ~182 effective total vs 10 HP = 95% intercept. Hard but proportional to wave 10-12. Jump from city 4 (~150) to city 5 (~182) = 21%, while HP grows 8→10 (+25%). The final challenge scales fairly. |
| A6 | Overall curve shape | Linear base formulas + step-function decoy multiplication | 🔶 NEEDS TUNING | Curve is: gentle linear (w1-6) → cliff (w7, decoy intro) → cliff (w10, decoy weight 2×) → plateau (w13-15). Two difficulty cliffs violate "curve not cliff" principle. Root cause: decoy group spawn (1→5 bodies) creates multiplicative inflation the wave counter doesn't account for. See DECISION-037. |

---

## B: Enemy Value & Threat (6 elements)

_Is each enemy type correctly priced in points vs how dangerous it is?_

**Verified ENEMY_TYPES (line 2125-2130):** rocket 1.0×/1HP/28px/1pt, drone 1.1×/1HP/22px/2pt, ballistic 0.7×/3HP/35px/5pt, decoy 0.7×/1HP/20px/1pt

| # | Element | Current Value | Rating | Notes |
|---|---------|--------------|--------|-------|
| B1 | Rocket: 1pt for HP1, speed 1.0× | Baseline | ✅ TUNED | Correct baseline. Simple, slow, big hitbox (28px). Easy to learn on, satisfying to hit. 1 point is right. |
| B2 | Drone: 2pts for HP1, speed 1.1×, wobble | 2× rocket value | ✅ TUNED | Speed was nerfed from 1.4× to 1.1× (previous balance pass). At 1.1×, barely faster than rockets. The wobble (±2.5px sine) + smaller hitbox (22px) are the real differentiators. 2pts is fair for the added difficulty. Minor note: 1.2× speed would feel more distinct, but current works. |
| B3 | Ballistic: 5pts for HP3, speed 0.7× | 5× rocket value | ✅ TUNED | Good risk/reward. 5pts for 3 interceptors = 1.67 pts/shot (better than rockets at 1pt/shot). Rewards focused fire. Slowest speed (0.7×) + biggest hitbox (35px) = easy to HIT, hard to KILL. Creates interesting "ammo investment" decision. |
| B4 | Decoy: 1pt for HP1, speed 0.7×, group of 5 | Same speed as ballistic, groups of 5 | 🔶 NEEDS TUNING | **Core balance issue.** Group of 5 bodies for 1 wave counter slot creates massive hidden difficulty inflation. 5pts total for 5 interceptors (1pt/shot) — least efficient enemy to chase. Yet unintercepted group = 5 city damage = potentially fatal. Threat:reward ratio is inverted. Speed 0.7× (tied slowest) is conceptually backward — "decoys" should be fast distractors, not slow swarms. See DECISION-037 for proposed fix: reduce group from 5→3. |
| B5 | Enemy type introduction pacing | Rockets w1-3, drones+ballistics w4, decoys w7 | ✅ TUNED | 3 waves of rockets-only is a good learning period. Two types at wave 4 works because rockets still dominate (70%). Ballistic at 10% means ~1-2 per wave — noticeable but not overwhelming. Decoys at wave 7 (10%) is reasonable. 3-wave gap between introductions is sufficient. |
| B6 | Late-game variety | 30/25/25/20 weight split at waves 13-15 | 🔶 NEEDS TUNING | Weights look balanced, but BODY COUNT tells a different story. At wave 15 (36 base): ~11 rockets, ~9 drones, ~9 ballistics, ~35 decoy bodies = decoys are 55% of all bodies despite 20% weight. Late game is visually and tactically dominated by decoy swarms. Reducing group size from 5→3 would drop decoys to ~28% of bodies — much better variety. |

---

## C: Scoring System (5 elements)

_Does the score meaningfully reflect skill? Can good players distinguish themselves?_

| # | Element | Current Value | Rating | Notes |
|---|---------|--------------|--------|-------|
| C1 | Base points per enemy type | 1/2/5/1 | UNAUDITED | Point spread wide enough? |
| C2 | Combo multiplier curve | 1×/2×/3×/5× at 1/3/6/10 kills | UNAUDITED | Does 5× feel earned? Is 10 kills achievable? |
| C3 | Combo timer (2 seconds) | 120 frames | UNAUDITED | Tight enough to reward skill, generous enough to not frustrate? |
| C4 | Score ceiling vs floor | Max possible vs casual play score | UNAUDITED | How big is the skill gap in points? |
| C5 | Leaderboard differentiation | Top 10 tracked | UNAUDITED | Do scores cluster or spread? |

---

## D: Ammo & Resource Pressure (4 elements)

_Does ammo create meaningful decisions, or is it just a number?_

| # | Element | Current Value | Rating | Notes |
|---|---------|--------------|--------|-------|
| D1 | Ammo ratio (4× base enemies) | Wave 1: 32 ammo for 8 enemies; Wave 15: 144 for 36 base (64 effective) | UNAUDITED | Too generous for w1-6? Too tight for w7+ with decoy inflation? |
| D2 | Ammo pressure by wave | 4× base count throughout; effective ratio drops from 4:1 to 2.2:1 | UNAUDITED | Decoy inflation creates hidden ammo pressure in late waves |
| D3 | Rapid Fire impact on economy | 15s free ammo + fire rate | UNAUDITED | Does it trivialize ammo or feel like a reward? |
| D4 | Zero-ammo scenario frequency | — | UNAUDITED | How often does a player actually run dry? |

---

## E: Power-Up Impact (5 elements)

_Is each power-up worth grabbing? Is any one too dominant?_

| # | Element | Current Value | Rating | Notes |
|---|---------|--------------|--------|-------|
| E1 | Shield value | 1 hit blocked | UNAUDITED | Is 1 hit meaningful in wave 13? Or worthless? |
| E2 | Rapid Fire value | 15s unlimited | UNAUDITED | Most impactful power-up? Game-changing or just nice? |
| E3 | EMP value | Full screen clear | UNAUDITED | Too powerful? Free wave-skip? |
| E4 | Power-up frequency | Every 5-combo milestone | UNAUDITED | Too frequent (trivializes) or too rare (forgotten)? |
| E5 | Power-up randomness | Equal weight for all 3 types | UNAUDITED | Should some be rarer? EMP too common? |

---

## F: Interceptor Feel (4 elements)

_Is the balance between player skill and aim-assist right?_

| # | Element | Current Value | Rating | Notes |
|---|---------|--------------|--------|-------|
| F1 | Homing generosity | ±30° cone, 60px, 0.15 rad/frame steer | UNAUDITED | Too much auto-aim? Not enough? |
| F2 | Blast radius value | 30px splash damage | UNAUDITED | Useful for groups? Or negligible? |
| F3 | Interceptor speed | 10 px/frame | UNAUDITED | Can it catch fast drones/decoys? Feels responsive? |
| F4 | Skill ceiling | Homing + blast combined | UNAUDITED | Can a skilled player meaningfully outperform aim-assist? |

---

## G: City Endurance & Stakes (4 elements)

_Does the HP system create tension without feeling unfair?_

| # | Element | Current Value | Rating | Notes |
|---|---------|--------------|--------|-------|
| G1 | HP scaling (5→6→7→8→10) | Proportional to wave count increase | UNAUDITED | Is the ratio right? Does HP grow fast enough? |
| G2 | Cross-wave damage accumulation | Persists across 3 waves, resets per city | UNAUDITED | Fair? Or should there be partial recovery? |
| G3 | Game over fairness | Binary: damage >= HP | UNAUDITED | Should there be a "last stand" mechanic? Warning? |
| G4 | Shield as endurance buffer | 1 shield = 1 HP saved | UNAUDITED | Is shield impactful enough for city defense? |

---

## H: Fun & Replay (4 elements)

_The big questions: Is this game fun? Does it make you want to play again?_

| # | Element | Current Value | Rating | Notes |
|---|---------|--------------|--------|-------|
| H1 | Session length | 15 waves, ~20-30 min | UNAUDITED | Right for a web game? Too long? Too short? |
| H2 | "One more try" after game over | — | UNAUDITED | Does the player feel motivated to retry? |
| H3 | Score-chasing motivation | Leaderboard + high score | UNAUDITED | Enough to bring players back? |
| H4 | Skill growth feeling | — | UNAUDITED | Does the player feel themselves getting better? |

---

## Audit Rounds

| Round | Focus | Elements | Completed | Date |
|-------|-------|----------|-----------|------|
| Round 1 | Difficulty + Enemy Value | A1-A6, B1-B6 (12) | **12/12** | 2026-03-14 |
| Round 2 | Scoring + Economy | C1-C5, D1-D4 (9) | 0/9 | — |
| Round 3 | Power-Ups + Interceptors + Endurance | E1-E5, F1-F4, G1-G4 (13) | 0/13 | — |
| Round 4 | Fun & Replay | H1-H4 (4) | 0/4 | — |
| **Total** | | **38 elements** | **12/38** | — |

### Round 1 Summary
- **7 of 12 elements rated ✅ TUNED:** A1, A2, A5, B1, B2, B3, B5
- **5 of 12 elements rated 🔶 NEEDS TUNING:** A3, A4, A6, B4, B6
- **0 elements ⚠️ BROKEN**
- **Root cause of all 5 issues:** Decoy group spawn mechanic (1 counter slot → 5 bodies)
- **Proposed fix:** DECISION-037 — reduce decoy group size from 5 to 3

---

## Balance Change Log

| Date | Element(s) | Old → New | Decision # | Result |
|------|-----------|-----------|-----------|--------|
| (pre-audit) | Drone speed | 1.4× → 1.1× | (prior session) | Made drones less punishing |
| (pre-audit) | Decoy speed | 2.0× → 0.7× | (prior session) | Made decoys interceptable |
| (pre-audit) | Wave formula | 10+w×5 → 6+w×2 | (prior session) | Reduced enemy counts |
| (pre-audit) | Speed formula | 1.1+w×0.1 → 0.9+w×0.06 | (prior session) | Gentler speed curve |
| (pre-audit) | Interval formula | max(30,90-w×5) → max(40,110-w×4) | (prior session) | Longer intervals |
| (proposed) | A3,A4,A6,B4,B6 | Decoy group 5→3 | DECISION-037 | Pending approval |

---

## Verified Wave Data (Round 1 Audit)

| Wave | City | HP | Base Enemies | +Decoy Extras | Effective | Speed | Interval | Ammo | Ammo:Eff |
|------|------|----|-------------|--------------|-----------|-------|----------|------|----------|
| 1 | Nahariya | 5 | 8 | 0 | 8 | 0.96 | 106f | 32 | 4.0:1 |
| 2 | Nahariya | 5 | 10 | 0 | 10 | 1.02 | 102f | 40 | 4.0:1 |
| 3 | Nahariya | 5 | 12 | 0 | 12 | 1.08 | 98f | 48 | 4.0:1 |
| 4 | Beer Sheva | 6 | 14 | 0 | 14 | 1.14 | 94f | 56 | 4.0:1 |
| 5 | Beer Sheva | 6 | 16 | 0 | 16 | 1.20 | 90f | 64 | 4.0:1 |
| 6 | Beer Sheva | 6 | 18 | 0 | 18 | 1.26 | 86f | 72 | 4.0:1 |
| 7 | Haifa | 7 | 20 | ~8 | ~28 | 1.32 | 82f | 80 | 2.9:1 |
| 8 | Haifa | 7 | 22 | ~10 | ~32 | 1.38 | 78f | 88 | 2.8:1 |
| 9 | Haifa | 7 | 24 | ~10 | ~34 | 1.44 | 74f | 96 | 2.8:1 |
| 10 | Jerusalem | 8 | 26 | ~20 | ~46 | 1.50 | 70f | 104 | 2.3:1 |
| 11 | Jerusalem | 8 | 28 | ~22 | ~50 | 1.56 | 66f | 112 | 2.2:1 |
| 12 | Jerusalem | 8 | 30 | ~24 | ~54 | 1.62 | 62f | 120 | 2.2:1 |
| 13 | Tel Aviv | 10 | 32 | ~26 | ~58 | 1.68 | 58f | 128 | 2.2:1 |
| 14 | Tel Aviv | 10 | 34 | ~28 | ~62 | 1.74 | 54f | 136 | 2.2:1 |
| 15 | Tel Aviv | 10 | 36 | ~28 | ~64 | 1.80 | 50f | 144 | 2.3:1 |

**Per-city totals (current):**

| City | HP | Total Eff. Enemies | Max Misses Allowed | Required Intercept % | Ammo:Eff |
|------|----|-------------------|-------------------|---------------------|----------|
| Nahariya | 5 | 30 | 4 | 87% | 4.0:1 |
| Beer Sheva | 6 | 48 | 5 | 90% | 4.0:1 |
| Haifa | 7 | ~94 | 6 | 94% | 2.8:1 |
| Jerusalem | 8 | ~150 | 7 | 95% | 2.2:1 |
| Tel Aviv | 10 | ~184 | 9 | 95% | 2.2:1 |

**Per-city totals (with proposed fix: group 5→3):**

| City | HP | Total Eff. Enemies | Max Misses | Required Intercept % | Ammo:Eff |
|------|----|-------------------|------------|---------------------|----------|
| Nahariya | 5 | 30 | 4 | 87% | 4.0:1 |
| Beer Sheva | 6 | 48 | 5 | 90% | 4.0:1 |
| Haifa | 7 | ~78 | 6 | 92% | 3.4:1 |
| Jerusalem | 8 | ~118 | 7 | 94% | 2.9:1 |
| Tel Aviv | 10 | ~146 | 9 | 94% | 2.8:1 |

---

*Last updated: 2026-03-14*
