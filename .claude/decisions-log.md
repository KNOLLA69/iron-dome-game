# Iron Dome Game — Design Decisions Log

> This log tracks every significant design decision made for the Iron Dome game.
> Every future decision must be appended here with rationale before implementation.
> This is **shared memory across all agents** — consult it before proposing changes that might contradict past decisions.

---

## Entry Template

```markdown
### DECISION-NNN: [Short Title]
- **Date:** YYYY-MM-DD
- **Agent:** Designer / UX Tester / Coder
- **Context:** Why this came up
- **Decision:** What we chose
- **Alternatives Considered:** What else was possible
- **Rationale:** Why this choice wins
- **Impact:** What changed (or what brief this responds to)
```

---

## Historical Decisions (Reconstructed)

### DECISION-001: Single-File Architecture
- **Date:** 2025-12 (estimated)
- **Context:** Needed a deployment strategy for a browser game on GitHub Pages
- **Decision:** Keep everything (HTML, CSS, JS) in a single `index.html` file
- **Alternatives Considered:** Multi-file with bundler (Webpack/Vite), ES modules
- **Rationale:** Zero build step, instant deploy to GitHub Pages, easy to share/fork, no dependencies to manage. For a canvas game under 5000 lines this is practical and fast.
- **Impact:** All code lives in `/index.html` (~4050 lines). No build pipeline needed.

### DECISION-002: Canvas 2D Over WebGL
- **Date:** 2025-12 (estimated)
- **Context:** Choosing rendering technology for the game
- **Decision:** HTML5 Canvas 2D API
- **Alternatives Considered:** WebGL (via Three.js/PixiJS), SVG, DOM-based
- **Rationale:** Canvas 2D is simpler to code, no shader knowledge required, perfect for 2D sprite/particle work. WebGL overkill for this scope. SVG too slow for hundreds of particles. Game runs smooth 60fps with Canvas 2D.
- **Impact:** All rendering uses `ctx` (CanvasRenderingContext2D). No external rendering libraries.

### DECISION-003: Five Israeli Cities with Real Landmarks
- **Date:** 2025-12 (estimated)
- **Context:** Game progression needed distinct stages with emotional connection
- **Decision:** 5 real Israeli cities: Nahariya, Beer Sheva, Haifa, Jerusalem, Tel Aviv — each with iconic landmarks
- **Alternatives Considered:** Generic/fictional cities, random city names, single city
- **Rationale:** Real cities create emotional stakes for Israeli players. Landmarks (Azrieli towers, Western Wall area, Bahai Gardens, Nahariya lighthouse) add visual variety and recognition. Progression south→north→center mirrors real Iron Dome deployment geography.
- **Impact:** CITIES array (lines 147-338) defines 5 cities with building configs and landmarks. Each city has unique skyline via generateCityCanvas().

### DECISION-004: Day/Night/Sunset Cycle Tied to Waves
- **Date:** 2025-12 (estimated)
- **Context:** Visual variety across 15 waves to prevent monotony
- **Decision:** 3-phase cycle per city: wave 1=day, wave 2=sunset, wave 3=night. Repeats per city.
- **Alternatives Considered:** Fixed time of day, real-time clock-based, random
- **Rationale:** Tied to waves ensures every player sees all lighting states. 3 states per city = visual freshness every wave. Night waves feel more intense (tracer-like missiles against dark sky). Sunset creates dramatic warm tones.
- **Impact:** `getTimeOfDay(wave)` function (lines 335-338). Sky gradient, star alpha, cloud alpha, sea colors, sun/moon all respond to time state.

### DECISION-005: Pre-Rendered City Canvases
- **Date:** 2025-12 (estimated)
- **Context:** Drawing 20+ buildings with windows every frame at 60fps was expensive
- **Decision:** Pre-render each city's skyline to an off-screen canvas once, then drawImage() each frame
- **Alternatives Considered:** Draw buildings every frame, use sprites/images
- **Rationale:** Massive performance win — one drawImage() call vs hundreds of rect/arc calls per frame. Buildings are static so no visual cost. Same approach used for battery.
- **Impact:** `generateCityCanvas()` (lines 1135-1177) creates off-screen canvas. `cityCanvas` variable drawn once per frame in draw().

