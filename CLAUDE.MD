# CLAUDE.MD — Iron Dome Game: Multi-Agent Project Hub

> **This file is auto-loaded by Claude Code.** It contains shared project knowledge for ALL agents working on this game. Agent-specific personas, standards, and work queues live in separate files under `.claude/agents/`.

---

## Agent Routing

Multiple specialized agents work on this project. **Before beginning work, identify which agent you are** from the session context or user's first message, then load your persona file.

| Agent | Persona File | Role | Edits Code? |
|-------|-------------|------|-------------|
| **Designer** | `.claude/agents/designer.md` | Visual polish, aesthetics, Canvas rendering | Yes |
| **UX Tester** | `.claude/agents/ux-tester.md` | Play-test, evaluate UX, write briefs | No |
| _(Future: Coder)_ | — | Game logic, bugs, features | Yes |
| _(Future: Marketing)_ | — | Content, social, promotion | No |

**Default:** If no agent is specified, default to the **Designer** agent.

**On session start:**
1. Read this file (auto-loaded)
2. Read your agent persona file from `.claude/agents/`
3. Read `.claude/HANDOFF.md` for context from previous sessions
4. Check your work queue (visual-audit.md or ux-audit.md)
5. Check `.claude/briefs/` for pending items

---

## Project Overview

**Game:** Iron Dome — HTML5 Canvas missile defense game
**File:** Single file at `index.html` (~4050 lines, HTML + CSS + JS)
**Engine:** Vanilla Canvas 2D (no frameworks)
**Deployment:** GitHub Pages at `https://knolla69.github.io/iron-dome-game/`
**Dev Server:** `python3 -m http.server 8080` (configured in `.claude/launch.json`)

### Game Summary
- Defend 5 Israeli cities across 15 waves (3 waves per city)
- 4 enemy types: Rocket, Drone (UAV), Ballistic, Decoy Flare
- Proximity-homing interceptors (Tamir missiles)
- Combo system → power-up drops at x5 milestones
- 3 power-ups: Shield, Rapid Fire, EMP
- City endurance HP system (damage accumulates per city)
- Day/sunset/night cycle per city (3 time states)
- Firebase leaderboard + play counter
- 3 languages: Hebrew (🇮🇱), English (🇺🇸), German (🇩🇪)
- Umami analytics (privacy-focused)

---

## Complete Architecture Map

All code is in `index.html`. Here's where everything lives:

### Structure & Data
| Section | Lines | What's There |
|---------|-------|-------------|
| HTML head + CSS | 1-38 | Meta tags, canvas CSS, responsive styles |
| i18n / TEXTS | 46-145 | All translatable strings (he/en/de) |
| CITIES config | 147-338 | 5 cities with buildings, landmarks, endurance |
| getTimeOfDay() | 335-338 | Wave → day/sunset/night mapping |
| Firebase functions | 340-434 | Leaderboard CRUD, country detect, play counter |
| Canvas setup | 439-448 | Responsive sizing, mobile detection |

### Audio
| Section | Lines | What's There |
|---------|-------|-------------|
| Sound engine | 450-755 | Web Audio API synthesis (all SFX + music) |

### Rendering (Pre-computed)
| Section | Lines | What's There |
|---------|-------|-------------|
| Constants & scaling | 757-767 | W, H, GROUND_Y, SEA_Y, scaleX, fz() |
| drawBuilding() | 774-1133 | All 10 building shapes (rect, dome, lighthouse, etc.) |
| generateCityCanvas() | 1135-1177 | Off-screen city skyline pre-render |
| Game state variables | 1180-1248 | All arrays, timers, flags, counters |
| prerenderBattery() | 1263-1377 | IDF truck chassis, radar, wheels, markings |
| drawLauncherTubes() | 1381-1466 | 4×5 tube grid with hydraulic arm |

### Input & Config
| Section | Lines | What's There |
|---------|-------|-------------|
| Event handlers | 1468-1632 | Mouse, touch, keyboard, resize, pause |
| Wave config | 1634-1651 | getWaveConfig(), getEnemyWeights() |

### Game Logic
| Section | Lines | What's There |
|---------|-------|-------------|
| startGame() / startWave() | 1653-1720 | Game init, wave setup, ammo calc |
| fireInterceptor() | 1722-1751 | Spawn interceptor at battery toward mouse |
| ENEMY_TYPES config | 1766-1771 | Speed/HP/hitbox/points per enemy type |
| spawnMissile() | 1773-1801 | Enemy spawn with type selection |
| Combo system | 1803-1825 | addCombo(), getMultiplier() |
| boom() | 1827-1862 | Explosion + particle spawner |
| Power-up system | 1868-1895 | spawnPowerUp(), collectPowerUp() |
| update() | 1898-2198 | Main game loop logic (all physics, collision, homing) |

### Rendering (Per-Frame)
| Section | Lines | What's There |
|---------|-------|-------------|
| draw() | 2200-2951 | **THE MAIN RENDER FUNCTION** — everything visual |
| ↳ Sky gradients | 2202-2231 | Day/sunset/night sky |
| ↳ Stars | 2237-2247 | Twinkling night stars |
| ↳ Clouds | 2249-2271 | Parallax cloud layers |
| ↳ Sun/Moon | 2274-2334 | Celestial bodies per time-of-day |
| ↳ Sea/Water | 2336-2414 | Gradient + sparkles + shimmer |
| ↳ City glow | 2419-2438 | Light pollution overlay |
| ↳ Smoke trails | 2448-2458 | Soft radial puffs |
| ↳ Missiles | 2460-2608 | Per-type rendering (rocket/drone/ballistic/decoy) |
| ↳ Interceptors | 2610-2679 | Tamir body + trail + engine glow |
| ↳ Explosions | 2681-2703 | Multi-layer radial gradients |
| ↳ Particles | 2705-2725 | Debris, fire, sparks |
| ↳ Battery | 2727-2748 | Pre-rendered truck + tubes + muzzle flash |
| ↳ Floating texts | 2757-2768 | Score popups |
| ↳ Airplane | 2770-2871 | Banner plane with waving banner |
| ↳ Power-ups | 2875-2888 | Floating emoji icons |
| ↳ Kill feed | 2891-2902 | Bottom-left recent kills |
| ↳ Screen effects | 2913-2924 | Flash + vignette |
| ↳ Pause overlay | 2927-2945 | Dark screen + resume |

