# Iron Dome Game — Visual Element Audit

> This is the **designer agent's work queue**. Every visual element in the game is cataloged here with a quality rating, priority, and specific upgrade notes.
> After upgrading an element, update its rating and add a completion date.
> Use this as the checklist when doing visual polish passes.

---

## Rating System

| Rating | Meaning | Action |
|--------|---------|--------|
| ★☆☆☆☆ | Placeholder/broken | Complete redo required |
| ★★☆☆☆ | Functional but ugly | High priority upgrade |
| ★★★☆☆ | Decent, not impressive | Polish pass needed |
| ★★★★☆ | Good, minor tweaks | Low priority refinement |
| ★★★★★ | AAA quality, ship it | No action needed |

## Priority Levels
- **CRITICAL** — Visible every second of gameplay, first thing players notice
- **HIGH** — Major gameplay element, seen frequently
- **MEDIUM** — Supporting visual, enhances atmosphere
- **LOW** — Minor detail, nice-to-have polish

---

## A. Sky & Environment

| # | Element | Function/Lines | Rating | Priority | Upgrade Notes |
|---|---------|---------------|--------|----------|---------------|
| A1 | Day sky gradient | draw() 2204-2216 | ★★★★☆ | DONE | **Sprint 1 complete (2026-03-14)** — 10 color stops: deep cerulean top → rich blues → warm atmospheric haze at horizon. Mediterranean feel. |
| A2 | Sunset sky gradient | draw() 2217-2231 | ★★★★☆ | DONE | **Sprint 1 complete (2026-03-14)** — 12 color stops: twilight purple → magenta → rose → deep orange → golden horizon glow. Dramatic Mediterranean sunset. |
| A3 | Night sky gradient | draw() 2232-2244 | ★★★★☆ | DONE | **Sprint 1 complete (2026-03-14)** — 11 color stops: near-black zenith → deep navy → midnight blue → subtle warm horizon. Much more depth. |
| A4 | Stars | draw() 2237-2247 | ★★☆☆☆ | MEDIUM | Basic dots only. Need: size variation (1-3px), brightness variation, proper twinkling animation (not just alpha), star colors (white/blue/yellow). Maybe 1-2 shooting stars per night wave. |
| A5 | Clouds (day) | draw() 2249-2340 | ★★★★☆ | DONE | **Sprint 1 complete (2026-03-14)** — 7-lobe volumetric clouds with 3-pass rendering: cool blue-gray undershadow, bright body with radial gradients, sunlit top highlights. |
| A6 | Clouds (sunset) | draw() 2249-2340 | ★★★★☆ | DONE | **Sprint 1 complete (2026-03-14)** — Warm orange-pink underlit bellies, warm body tones, golden top highlights. Sunset clouds are now visually stunning. |
| A7 | Clouds (night) | draw() 2249-2340 | ★★★½☆ | MEDIUM | Upgraded as part of cloud rework — dark blueish shadow, muted body tones. Could still add moonlit silver edges. |
| A8 | Sun (day) | draw() 2274-2295 | ★★★☆☆ | MEDIUM | Basic yellow circle with glow. Needs: corona rays, heat shimmer, lens flare hint. |
| A9 | Sun (sunset) | draw() 2296-2315 | ★★★☆☆ | HIGH | Needs to be more dramatic — large, deep orange/red, partially below horizon, radiating warm glow bands across sky. |
| A10 | Moon | draw() 2316-2334 | ★★★☆☆ | MEDIUM | Has craters which is nice. Add: subtle blue glow halo, surface texture variation, maybe phase (crescent). |
| A11 | Sea (water gradient) | draw() 2336-2420 | ★★★★☆ | DONE | **Sprint 1 complete (2026-03-14)** — 5-stop gradients per time state, wave motion ripple lines at surface, sun/moon reflection paths (golden sunset path, silver moonlight, bright sun sparkle column). |
| A12 | Sea sparkles/shimmer | draw() 2370-2420 | ★★★★☆ | DONE | **Sprint 1 complete (2026-03-14)** — Concentrated reflection paths under sun/moon, elongated horizontal sparkles, animated shimmer with depth-based brightness, city light vertical streaks with glow. |
| A13 | City glow | draw() 2419-2438 | ★★★☆☆ | MEDIUM | Light pollution gradient is functional. Could be warmer (sodium lamp orange), more prominent at night, subtle at day. |
| A14 | Ground line | draw() ~2440 | ★★☆☆☆ | LOW | Just a thin line. Could add terrain texture hint, grass, sandy earth tone, or subtle ground-level dust/haze. |
| A15 | Vignette | draw() 2920-2924 | ★★★★☆ | LOW | Works well. Could be slightly stronger during night waves for cinematic effect. |