### DECISION-006: Boss System Removed — 3 Plain Waves Per City
- **Date:** 2026-03-13
- **Context:** v3 overhaul inspired by competitor (irondome.fun). Bosses felt arcade-y and unrealistic for a military defense sim. Competitor used wave-based progression without bosses.
- **Decision:** Remove entire boss system. Each city = exactly 3 waves of normal enemies. 5 cities x 3 waves = 15 waves to victory.
- **Alternatives Considered:** Keep bosses but redesign them, add mini-bosses, make bosses optional
- **Rationale:** Bosses broke immersion — Iron Dome defends against missile barrages, not single big targets. 3-wave structure is cleaner, allows difficulty curve via enemy mix. More missiles per wave compensates for removed boss challenge.
- **Impact:** Removed ~150 lines: drawBoss(), drawBossIntro(), boss collision, boss state machine. Cleaned all `if(boss)` references. Wave config simplified.

### DECISION-007: Four Enemy Types
- **Date:** 2026-03-13
- **Context:** Single missile type was boring. Competitor had visual variety. Real Iron Dome faces diverse threats.
- **Decision:** 4 types: Rocket (basic), Drone/UAV (fast wobble), Ballistic (slow tanky 3HP), Decoy Flare (fast tiny swarm)
- **Alternatives Considered:** 2 types only, 6+ types, enemy formations
- **Rationale:** 4 types = enough variety without overwhelming. Each type demands different player strategy: rockets=easy, drones=track the wobble, ballistic=focus fire, decoys=ignore or panic. Weighted spawning per wave creates natural difficulty curve.
- **Impact:** ENEMY_TYPES config (lines 1766-1771), getEnemyWeights() (lines 1636-1642), spawnMissile() accepts type, per-type rendering (~160 lines in draw), multi-HP collision logic.

### DECISION-008: Proximity Homing Interceptors
- **Date:** 2026-03-13
- **Context:** Original collision was rigid 28px circle — felt random and unsatisfying. Players would click precisely but miss due to timing.
- **Decision:** Smart interceptor with proximity guidance: ±30° detection cone, 60px range, 0.15 lerp steering toward nearest target. Plus 30px blast radius at detonation point.
- **Alternatives Considered:** Bigger collision radius (too easy), auto-aim (no skill), area-of-effect only
- **Rationale:** Creates "guided missile" feel that's authentic to real Tamir interceptors. Player still needs to aim in roughly the right direction (skill), but the interceptor does the fine-tuning (satisfaction). Blast radius rewards aiming into clusters.
- **Impact:** Interceptor update loop rewritten with heading tracking, cone search, lerp steering. Detonation creates blast area affecting multiple nearby missiles.

### DECISION-009: City Endurance Replaces Lives/Hearts
- **Date:** 2026-03-13
- **Context:** Hearts/lives system felt too arcade. Losing a life per missile hit was punishing and not realistic.
- **Decision:** Each city has an endurance HP (5-10 based on city). Missiles that reach ground reduce HP. HP accumulates across all 3 waves for a city. City destroyed (HP=0) = game over. HP resets on new city.
- **Alternatives Considered:** Keep hearts but more of them, percentage health bar, no fail state
- **Rationale:** Endurance system feels like real city defense — you can take some hits but not too many. Different HP per city creates natural difficulty (small Nahariya=5HP, big Tel Aviv=10HP). Accumulation across waves means early mistakes compound.
- **Impact:** Replaced `lives` variable with `cityDamage`/`cityMaxHP`. HUD shows green→yellow→red endurance bar. Game over when cityDamage >= cityMaxHP.

### DECISION-010: Combo Triggers Power-Up Drops
- **Date:** 2026-03-13
- **Context:** Power-ups needed a skill-based spawn mechanism, not random drops
- **Decision:** Every x5 combo milestone spawns a random power-up. Combos timeout after 2 seconds of no kills.
- **Alternatives Considered:** Random drops per kill (5% chance), timed spawns, shop between waves
- **Rationale:** Skill-based: only players who chain kills quickly get power-ups. Creates exciting "keep the combo going" tension. x5 threshold is achievable but requires good aim. Combo timeout prevents camping.
- **Impact:** addCombo() function (lines 1803-1814) checks `combo % 5 === 0` and calls spawnPowerUp(). Power-up floats down for collection.

