# Game Aggregator Submission Guide

> These are the platforms where Iron Dome can be listed for **passive discovery** — players find the game while browsing these sites. Once submitted, they drive traffic indefinitely with zero maintenance.

---

## Tier 1: High Impact (submit first)

### itch.io
- **URL:** https://itch.io
- **Audience:** Millions of indie game players
- **How:** Create account → Upload new project → Select "HTML" → ZIP index.html → Check "plays in browser" → Publish
- **Effort:** 10 minutes
- **Requirements:** ZIP file with index.html at root, cover image (630×500), description, screenshots
- **Revenue:** Optional (can set "pay what you want" or free)
- **Notes:** Tag with: missile-defense, html5, strategy, action, browser-game, israel

### Newgrounds
- **URL:** https://www.newgrounds.com
- **Audience:** Legacy gaming community, millions monthly
- **How:** Create account → Submit Game → Upload HTML5 → Add metadata
- **Effort:** 15 minutes
- **Requirements:** HTML5 game file, description, genre tags, rating
- **Notes:** Strong community, reviews and ratings drive visibility

### GameJolt
- **URL:** https://gamejolt.com
- **Audience:** Indie game community
- **How:** Create account → Add Game → Web Playable → Upload
- **Effort:** 10 minutes
- **Requirements:** Similar to itch.io

---

## Tier 2: High Reach (SDK integration may be needed)

### CrazyGames
- **URL:** https://developer.crazygames.com
- **Audience:** 35M+ monthly players
- **How:** Developer account → Submit game → Integrate CrazyGames SDK (for ads)
- **Effort:** 2-4 hours (SDK integration)
- **Requirements:** SDK integration required, strict file size limits, game covers + description
- **Revenue:** Ad revenue share
- **Notes:** Worth the SDK effort due to massive audience. Use embed mode (`?embed=true`).

### Poki
- **URL:** https://developers.poki.com
- **Audience:** 50M+ monthly players
- **How:** Submit via developer portal → Hand-curated review
- **Effort:** 1-2 hours (SDK integration)
- **Requirements:** Poki SDK, <8MB initial download, mobile controls, thumbnails
- **Revenue:** Ad revenue share
- **Notes:** Curated — not all games accepted. High quality required.

### GameDistribution
- **URL:** https://gamedistribution.com/developers
- **Audience:** 350M+ monthly users across 4000+ portals
- **How:** Register → Upload ZIP → Integrate GD SDK → Request activation
- **Effort:** 2-4 hours (SDK integration)
- **Requirements:** GD SDK for ads, metadata, game covers
- **Revenue:** Ad revenue share (high eCPMs)
- **Notes:** Once accepted, game auto-distributes to 4000+ sites. Massive reach.

---

## Tier 3: Additional Portals (low effort, incremental reach)

### Kongregate
- **URL:** https://www.kongregate.com
- **Notes:** Legacy platform, still has traffic

### html5games.com
- **URL:** https://html5games.com
- **Notes:** Curated HTML5 game directory

### GameFlare
- **URL:** https://distribution.gameflare.com
- **Notes:** Game distribution for publishers

### GameMonetize
- **URL:** https://gamemonetize.com
- **Notes:** Similar to GameDistribution

---

## Embed Code (for all platforms)

```html
<iframe src="https://iron-dome-game.com/?embed=true" width="1100" height="700" frameborder="0" allowfullscreen></iframe>
```

---

## Submission Priority Order

1. **itch.io** (10 min, huge audience, no SDK needed)
2. **Newgrounds** (15 min, strong community)
3. **GameJolt** (10 min, indie community)
4. **CrazyGames** (2-4h, 35M players, needs SDK)
5. **GameDistribution** (2-4h, 350M reach via 4000+ sites, needs SDK)
6. **Poki** (1-2h, curated, 50M players, needs SDK)

*Tier 1 can be done today. Tier 2 needs SDK integration sessions by the Marketer agent.*