---

## B. Cities & Buildings

| # | Element | Function/Lines | Rating | Priority | Upgrade Notes |
|---|---------|---------------|--------|----------|---------------|
| B1 | Rect building | drawBuilding() 790-870 | ★★★☆☆ | MEDIUM | Basic gradient fill + windows. Need: rooftop details (AC units, railings, water tanks), wall texture variation, balcony hints, occasional antenna. |
| B2 | Round building (Azrieli) | drawBuilding() 872-910 | ★★★☆☆ | MEDIUM | Curved top reads well. Add: glass panel reflections, structural ring at base of dome, subtle highlight from sun direction. |
| B3 | Triangle building | drawBuilding() 912-945 | ★★★☆☆ | MEDIUM | Triangular peak is clear. Add: glass curtain wall line pattern, structural vertex accents. |
| B4 | Spire building | drawBuilding() 947-980 | ★★★☆☆ | MEDIUM | Modern tower with spire. Add: antenna light at top (red blinking at night), glass reflection strip. |
| B5 | Dome (Jerusalem) | drawBuilding() 982-1015 | ★★★☆☆ | HIGH | Important landmark. Needs: golden color for Dome of the Rock reference, better dome curvature, ornamental base ring, warm glow at night. |
| B6 | Wall (Old City) | drawBuilding() 1017-1050 | ★★★☆☆ | HIGH | Crenellations are good. Add: stone block texture (alternating shade rows), aged/weathered look, torch/lamp glow at night. |
| B7 | Lighthouse (Nahariya) | drawBuilding() 1052-1090 | ★★★★☆ | LOW | Already has red/white stripes, lamp room, beacon glow. Minor: add beacon rotation sweep, better glass reflection in lamp room. |
| B8 | Crane (Haifa port) | drawBuilding() 1092-1110 | ★★★☆☆ | MEDIUM | Structural shape is clear. Add: cable thickness, trolley detail, warning light blink. Red/white hazard stripes. |
| B9 | Sail (Haifa tower) | drawBuilding() 1112-1125 | ★★★☆☆ | MEDIUM | Sail shape recognizable. Add: glass panel reflections with sun angle, structural rib lines, wind-swept accent. |
| B10 | Stepped (Bahai) | drawBuilding() 1127-1133 | ★★★☆☆ | HIGH | Terraced garden with dome. Add: more terrace levels with flower color variation, better golden dome with gleam, garden greenery detail. |
| B11 | Windows (all buildings) | drawBuilding() ~850-870 | ★★☆☆☆ | HIGH | Generic yellow rectangles. Upgrade to: warm interior glow at night (varied colors), some dark/unlit windows, occasional blue TV flicker, curtain hints. Day windows should be reflective glass blue. |
| B12 | City skyline composition | generateCityCanvas() 1135-1177 | ★★★☆☆ | MEDIUM | Layering (bg→mid→landmark→fg) works. Could add: depth fog between layers, subtle parallax on scroll, atmospheric perspective (distant = hazier/bluer). |
| B13 | Building foundations | drawBuilding() base | ★★☆☆☆ | LOW | Buildings just sit on ground line. Add: sidewalk strip, subtle shadow at base, street-level detail (streetlamp, tree). |
| B14 | Night city glow effect | draw() 2419-2438 | ★★★☆☆ | MEDIUM | Window glow should cast faint light onto building face. Night buildings should have warm ambiance around lit windows. |

---

## C. Battery & Launcher