### DECISION-011: Three Power-Up Types
- **Date:** 2026-03-13
- **Context:** Needed meaningful combat buffs that feel impactful
- **Decision:** Shield (blocks 1 city hit), Rapid Fire (15 seconds unlimited ammo), EMP (destroy all on-screen enemies)
- **Alternatives Considered:** Speed boost, double points, slow-motion, extra interceptor speed
- **Rationale:** Each addresses a different threat: Shield=insurance vs ballistic leaking through, Rapid=handle decoy swarms without ammo worry, EMP=emergency clear when overwhelmed. All three feel powerful and satisfying to use.
- **Impact:** collectPowerUp() (lines 1873-1895) with per-type logic. Shield sets shieldHP=1, Rapid sets 900-frame timer with unlimited ammo, EMP destroys all missiles with screen flash.

### DECISION-012: 4x Ammo Multiplier
- **Date:** 2026-03-13
- **Context:** Players were running out of ammo too quickly with 2x multiplier, especially against multi-HP ballistic missiles
- **Decision:** maxAmmo = totalThreats × 4 (was × 2)
- **Alternatives Considered:** ×3, unlimited ammo, ammo pickups
- **Rationale:** 4× feels generous — player can miss ~75% of shots and still clear the wave. Removes frustration while keeping ammo bar relevant as visual feedback. Ballistic missiles consuming 3+ interceptors each make 4× actually necessary.
- **Impact:** Single line change in wave start ammo calculation.

### DECISION-013: Firebase REST API (No SDK)
- **Date:** 2025-12 (estimated)
- **Context:** Needed online leaderboard for competitive element
- **Decision:** Direct Firebase Realtime Database REST API calls via fetch()
- **Alternatives Considered:** Firebase JS SDK, Supabase, custom backend, localStorage only
- **Rationale:** REST API = zero dependencies, ~20 lines of code. Firebase JS SDK would add 100KB+ to a single-file game. No auth needed for public leaderboard. Free tier handles hobby traffic.
- **Impact:** fetchLeaderboard(), submitScore() functions (lines 360-411) using fetch() to Firebase REST endpoints.

### DECISION-014: Country Detection via api.country.is
- **Date:** 2025-12 (estimated)
- **Context:** Wanted flag emoji next to leaderboard names for international flair
- **Decision:** Detect player country via api.country.is (free, no-auth IP geolocation)
- **Alternatives Considered:** Ask player to select country, use browser locale, skip flags
- **Rationale:** Automatic and invisible to user. Free API with no rate limits for our scale. Country code → flag emoji conversion is simple. Adds competitive international element to leaderboard.
- **Impact:** detectCountry() function stores country code, passed to submitScore(). Leaderboard renders flag emoji per entry.

### DECISION-015: Flag-Based Language Selector
- **Date:** 2026-03-13
- **Context:** Text buttons (עב, EN, DE) were small, hard to tap on mobile, and not visually intuitive
- **Decision:** Flag emoji buttons (🇮🇱, 🇺🇸, 🇩🇪) with text labels, larger hit areas (fz(44)×fz(44)), active glow
- **Alternatives Considered:** Dropdown select, auto-detect from browser, icon-only
- **Rationale:** Flags are universally recognizable. Larger buttons solve mobile tap issues. Active glow provides clear state feedback. Text labels (עב/EN/DE) below flags ensure clarity.
- **Impact:** drawLangToggle() rewritten (lines 3306-3343). langBtnRects[] for hit testing.

### DECISION-016: Big Menu Buttons (Competitor-Inspired)
- **Date:** 2026-03-13
- **Context:** Competitor (irondome.fun) had large, professional-looking menu buttons vs our small text links
- **Decision:** Full-width rounded buttons with gradients, icons, and hover states: New Game (green), Leaderboard (blue), Contact (gray)
- **Alternatives Considered:** Keep minimal text links, sidebar menu, animated menu
- **Rationale:** Big buttons look professional, are mobile-friendly, and guide the player clearly. Green for primary action (play), blue for secondary (scores), gray for tertiary (contact). Matches modern mobile game conventions.
- **Impact:** drawMenu() rewritten (lines 3409-3553), drawMenuButton() helper (lines 3382-3407).

### DECISION-017: Share Buttons (4 Platforms)
- **Date:** 2026-03-13
- **Context:** Competitor had share functionality. Viral sharing is free marketing.
- **Decision:** WhatsApp, X/Twitter, Email, Copy Link — four small square buttons
- **Alternatives Considered:** Native Web Share API only, more platforms (Facebook, Telegram), QR code
- **Rationale:** WhatsApp dominates Israeli messaging (primary audience). X/Twitter for gaming community. Email for direct sharing. Copy link as universal fallback. Web Share API has inconsistent support. Four buttons fit nicely in a row.
- **Impact:** Share button rendering in drawMenu(), click handlers with platform-specific URLs, i18n share text.

