# Iron Dome Game — Marketing Growth Backlog

> This is the **Marketer agent's work queue**. Prioritized by impact x effort.
> Focus: organic growth only. Zero budget.
> This document is owned by the Marketer agent. Other agents may reference but should not edit.

---

## Status Key

| Status | Meaning |
|--------|---------|
| 🔴 NOT STARTED | Not yet worked on |
| 🟡 IN PROGRESS | Currently being implemented |
| ✅ DONE | Implemented and verified |
| ⏭️ SKIPPED | Deprioritized (with reason) |

---

## Priority 1: Foundation

_Must be done before any promotion. No point driving traffic to a game with broken social previews and no analytics._

| # | Tactic | Status | Notes |
|---|--------|--------|-------|
| M1 | Add OG meta tags (og:title, og:description, og:image, og:url) | ✅ DONE | DECISION-041. Tags added. OG image pending (BRIEF-012 for Designer). |
| M2 | Add Twitter Card meta tags (twitter:card, twitter:title, twitter:image) | ✅ DONE | DECISION-041. summary_large_image card with hook title. |
| M3 | Configure Umami analytics (create account, set website ID) | ✅ DONE | Website ID: b0ce0ca8-24a3-42e5-bf10-d0a2b90634ee. Tracking: page views, game-start, game-end (score/wave/result). |
| M4 | Add SEO meta description tag | ✅ DONE | DECISION-041. Page title also updated to bilingual. |
| M5 | Test social share previews on WhatsApp, X, Discord, LinkedIn | 🟡 IN PROGRESS | Tags verified in code. Full platform test blocked on og-image.png (BRIEF-012). |

---

## Priority 2: Shareable Moments

_Make the game viral-ready. Every play session should create a share opportunity._

| # | Tactic | Status | Notes |
|---|--------|--------|-------|
| M6 | Include score in share text ("I scored 2,450 on Iron Dome!") | ✅ DONE | DECISION-045. `getShareText()` generates score-embedded text in all 3 languages. Game over: "I scored X points... Can you beat me?" Victory: "I defended all 5 cities... X points!" |
| M7 | Share prompt after game over | ✅ DONE | DECISION-045. Share buttons (WhatsApp, X, email, copy) rendered below Play Again button on game over screen. |
| M8 | Share prompt after victory | ✅ DONE | DECISION-045. Same share row on victory screen. Victory share text uses triumphant framing. |
| M9 | Shareable leaderboard rank | 🔴 NOT STARTED | "I'm #3 worldwide!" — link that shows the leaderboard |

---

## Priority 3: Discovery & SEO

_Make the game findable by people searching for it._

| # | Tactic | Status | Notes |
|---|--------|--------|-------|
| M10 | Add JSON-LD structured data (Game schema) | ✅ DONE | DECISION-042. VideoGame schema with genre, platform, author, free offer. |
| M11 | Submit to indie game directories | 🟡 IN PROGRESS | Submission guide created at `.claude/marketing/game-aggregators.md`. Tier 1 (itch.io, Newgrounds, GameJolt) ready — user to submit. |
| M12 | Create press kit | 🔴 NOT STARTED | Screenshots, description, dev bio, contact. Save to .claude/marketing/ |
| M13 | Optimize page title for search | ✅ DONE | DECISION-041. Bilingual title + SEO description. sitemap.xml + robots.txt deployed (DECISION-044). |

---

## Priority 4: Content & Community

_Create content and seed communities for organic reach._

| # | Tactic | Status | Notes |
|---|--------|--------|-------|
| M14 | Draft social media launch posts | ✅ DONE | Reddit posts for 5 subreddits at `.claude/marketing/reddit-launch-posts.md`. Staggered schedule with unique angles per sub. |
| M15 | Create dev-log content template | 🔴 NOT STARTED | "How I built an Iron Dome game in vanilla JS" — dev community appeal |
| M16 | Identify 10 target communities | ✅ DONE | 5 subreddits identified + 6 game aggregators researched. See reddit-launch-posts.md and game-aggregators.md. |
| M17 | Plan content calendar (weekly rhythm) | 🔴 NOT STARTED | What to post, where, when. Sustainable cadence. |

---

## Priority 5: Advanced Virality

_Growth mechanics that compound over time._

| # | Tactic | Status | Notes |
|---|--------|--------|-------|
| M18 | Referral tracking | 🔴 NOT STARTED | UTM params or custom ref codes. Know which channel brings players. |
| M19 | Configurable airplane banner | 🔴 NOT STARTED | Replace hardcoded personal message with campaign-ready config. |
| M20 | Enhanced social proof on menu | 🔴 NOT STARTED | Show: total plays, countries represented, "Join X players worldwide" |
| M21 | Challenge links | 🔴 NOT STARTED | "Can you beat my score of 2,450?" — personalized share URLs |
| M22 | Embeddable iframe widget (?embed=true) | ✅ DONE | DECISION-043. Clean embed mode hides share/legal, shows attribution link back. |

---

## Growth Metrics to Track

_Once Umami is configured (M3), track these:_

| Metric | How to Measure | Target |
|--------|---------------|--------|
| Daily players | Umami page views | Baseline first, then grow |
| Share rate | Track share button clicks vs games played | >5% of sessions |
| Viral coefficient | New players from shares vs total shares | >0.3 (each 10 shares → 3 new players) |
| Retention | Return visits (Umami) | >20% day-7 return |
| Completion rate | Victory events vs game-start events | Track baseline |
| Bounce rate | Single-page exits | <40% |
| Geographic spread | Country distribution from leaderboard | Track diversity |

---

## Content Archive

_Links to created content (updated as content is produced):_

| Content | File | Platform | Date | Status |
|---------|------|----------|------|--------|
| Reddit launch posts (5 subs) | `.claude/marketing/reddit-launch-posts.md` | Reddit | 2026-03-19 | Ready to post |
| Game aggregator guide | `.claude/marketing/game-aggregators.md` | itch.io, Newgrounds, etc. | 2026-03-19 | Ready to submit |

---

*Last updated: 2026-03-19*