| # | Element | Function/Lines | Rating | Priority | Upgrade Notes |
|---|---------|---------------|--------|----------|---------------|
| C1 | Truck chassis | prerenderBattery() 1280-1320 | ★★★★☆ | DONE | **Realism pass (2026-03-14)** — Truck cab added with windshield, undercarriage shadow, enhanced panel lines with highlight, structural I-beam frame rails. |
| C2 | Wheels & axles | prerenderBattery() 1321-1345 | ★★★★☆ | DONE | **Realism pass (2026-03-14)** — Radial tire gradients, tread pattern lines, metallic rim gradient, shadow ellipses under each wheel, detailed hub caps. |
| C3 | Stabilizer jacks | prerenderBattery() 1295-1305 | ★★★½☆ | DONE | **Realism pass (2026-03-14)** — Hydraulic cylinder detail added, gradient shading, rounded jack pads with wider base. |
| C4 | Radar/sensor unit | prerenderBattery() 1346-1360 | ★★★★☆ | DONE | **Realism pass (2026-03-14)** — Replaced dish with phased-array flat panel antenna (EL/M-2084 style), grid pattern face, frame, warning light with glow. |
| C5 | Launch tubes (4×5) | drawLauncherTubes() 1381-1466 | ★★★☆☆ | HIGH | Tube openings visible. Need: deeper tube shadow (recessed), tube interior highlight ring, launch smoke residue after firing, ammo indicator (dark tubes = empty). |
| C6 | Hydraulic arm | drawLauncherTubes() 1440-1460 | ★★★☆☆ | LOW | Basic arm shape. Add: piston detail, joint pivot points, hydraulic line. |
| C7 | Star of David marking | prerenderBattery() ~1370 | ★★★★☆ | LOW | Subtle placement. Fine as-is. |

---

## D. Projectiles & Trails

| # | Element | Function/Lines | Rating | Priority | Upgrade Notes |
|---|---------|---------------|--------|----------|---------------|
| D1 | Rocket body | draw() 2824-2984 | ★★★★☆ | DONE | **Realism pass (2026-03-14)** — Qassam/Grad-style steel tube with 6-stop 3D cylinder gradient, ogive nose cone with quadratic curves, weld seam lines, warhead junction band, specular highlight strip, sheet-metal stabilizer fins with edge highlights. |
| D2 | Rocket exhaust flame | draw() 2574-2630 | ★★★★☆ | DONE | **Sprint 1 complete (2026-03-14)** — 4-layer flame: outer red glow, orange mid, yellow inner, white-hot core. Sine-based flicker animation with random variation. Nozzle glow ring. Ballistic flames larger/redder. |
| D3 | Rocket smoke trail | draw() 2466-2530 | ★★★★☆ | DONE | **Sprint 1 complete (2026-03-14)** — Volumetric puff trail with expanding radial gradients, wind drift + turbulence, warm-to-gray color transition, hot core near engine. Drone: heat shimmer. Decoy: spark trail. |
| D4 | Drone body (Shahed delta-wing) | draw() 2720-2820 | ★★★★☆ | DONE | **Realism pass (2026-03-14)** — Complete redesign from quad-rotor to Shahed-136 delta-wing UAV. Swept-back delta wings with gradient + panel lines, narrow cylindrical fuselage, pointed nose cone with dark seeker, V-tail stabilizers, rear pusher propeller with spinning blur, warhead marking band. Olive-green military color scheme. |
| D5 | Drone trail | draw() drone trails | ★★★☆☆ | MEDIUM | Heat shimmer effect (from Sprint 1) suits the delta-wing design well — no smoke, just faint distortion. |
| D6 | Ballistic body | draw() 2824-2920 | ★★★★☆ | DONE | **Realism pass (2026-03-14)** — Iranian Fajr/Fateh-style: 6-stop red 3D cylinder gradient, separate warhead section with darker conical shape, ogive nose cone with quadratic curves, 3 body segment bands, warhead junction ring, specular highlight strip, larger swept-back stabilizer fins with gradient fill. |
| D7 | Ballistic HP ring | draw() ~2570 | ★★★☆☆ | MEDIUM | Cyan arc indicator works. Could be: segmented HP pips instead of arc, flash red when taking damage, shrink animation on hit. |
| D8 | Ballistic heavy trail | draw() ballistic trails | ★★☆☆☆ | HIGH | Should be much thicker/more dramatic than rocket trail. Heavy smoke + fire. Occasional debris/sparks from damaged body. |
| D9 | Decoy flare dot | draw() 2487-2493 | ★★★☆☆ | MEDIUM | Flickering orange is functional. Add: proper flare burn effect (bright white center, orange halo, trailing spark shower), intensity pulsing. |
| D10 | Interceptor (Tamir) body | draw() 3103-3170 | ★★★★☆ | DONE | **Realism pass (2026-03-14)** — Slender white cylinder with 6-stop 3D gradient, dark seeker dome nose with ogive curve, seeker ring, canard fins near nose, larger cruciform tail fins, body marking band, specular highlight, engine nozzle detail. |
| D11 | Interceptor engine glow | draw() 3155-3175 | ★★★★☆ | DONE | **Realism pass (2026-03-14)** — Bright blue-white pulse motor: large engine glow circle, 4-stop exhaust gradient with curved plume shape, white-hot core flame. Much more visible and realistic. |
| D12 | Interceptor trail | draw() 2660-2679 | ★★★☆☆ | HIGH | Cyan gradient line. Needs: thicker near engine → thin at tail, slight curves from homing maneuvers, smoke puff at tight turns. |
| D13 | Muzzle flash | draw() 2731-2748 | ★★★☆☆ | MEDIUM | Blue glow burst. Add: directional flash (cone shape toward target), brief smoke ring, illuminates nearby battery surface. |