### DECISION-018: Legal Overlay (3 Tabs)
- **Date:** 2026-03-13
- **Context:** Professional games need Terms, Privacy Policy, and Disclaimer. Required for GDPR compliance and legitimacy.
- **Decision:** Canvas-drawn overlay with 3 tab buttons (Terms | Privacy | Disclaimer), scrollable text, close button
- **Alternatives Considered:** Separate HTML pages, external links, footer text only
- **Rationale:** In-game overlay keeps player in the game context. Tabs are compact. Canvas rendering matches game aesthetic. No navigation away from game = better UX.
- **Impact:** drawLegalOverlay() function (lines 3555-3665), showLegalOverlay/legalTab/legalScrollY state vars, click handlers for tabs and close.

### DECISION-019: Play Counter via Firebase
- **Date:** 2026-03-13
- **Context:** Competitor showed play count as social proof ("X games played")
- **Decision:** Firebase path `/stats/totalPlays` — increment on each game start, display on menu
- **Alternatives Considered:** Umami event count, local counter only, no counter
- **Rationale:** Real-time global count creates social proof and FOMO. Firebase already in use for leaderboard so no new dependency. Simple increment via REST.
- **Impact:** fetchPlayCount(), incrementPlayCount() (lines 414-434). Counter displayed in drawMenu().

### DECISION-020: IDF-Accurate Battery Redesign
- **Date:** 2026-03-13
- **Context:** Original battery was a simple colored box. Needed military realism.
- **Decision:** Detailed olive drab truck with frame rails, dual axle wheels with lug nuts, stabilizer jacks, radar/sensor dish with antenna, cable bundle, Star of David marking. Canvas: 180×100.
- **Alternatives Considered:** Photo-based sprite, simpler truck, multiple battery positions
- **Rationale:** Hand-drawn Canvas matches game's procedural art style. Military details (radar dish, stabilizer jacks, cable bundle) add authenticity. Olive drab (#4a5a3a) is correct IDF color. Star of David subtle nod to IDF identity.
- **Impact:** prerenderBattery() rewritten (lines 1263-1377). Canvas increased from 140×80 to 180×100.

### DECISION-021: Launcher Tubes 4×5 Grid
- **Date:** 2026-03-13
- **Context:** Real Iron Dome launcher has 20 Tamir missiles in a rectangular container
- **Decision:** 4 rows × 5 columns of tube openings, with rivets, housing, and hydraulic tilt arm
- **Alternatives Considered:** Single barrel, abstract launcher shape, turret style
- **Rationale:** 4×5=20 tubes matches real Iron Dome launcher specs. Tube openings create visual depth. Hydraulic arm suggests the launcher can rotate (which it does in gameplay via batteryAngle).
- **Impact:** drawLauncherTubes() (lines 1381-1466) with rotation transform.

### DECISION-022: Umami Analytics Over Google Analytics
- **Date:** 2026-03-13
- **Context:** Needed usage analytics without heavy tracking
- **Decision:** Umami Cloud (free tier) — privacy-focused, no cookies, lightweight script
- **Alternatives Considered:** Google Analytics 4, Plausible, no analytics
- **Rationale:** Umami respects user privacy (no cookies = no consent banner needed). GDPR-friendly. Free cloud tier sufficient. Lightweight script (~2KB). Custom events for game-start, game-end, share, leaderboard-submit.
- **Impact:** Script tag in `<head>` with TODO for site ID. umami.track() calls at key game events.

### DECISION-023: Kill Feed (Bottom-Left)
- **Date:** 2026-03-13
- **Context:** Players had no feedback about which enemy types they were destroying
- **Decision:** Bottom-left corner shows last 3 kills with enemy type emoji, name, and score colored by type
- **Alternatives Considered:** Top-right feed, center screen popups, sound-only feedback
- **Rationale:** Bottom-left is the least used screen area (battery is center-bottom, score is top-right). Last 3 keeps it compact. Color-coding per type reinforces enemy recognition. Fades naturally.
- **Impact:** killFeed[] array, rendering in draw() (lines 2891-2902).