### Screen Functions
| Section | Lines | What's There |
|---------|-------|-------------|
| drawSirenIntro() | 2953-2979 | Red alert pulsing |
| drawCityIntro() | 2981-3056 | City name + progress reveal |
| drawHUD() | 3058-3242 | Score, ammo, combo, wave, crosshair, endurance |
| drawWaveTransition() | 3244-3304 | Wave complete → next wave |
| drawLangToggle() | 3306-3343 | 🇮🇱🇺🇸🇩🇪 flag buttons |
| drawLeaderboardTable() | 3345-3380 | Top 10 scores |
| drawMenuButton() | 3382-3407 | Reusable gradient button |
| drawMenu() | 3409-3553 | Main menu screen (title, buttons, share, legal) |
| drawLegalOverlay() | 3555-3665 | Terms/Privacy/Disclaimer tabs |
| drawNameInput() | 3667-3764 | Score entry with mobile keyboard |
| drawVictory() | 3766-3887 | Victory celebration screen |
| drawGameOver() | 3889-4021 | Defeat screen with rating |
| gameLoop() / resize | 4023-4048 | RAF loop + responsive resize |

---

## Key Configuration Objects

### ENEMY_TYPES (line ~1766)
```
rocket:    { speedMult: 1.0, hp: 1, hitbox: 28, points: 1 }
drone:     { speedMult: 1.4, hp: 1, hitbox: 22, points: 2 }
ballistic: { speedMult: 0.7, hp: 3, hitbox: 35, points: 5 }
decoy:     { speedMult: 2.0, hp: 1, hitbox: 15, points: 1 }
```

### CITIES (lines 147-338)
1. Nahariya (endurance: 5, waves 1-3) — lighthouse landmark
2. Beer Sheva (endurance: 6, waves 4-6) — desert buildings
3. Haifa (endurance: 7, waves 7-9) — port crane, sail tower, Bahai gardens
4. Jerusalem (endurance: 8, waves 10-12) — dome, wall landmarks
5. Tel Aviv (endurance: 10, waves 13-15) — Azrieli towers (round, triangle)

### Power-Up Types
- Shield: blocks 1 city hit (shieldHP = 1)
- Rapid Fire: 15 seconds (900 frames) unlimited ammo
- EMP: destroy all on-screen missiles + screen flash

---

## Cross-Agent Communication Protocols

### Shared Documents
| Document | Owner | Who Reads | Who Writes |
|----------|-------|-----------|------------|
| `decisions-log.md` | All agents | All agents | All agents (append only) |
| `visual-audit.md` | Designer | All agents | Designer only |
| `ux-audit.md` | UX Tester | All agents | UX Tester only |
| `briefs/BRIEF-NNN-*.md` | UX Tester creates | Designer + Coder | UX Tester creates; assigned agent updates status |
| `HANDOFF.md` | All agents | All agents | Current agent at session end |

### Rules
1. **One agent edits `index.html` at a time.** UX Tester never touches code. Designer and Coder should not have overlapping edit sessions. HANDOFF.md tracks who last modified.
2. **Briefs are the contract.** UX Tester files a brief → Designer/Coder picks it up → changes status to IN PROGRESS → implements → changes status to RESOLVED. UX Tester re-tests in next session.
3. **Decision log is append-only.** Never edit past decisions. To reverse a decision, file a new entry that references the old one.
4. **Each agent owns their audit document.** Cross-referencing is fine. Editing someone else's audit is not.
5. **Always update HANDOFF.md** at the end of every session.

### Brief Workflow
```
UX Tester finds issue → Files BRIEF in .claude/briefs/
    → Designer/Coder reads brief
    → Updates status: IN PROGRESS
    → Implements fix
    → Updates status: RESOLVED (with notes)
    → UX Tester re-tests
    → Updates status: VERIFIED (or REOPENED)
```

---

## Reference Documents

| Document | Path | Purpose |
|----------|------|---------|
| Designer Persona | `.claude/agents/designer.md` | Visual design standards, working protocols |
| UX Tester Persona | `.claude/agents/ux-tester.md` | Testing methodology, evaluation criteria |
| Decision Log | `.claude/decisions-log.md` | Every design decision with rationale |
| Visual Audit | `.claude/visual-audit.md` | Designer's element inventory + ratings |
| UX Audit | `.claude/ux-audit.md` | UX Tester's 76 test cases + status |
| Brief Template | `.claude/briefs/TEMPLATE.md` | Cross-agent issue reporting format |
| Handoff Log | `.claude/HANDOFF.md` | Session state transfer between agents |
| Launch Config | `.claude/launch.json` | Dev server setup (python3 http.server 8080) |
| Game Code | `index.html` | The entire game (~4050 lines) |

---

## Quick Commands

```bash
# Start dev server
python3 -m http.server 8080

# Check syntax (basic)
node -e "require('fs').readFileSync('index.html','utf8')" && echo "OK"

# Deploy (push to GitHub Pages)
git add index.html && git commit -m "visual: [description]" && git push
```

---

*Last updated: 2026-03-14*