---

## E. Explosions & Particles

| # | Element | Function/Lines | Rating | Priority | Upgrade Notes |
|---|---------|---------------|--------|----------|---------------|
| E1 | Explosion radial gradient | draw() 2826-2890 | ★★★★☆ | DONE | **Sprint 1 complete (2026-03-14)** — Shockwave ring (fast-expanding, fading), asymmetric fireball center, 7-stop gradient, internal bright flash timer, secondary fireballs for big explosions. Spark streaks with motion blur (elongated ellipses). |
| E2 | Debris particles | boom() 1827-1862 | ★★☆☆☆ | HIGH | Basic dots. Needs: varied shapes (rectangles for metal shards, triangles for fragments), tumbling rotation, proper gravity arc, size variation, metallic colors. |
| E3 | Fire particles | boom() fires | ★★☆☆☆ | HIGH | Simple colored dots rising up. Needs: teardrop/flame shape, orange→red→black color transition, flicker animation, heat distortion above fire. |
| E4 | Spark particles | boom() sparks | ★★★☆☆ | MEDIUM | Bright and fast. Add: longer streak tails (motion blur), random direction scatter, brighter white core, gravity curve. |
| E5 | Smoke trail particles | fireInterceptor() smoke | ★★★☆☆ | MEDIUM | Launch smoke puffs. Add: expansion over time, turbulence drift, proper gray→transparent fade, inter-particle blending. |
| E6 | Shield block effect | update() shield hit | ★★☆☆☆ | MEDIUM | Minimal currently. Needs: visible golden hexagonal shield flash, ripple effect outward, absorbed missile dissolves into particles, satisfying "blocked!" feedback. |

---

## F. Power-Ups

| # | Element | Function/Lines | Rating | Priority | Upgrade Notes |
|---|---------|---------------|--------|----------|---------------|
| F1 | Shield pickup icon | draw() 2875-2888 | ★★☆☆☆ | MEDIUM | Plain emoji on circle. Redesign: golden hexagonal shield shape, pulsing glow, rotating slowly, energy particles orbiting. |
| F2 | Rapid fire pickup icon | draw() 2875-2888 | ★★☆☆☆ | MEDIUM | Plain emoji on circle. Redesign: electric lightning bolt shape, crackling electricity arcs, blue/white energy, spark effects. |
| F3 | EMP pickup icon | draw() 2875-2888 | ★★☆☆☆ | MEDIUM | Plain emoji on circle. Redesign: pulsing red/orange energy orb, shockwave rings, ominous glow, dangerous-looking. |
| F4 | Power-up collection effect | collectPowerUp() | ★★☆☆☆ | MEDIUM | Minimal currently. Add: absorption particle burst, screen-wide color flash matching power type, HUD icon animation (slides in), satisfying sound cue. |

---

## G. HUD & Gameplay UI