### DECISION-024: Screen Shake + Flash for Impact
- **Date:** 2025-12 (estimated)
- **Context:** Explosions felt flat without camera feedback
- **Decision:** Random shake offset (shakeX/shakeY) with ease-out duration on big events. White screen flash on EMP and big explosions.
- **Alternatives Considered:** No screen effects, chromatic aberration, slow-motion
- **Rationale:** Shake is the industry standard for impact feel — simple to implement, universally effective. Flash draws attention to major events. Both are cheap to render (translate + overlay).
- **Impact:** shakeX/shakeY/shakeDuration in update loop, ctx.translate() in draw(), screenFlash white overlay.

### DECISION-025: Banner Airplane as Ambient Detail
- **Date:** 2025-12 (estimated)
- **Context:** Sky felt empty during gameplay, especially in day mode
- **Decision:** Periodic airplane crossing the sky pulling a banner (every ~30 seconds / 1800 frames)
- **Alternatives Considered:** Birds, helicopters, weather effects, satellites
- **Rationale:** Adds life to the sky without gameplay distraction. Banner can carry branding or messages. Airplane silhouette is simple to render. Periodic timing prevents it from being distracting.
- **Impact:** Airplane rendering in draw() (lines 2770-2871), AIRPLANE_INTERVAL constant (1800 frames).

---

### DECISION-026: Multi-Layer Rocket Exhaust Flame
- **Date:** 2026-03-14
- **Context:** Sprint 1 — rocket exhaust was a single gradient triangle (★★). Most visible gameplay element.
- **Decision:** 4-layer flame rendering: outer red glow (widest, faintest), orange mid flame, yellow inner flame, white-hot core (brightest, smallest). Sine-based flicker with compound frequency for natural randomness. Nozzle glow ring at base. Ballistic missiles get larger/redder flames.
- **Alternatives Considered:** Particle-based flame (too expensive per-frame for every missile), sprite sheet animation (no Canvas sprite system), 2-layer simple upgrade
- **Rationale:** 4 layers creates convincing depth at low render cost. Quadratic curves for each layer maintain smooth shape. Compound sine flicker (multiple frequencies) avoids repetitive animation. White-hot core is physically accurate (hottest part of flame).
- **Impact:** Replaced 14 lines with ~55 lines in rocket/ballistic rendering section. No performance concern — gradient fills are GPU-accelerated.