| # | Element | Function/Lines | Rating | Priority | Upgrade Notes |
|---|---------|---------------|--------|----------|---------------|
| G1 | Score display | drawHUD() 3139-3147 | ★★★☆☆ | MEDIUM | White text with glow. Add: score change animation (countup), scale pulse on increment, better font styling (maybe outlined), digit separator for large numbers. |
| G2 | Ammo bar | drawHUD() 3149-3192 | ★★★☆☆ | HIGH | Functional gradient bar. Needs: segmented look (individual ammo pips), bullet icon, smoother color transitions, low-ammo pulse/flash warning, ammo count text centered. |
| G3 | City endurance bar | drawHUD() 3081-3116 | ★★★☆☆ | HIGH | Green→yellow→red works. Add: heart or shield icon, segmented HP pips (one per HP point), damage flash animation, shaking when critical (red zone). |
| G4 | Combo counter | drawHUD() 3194-3221 | ★★★☆☆ | MEDIUM | Pulsing text works. Add: multiplier number should grow larger at higher combos, fire/energy effects around high combos, timeout bar should pulse urgently when nearly expired. |
| G5 | Wave info text | drawHUD() 3118-3129 | ★★★☆☆ | LOW | Functional. Could add: city icon/silhouette thumbnail, more prominent wave number, animated transition when wave changes. |
| G6 | Crosshair | drawHUD() 3224-3241 | ★★☆☆☆ | HIGH | Basic dashed circle + lines. Redesign: military-style reticle (thin precise lines), range finder marks, target lock indicator when near enemy, color change on hovering target. |
| G7 | Pause overlay | draw() 2927-2945 | ★★★☆☆ | LOW | Dark screen + button. Add: frosted glass/blur effect, animated pause icon (two bars), menu options (resume, settings, quit). |
| G8 | Kill feed | draw() 2891-2902 | ★★★☆☆ | MEDIUM | Functional text list. Add: enemy type mini-icon (not just emoji), slide-in animation, background pill shape for readability, score points with color. |
| G9 | Floating score text | draw() 2757-2768 | ★★★☆☆ | LOW | Rising fading text. Add: scale animation (pop-in), trajectory curve, combo multiplier shows differently (larger, colored). |
| G10 | Shield indicator | drawHUD() ~3105 | ★★☆☆☆ | MEDIUM | Small indicator. Needs: visible golden shield icon in HUD, glow animation while active, clear "protected" state. |
| G11 | Active power indicator | drawHUD() ~3110 | ★★☆☆☆ | MEDIUM | Timer display. Needs: power-specific icon + color, countdown ring (circular timer), pulsing urgency when about to expire. |

---

## H. Menus & Overlays

| # | Element | Function/Lines | Rating | Priority | Upgrade Notes |
|---|---------|---------------|--------|----------|---------------|
| H1 | Title text | drawMenu() ~3420 | ★★★☆☆ | HIGH | Text with shadow. Needs: stylized military font look (sharp angles), metallic gradient fill, subtle glow, maybe Iron Dome logo/emblem. |
| H2 | Subtitle | drawMenu() ~3430 | ★★★☆☆ | MEDIUM | Plain text. Add: typewriter reveal animation on first load, subtitle glow. |
| H3 | Flag language selector | drawLangToggle() 3306-3343 | ★★★☆☆ | MEDIUM | Flags + labels work. Add: hover scale animation, smoother active state transition, flag shadow/depth. |
| H4 | New Game button | drawMenu()/drawMenuButton() | ★★★☆☆ | HIGH | Green gradient is good. Needs: subtle shine/glare sweep animation, press effect (scale down), icon animation (play arrow pulses), glow on hover. |
| H5 | Leaderboard button | drawMenu()/drawMenuButton() | ★★★☆☆ | HIGH | Blue gradient is good. Same polish as H4: shine sweep, press effect, trophy icon gleam. |
| H6 | Contact button | drawMenu()/drawMenuButton() | ★★★☆☆ | MEDIUM | Gray/dark blue. Same polish as H4. |
| H7 | Play counter | drawMenu() ~3500 | ★★☆☆☆ | MEDIUM | Plain number. Add: animated counting-up effect on load, subtle background badge/pill, icon (gamepad or play symbol). |
| H8 | Share buttons | drawMenu() ~3510-3540 | ★★☆☆☆ | MEDIUM | Basic colored squares. Needs: branded colors (WhatsApp green, X black, etc.), proper icons (not text), hover glow, rounded corners, consistent sizing. |
| H9 | Legal links | drawMenu() ~3545 | ★★★☆☆ | LOW | Text links. Fine functionally. Could add: subtle underline on hover, separator dots. |
| H10 | Legal overlay | drawLegalOverlay() 3555-3665 | ★★★☆☆ | MEDIUM | Functional overlay. Needs: smoother fade-in/out animation, better tab styling (underline active tab), scroll indicators (arrow or fade at top/bottom), nicer close button (X with circle). |
| H11 | Leaderboard table | drawLeaderboardTable() 3345-3380 | ★★★☆☆ | MEDIUM | Basic table. Needs: alternating row backgrounds, podium highlight for top 3 (gold/silver/bronze), flag emoji better rendered, score right-aligned with digit separator. |
| H12 | Name input screen | drawNameInput() 3667-3764 | ★★☆☆☆ | HIGH | Functional but plain. Needs: styled input box (glowing border), cursor blink animation, mobile keyboard buttons with press feedback, better layout/spacing, character limit indicator. |
| H13 | Victory screen | drawVictory() 3766-3887 | ★★★☆☆ | HIGH | Has celebration but feels flat. Needs: confetti/fireworks particle system, dramatic title reveal animation, stat counters that count up, golden color theme, triumphant feeling. |
| H14 | Game Over screen | drawGameOver() 3889-4021 | ★★★☆☆ | HIGH | Functional. Needs: impactful title (shake/flash), fire/smoke particles from destroyed city, score reveal animation, rating badge visual (not just text), somber color theme. |
| H15 | Siren intro | drawSirenIntro() 2953-2979 | ★★★☆☆ | MEDIUM | Red pulsing overlay. Add: siren sound wave visual (concentric rings), more dramatic red strobing, text size pulse, "incoming threat" military alert style. |
| H16 | City intro | drawCityIntro() 2981-3056 | ★★★☆☆ | MEDIUM | City name + progress. Add: city silhouette preview, dramatic text reveal (slide in from side), progress dots instead of bar, atmospheric intro feel. |
| H17 | Wave transition | drawWaveTransition() 3244-3304 | ★★★☆☆ | MEDIUM | Text messages. Add: wave number large and dramatic (military stencil style), enemy type preview icons, breather animation (calm sky moment). |

---

## Sprint Groupings

### Sprint 1 — High Impact (Most Visible During Gameplay)
Focus: Things players stare at every second
- D2: Rocket exhaust flame (CRITICAL)
- D3: Rocket smoke trail (CRITICAL)
- E1: Explosion radial gradient (CRITICAL)
- A1-A3: Sky gradients (all 3 states)
- A5-A6: Clouds (day + sunset)
- A11-A12: Sea water + sparkles

### Sprint 2 — Core Game Objects (MOSTLY COMPLETE)
Focus: The things you're shooting at and shooting with
- ~~D1: Rocket body detail~~ ✅ (Realism pass 2026-03-14)
- ~~D4: Drone body → Shahed delta-wing~~ ✅ (Realism pass 2026-03-14)
- ~~D6: Ballistic body~~ ✅ (Realism pass 2026-03-14)
- ~~D10: Interceptor (Tamir) body~~ ✅ (Realism pass 2026-03-14)
- ~~D11: Interceptor engine~~ ✅ (Realism pass 2026-03-14)
- D12: Interceptor trail (remaining)
- G6: Crosshair redesign (remaining)
- **Also completed:** C1-C4 battery realism upgrade, HiDPI canvas scaling

### Sprint 3 — Feedback & Effects
Focus: Juice that makes the game feel alive
- E2: Debris particles (shapes, rotation)
- E3: Fire particles
- E6: Shield block effect
- F1-F4: All power-up visuals
- G3: City endurance bar
- G2: Ammo bar
- D8: Ballistic heavy trail
- D5: Drone trail

### Sprint 4 — HUD & UI Polish
Focus: Information presentation
- G1: Score display animation
- G4: Combo counter effects
- G8: Kill feed styling
- G10-G11: Shield + power indicators
- B11: Window rendering (all buildings)
- A4: Stars upgrade

### Sprint 5 — Screens & Menus
Focus: First and last impression
- H1: Title text styling
- H4-H5: Menu buttons polish
- H7-H8: Play counter + share buttons
- H12: Name input screen
- H13: Victory screen (confetti/fireworks)
- H14: Game Over screen (dramatic)
- H15-H17: Intro/transition screens

---

## Completion Tracking

| Sprint | Elements | Completed | Last Updated |
|--------|----------|-----------|-------------|
| Sprint 1 | 10 elements | 9/10 | 2026-03-14 |
| Sprint 2 | 7 elements | 0/7 | — |
| Sprint 3 | 10 elements | 0/10 | — |
| Sprint 4 | 6 elements | 0/6 | — |
| Sprint 5 | 9 elements | 0/9 | — |
| **Total** | **42 elements** | **9/42** | 2026-03-14 |

---