### DECISION-027: Volumetric Smoke Trail System
- **Date:** 2026-03-14
- **Context:** Sprint 1 — rocket trails were simple gradient circles (★★). Most visible element in gameplay — players stare at trails constantly.
- **Decision:** Per-type trail rendering: Rockets/Ballistic get volumetric smoke puffs (expanding radial gradients with wind drift + turbulence, warm-to-gray color transition, hot core near engine). Drones get heat shimmer effect (no smoke — drones don't produce exhaust smoke). Decoys get tiny spark trails.
- **Alternatives Considered:** Single unified trail for all types (less visual variety), particle-based trail (too many particles), pre-rendered trail texture
- **Rationale:** Type-specific trails reinforce enemy identification at a glance. Expanding puffs with wind drift create convincing volumetric smoke without particle overhead. Drones having heat shimmer instead of smoke is physically accurate and visually distinct.
- **Impact:** Replaced 14-line unified trail renderer with ~60-line per-type system. Performance: uses radial gradients per trail point (max 20-30 per missile), acceptable for ~10-15 missiles on screen.

### DECISION-028: Shockwave Ring + Asymmetric Explosions
- **Date:** 2026-03-14
- **Context:** Sprint 1 — explosions were basic radial gradients (★★★). Needed more visual impact.
- **Decision:** Added: (1) expanding shockwave ring (thin circle, fast expansion, rapid fade), (2) asymmetric fireball center (random offset from explosion origin), (3) internal bright flash (first few frames only), (4) secondary delayed fireball for big explosions, (5) spark particles with motion blur (elongated ellipses in direction of travel).
- **Alternatives Considered:** Pre-rendered explosion sprite sheet, more gradient layers without shockwave, distortion shader (not available in Canvas 2D)
- **Rationale:** Shockwave ring is the single highest-impact visual addition — it sells the concussive force of explosions. Asymmetric center prevents the "perfect circle" look that screams "video game." Flash timer creates brief intense moment. Secondary fireball adds drama to big explosions cheaply.
- **Impact:** Modified boom() to store shockwave/asymmetry/flash data. Explosion update loop handles shockwave expansion. Rendering from ~22 lines to ~50 lines. Particle renderer gained spark streak branch with ellipse motion blur.

### DECISION-029: Enhanced Sky Gradients (All 3 Time States)
- **Date:** 2026-03-14
- **Context:** Sprint 1 — sky gradients had 6 color stops each (★★★). Needed more depth and atmosphere.
- **Decision:** Day: 10 stops (cerulean → blues → warm atmospheric haze at horizon). Sunset: 12 stops (twilight purple → magenta → rose → deep red → orange → golden glow). Night: 11 stops (near-black zenith → deep navy → midnight blue → subtle warm horizon).
- **Alternatives Considered:** Procedural sky with Rayleigh scattering simulation (too expensive), texture-based sky (no texture loading system), keeping 6 stops with better colors
- **Rationale:** More color stops create smoother, more photographic gradients. Warm horizon haze (day) and golden glow (sunset) are key Mediterranean atmospheric effects. Night sky benefits from very gradual transitions to avoid visible banding in dark colors.
- **Impact:** Sky gradient section grew from 18 to 30 lines. No performance impact — linear gradients are trivially cheap.

### DECISION-030: Volumetric Cloud Rendering (3-Pass)
- **Date:** 2026-03-14
- **Context:** Sprint 1 — clouds were 4 flat single-color ellipses (★★). Needed fluffy volumetric feel.
- **Decision:** 3-pass rendering per cloud: (1) shadow/underside pass (offset down, darker), (2) main body pass (7 lobes with radial gradients), (3) bright top highlights (sunlit edges, day/sunset only). Time-of-day colors: Day = white body + cool blue shadow + white highlights. Sunset = warm orange body + orange-pink underlit shadow + golden highlights. Night = muted dark blue-gray.
- **Alternatives Considered:** More ellipses with simple fills (still flat), procedural noise clouds (expensive), pre-rendered cloud sprites
- **Rationale:** 3-pass approach creates convincing depth: shadow gives bottom weight, main body gives shape, highlights give sunlit top edge. Radial gradients on each lobe create soft, fluffy edges naturally. 7 lobes provide enough shape complexity for realistic cumulus appearance.
- **Impact:** Cloud rendering from 12 lines to ~80 lines. Performance: ~21 radial gradients per cloud × ~8 clouds = ~170 gradient fills. Acceptable — gradient fills are fast.

### DECISION-031: Sea Water Reflection Path System
- **Date:** 2026-03-14
- **Context:** Sprint 1 — sea had random dot sparkles (★★) and basic gradient (★★★). No focused reflection path.
- **Decision:** Added: (1) 5-stop sea gradients per time state, (2) animated wave ripple lines at surface (3 overlapping sine waves), (3) concentrated sun/moon reflection path (vertical column of sparkle streaks under celestial body), (4) sunset golden glow radial gradient on water, (5) depth-based ambient shimmer, (6) city light reflections with glow.
- **Alternatives Considered:** Full wave simulation (too expensive), reflection mapping (no framebuffer support), simple gradient-only enhancement
- **Rationale:** The "golden path" reflection column is the single most impactful sea visual — it creates the iconic sunset-on-water look. Concentrating sparkles in a column under the sun/moon instead of random distribution looks dramatically more realistic. Wave ripple lines at surface sell the water surface tension.
- **Impact:** Sea section rewritten from ~80 lines to ~100 lines. Added 60-80 sparkle streak fills per frame for reflection path — lightweight rect fills.

---

### DECISION-032: UX Round 1 Findings — Onboarding is the #1 Gap
- **Date:** 2026-03-14
- **Agent:** UX Tester
- **Context:** First UX audit session (Round 1: Cold Start + Core Loop). Tested A1-A7 and B1-B10 (17 test cases). Played through multiple game starts as a first-time English-speaking player on desktop 1280x800.
- **Decision:** Documented 5 briefs (BRIEF-001 through BRIEF-005). The single highest-impact finding is: **there are zero instructions for first-time players**. No tutorial, no "click to fire" hint, no enemy type introduction. This caused consistent game-over at wave 1 in test runs.
- **Alternatives Considered:** N/A (observation, not design choice)
- **Rationale:** A game that expects players to already know the controls will have high first-session churn. The visual polish (Sprint 1) is excellent, but a first-time player can't appreciate it if they don't know how to play. Onboarding should be addressed before further visual polish.
- **Impact:** 5 briefs filed. Priority recommendation: BRIEF-001 (controls tutorial, P1) should be implemented first, followed by BRIEF-002/003 (HUD bugs, P2), then BRIEF-004/005 (game over explanation + enemy intro, P2).

---

### DECISION-033: HiDPI Canvas Scaling (devicePixelRatio)
- **Date:** 2026-03-14
- **Agent:** Visual Designer
- **Context:** User requested higher graphics quality/sharpness. Canvas was rendering at 1x resolution regardless of display DPI, causing blurriness on retina/HiDPI screens.
- **Decision:** Multiply canvas buffer dimensions by `window.devicePixelRatio`, apply `ctx.scale(dpr, dpr)` for all canvases (main, city, battery). Use logical W/H for all game coordinates. Fix `drawImage()` calls to specify logical dimensions.
- **Alternatives Considered:** CSS `image-rendering: pixelated` (wrong aesthetic), SVG overlay (incompatible with Canvas API).
- **Rationale:** The single most impactful sharpness improvement. On 2x displays, all text, lines, and gradients now render at double resolution. Zero gameplay impact since all coordinates remain in logical space.
- **Impact:** All visual elements are 2x sharper on retina displays. Off-screen canvases (city, battery) also benefit.

### DECISION-034: Drone Redesign — Quad-rotor → Shahed-136 Delta-Wing
- **Date:** 2026-03-14
- **Agent:** Visual Designer
- **Context:** User provided reference photo of Shahed-136 drone and requested realistic enemy designs. The existing quad-rotor drone was unrealistic for a military kamikaze drone scenario.
- **Decision:** Complete redesign: delta wings with swept-back shape, narrow cylindrical fuselage, pointed nose cone with dark seeker, V-tail stabilizers, rear pusher propeller with spinning blur animation, olive-green military color scheme, warhead marking band.
- **Alternatives Considered:** Keep quad-rotor with better detail (rejected — wrong aircraft type), flying wing design (rejected — Shahed has distinct fuselage).
- **Rationale:** The Shahed-136 is the actual drone used in the real-world conflict this game references. Accurate representation improves both realism and educational value.
- **Impact:** D4 element completely replaced. Hitbox unchanged (22px). Trail system (heat shimmer) already suited for fixed-wing design.

### DECISION-035: Realistic Projectile Bodies (Rocket, Ballistic, Tamir)
- **Date:** 2026-03-14
- **Agent:** Visual Designer
- **Context:** User provided reference photos of Iranian missile and real Iron Dome battery, requesting maximum realism.
- **Decision:** Three projectile upgrades: (1) Rocket: Qassam/Grad-style steel tube with 6-stop 3D cylinder gradient, ogive nose cone, weld seams, sheet-metal fins. (2) Ballistic: Fajr/Fateh-style with separate red warhead section, body segment bands, larger swept fins. (3) Tamir interceptor: slender white body with seeker dome, canard + tail fins, engine nozzle detail, curved exhaust plume.
- **Alternatives Considered:** Keep simple shapes with just color changes (rejected — user specifically asked for photo-reference realism).
- **Rationale:** Reference-photo-based designs create visual authenticity. Each enemy type now has a distinct silhouette recognizable at a glance.
- **Impact:** D1, D6, D10, D11 elements upgraded. Body dimensions slightly increased (rocket: 12→14, ballistic: 18→22) for more detail room.

### DECISION-036: Iron Dome Battery Realism Upgrade
- **Date:** 2026-03-14
- **Agent:** Visual Designer
- **Context:** Reference photo of actual Iron Dome battery system showed key details missing from game version.
- **Decision:** Enhanced battery with: truck cab with windshield, undercarriage shadow, phased-array radar panel (EL/M-2084 style) replacing simple dish, radial tire tread patterns, metallic rim gradients, hydraulic jack detail, cable highlight, warning light glow.
- **Alternatives Considered:** Complete battery redesign from scratch (rejected — existing layout was solid, just needed detail enhancement).
- **Rationale:** The battery is the player's avatar — it should look as authentic as possible. The phased-array radar panel is the most recognizable element of the real system.
- **Impact:** C1-C4 elements upgraded. Canvas dimensions unchanged (180×100). Pre-render approach preserved.

---

## Future Decisions

> Append new decisions below this line. Always include date, context, alternatives, and rationale.
> Number sequentially from DECISION-037 onward.

---
