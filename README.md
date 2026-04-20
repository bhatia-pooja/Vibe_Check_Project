# 🌶 Vibe Check

**Voice-first local discovery for the Bay Area.**  
Ask Pepper where to go. She reads Reddit, Google, and your mood — then tells you in 30 seconds.

> Built end-to-end: discovery research → problem scoping → JTBD → architecture → 5 build iterations → beta deployment

---

## Table of Contents

1. [Research: The Discovery Problem](#1-research-the-discovery-problem)
2. [Market Signals](#2-market-signals)
3. [The Core Insight](#3-the-core-insight)
4. [Product Frameworks](#4-product-frameworks)
5. [How It Works](#5-how-it-works)
6. [Key Build Decisions](#6-key-build-decisions)
7. [How the Product Evolved](#7-how-the-product-evolved)
8. [Tech Stack](#8-tech-stack)

---

## 1. Research: The Discovery Problem

Local discovery is broken in a specific way: **too much information, not enough opinion you can trust.**

A person in Palo Alto looking for a coffee shop with wifi has to bounce between:

| Platform | What it gives you | What's broken |
|---|---|---|
| Google Maps | Ratings, hours, AI summaries | SEO-gamed, sterile, no personality |
| Yelp | Long-form reviews | Trust crisis, ad-driven, filtered reviews |
| Instagram | Visual discovery | Not searchable by intent, passive |
| TikTok | Creator recommendations | Algorithmically random, not local |
| Reddit | Authentic community opinions | Unstructured, slow to parse, not real-time |

None of these answer the actual question: **"What's the vibe?"**

The closest analog is a friend who actually lives there. That's what Vibe Check is.

---

## 2. Market Signals

### 2.1 The Closest Comp: DoorDash Zesty (shut down April 17, 2026)

DoorDash launched Zesty in December 2025 — an AI-powered restaurant discovery app in SF and NYC built on conversational prompts like *"a low-key dinner in Williamsburg that's actually good for introverts."*

**What Zesty got right:**
- Intent-based discovery (mood, occasion, context — not just cuisine type)
- Positioned as a "complementary discovery layer" to Yelp/Google
- Reasoning through the request rather than listing results

**Why Zesty failed:**
- Convincing users to download *another* standalone app when habits are already fragmented
- No differentiated modality — text chat feels identical to asking ChatGPT the same question
- No voice, no personality, no trusted source transparency

**Takeaway:** The concept is validated. The execution gap is modality and personality. Zesty felt like a utility. Vibe Check is a friend.

---

### 2.2 The Review Trust Crisis

| Signal | Data |
|---|---|
| Consumers reading reviews before purchase | 98% |
| Customers avoiding a business due to negative reviews | 90% |
| Users identifying errors in Google AI Overview outputs | 75% (NP Digital, 1,000 US respondents) |
| Google AI Overviews citing Reddit as top source | 21% |
| Perplexity citing Reddit | 46.7% |

**Core insight:** Star ratings have become a *disqualification tool*, not a discovery tool. People use them to eliminate options (below 4.0 = skip), not to find great ones. The actual discovery happens through vibes — Reddit threads, friend texts, TikTok videos.

---

### 2.3 Reddit as the New Honest Discovery Layer

People are appending `reddit` to Google searches ("best tacos SF reddit") because they trust community-sourced opinions over platform-curated results.

**Reddit's strengths:** Unfiltered, opinionated, contextual ("the vibe is..." / "go on a weekday, weekends are hell" / "order the X, skip the Y")

**Reddit's weakness:** Unstructured, hard to parse, slow, not location-aware in real-time

→ This is the gap Vibe Check fills.

---

### 2.4 The Voice Opportunity

Every competitor returns text. But the use case is inherently conversational.

When you ask a friend "where should I grab coffee?" they don't hand you a bulleted list. They tell you.

ElevenLabs is at $330M ARR with an $11B valuation as of February 2026. Their v3 model supports emotional expressiveness and audio tags for tone control. A voice that says *"honestly? the coffee is mid but the patio is worth it on a sunny day"* carries more credibility than a 4.2-star rating.

---

## 3. The Core Insight

```
The problem isn't finding places.
It's making the decision.
```

Google Maps gives you 20 results. A trusted friend gives you one answer.

**Problem decomposition:**

| Layer | Symptom | Root Cause |
|---|---|---|
| Discovery | Google returns 20 identical-looking results for "ramen near me" | Ranking optimizes for SEO and paid placement, not vibe fit |
| Signal quality | 4.3★ from 847 reviews tells you nothing about whether it's quiet on a Tuesday | Aggregate ratings flatten nuance; Reddit text is buried and unsynthesized |
| Context blindness | No product adapts to "rainy day + solo + need wifi + walking distance" | Search engines treat queries as keywords, not expressed needs |
| Format mismatch | Users want a decision; apps return a list | Mobile UI copied desktop web metaphors — infinite scroll ≠ decision support |
| Trust gap | "AI recommended it" feels hollow | Black-box recommendations have no cited sources |

---

## 4. Product Frameworks

### 4.1 Jobs To Be Done

The JTBD framework asks: what is the user *hiring* this product to do? Not "find a restaurant" — that's a feature request.

| Job | Trigger (When...) | Desired Outcome | Anxiety to Remove |
|---|---|---|---|
| Make a decision fast | I'm hungry now, friends are waiting, I have 2 mins | Single confident pick, committed tone, no hedging | "What if I pick wrong and everyone's annoyed?" |
| Discover something non-obvious | I'm bored of my usual spots | A place I wouldn't have Googled — sourced from community | "It'll just recommend the same tourist traps" |
| Validate a specific place | Someone mentioned a spot; I want a second opinion | Reddit + Google synthesis on that exact place | "The reviews are mixed — I can't tell what's real" |
| Match the mood/occasion | It's raining / it's a date night / I'm hungover | Recommendation that explicitly acknowledges my context | "No product gets what I actually mean" |
| Work with constraints | Need wifi + outlets + quiet + walking distance | Pepper confirms or flags each constraint | "I'll get there and the wifi will be terrible" |
| Feel heard, not processed | Tired of typing into a search box | Response tone is warm, funny, personal | "AI recommendations feel generic and cold" |

---

### 4.2 Competitive Landscape

| Product | What it does | Modality | Gap |
|---|---|---|---|
| Google Maps | Ratings, hours, AI summaries | Text + map | Sterile, SEO-gamed, no personality |
| Yelp | Long-form reviews, AI assistant | Text | Trust crisis, ad-driven, filtered |
| DoorDash Zesty | Conversational AI discovery | Text chat | No voice, no personality, shut down |
| ChatGPT / Claude | Can answer "where to eat" | Text chat | No local data pipeline, no voice output |
| Google AI Mode | Conversational search | Text | 9% error rate, cites Reddit/Facebook without sourcing |
| TikTok / Instagram | Visual discovery via creators | Video | Not searchable by intent, algorithmic, passive |
| Reddit | Authentic community opinions | Text threads | Unstructured, slow to parse, not real-time |
| **Vibe Check** | **Synthesized, spoken opinion** | **Voice + text** | MVP scope, Bay Area only |

---

### 4.3 Pepper's Decision Algorithm

Pepper is not a search ranker. She is a system-prompted character that reads signals in a specific order and commits to exactly one place.

**Input signals Pepper reads:**
1. Raw query text
2. Mood / context cues (rainy, hungover, date night, celebrating...)
3. Hard constraints (distance, wifi, dietary, budget...)
4. Google reviews (up to 3 per candidate)
5. Reddit thread snippets (up to 10)
6. Candidate address + Google rating

**How Pepper reads the room:**

| User says... | Pepper hears... | Behavior change |
|---|---|---|
| "rainy day" / "cold outside" | Cozy, warm, comfort | Lean into warm spots; open with "Rainy day ramen? Say no more." |
| "date night" / "romantic" | Ambiance, lighting, romance | Prioritize atmosphere over food quality; tone slightly elevated |
| "hungover" / "need caffeine" | Comfort, speed, sympathy | Be funny and sympathetic; recommend comfort food and strong coffee |
| "working remotely" / "need wifi" | Practical: wifi, noise, outlets | Constraint extracted; Pepper must confirm or flag wifi explicitly |
| "I'm celebrating!" | High energy, match the mood | Mirror excitement; pick somewhere special |
| "I don't know what I want" | Indecision = decision needed | Be more decisive than usual; commit hard; don't offer alternatives |

**Hard rules in Pepper's system prompt:**
- Single place: ≤ 55 words. Comparison: ≤ 80 words. Count before writing, delete whole sentences if over.
- First sentence must name the neighborhood / city. Never recommend a place in the wrong city.
- If review data is thin (< 3 reviews), acknowledge it — never invent menu items or atmosphere details.
- Call out specific items ("order the garlic naan"). Mention practical details ("go before 11am weekdays").
- Never hedge. Pick ONE place and commit.

---

## 5. How It Works

```
User types free-form query
        ↓
Constraint extractor (25+ signals: distance, wifi, dietary, hours, budget, group...)
        ↓
isDiscovery() router
  ├─ Discovery flow (no named place) → 3 parallel Brave Search queries targeting Reddit
  │       → extract top 6 place names (regex + business signal scoring)
  │       → Google Places enrichment for each (parallel, up to 6 calls)
  └─ Specific flow (named place) → Google Places lookup → Brave/Reddit for validation
        ↓
synthesizeWithPepper()
  → Claude claude-sonnet-4-6 with Pepper system prompt
  → ≤ 55 word script
        ↓
ElevenLabs TTS → audio file
        ↓
Frontend: voice note + place card + Reddit source pills + Maps deep link
```

**Fallback:** If Reddit/Brave yields 0 named places → Google text search on intent.

---

## 6. Key Build Decisions

| Decision | Rejected | Chosen | Why |
|---|---|---|---|
| Location context | GPS auto-detect | City picker pills | GPS prompt is alarming before user has seen any value. City picker builds trust gradually. Also: GPS coordinates don't help with subreddit targeting — city-level anchor does. |
| Reddit access | Reddit direct API | Brave Search proxy | Railway shared datacenter IPs are permanently flagged by Reddit's rate limiter. All direct API calls returned 0 results in production even though they worked locally. |
| Candidate sourcing | Google top-1 result | Reddit pool → Pepper picks | Single top-Google-result is what Maps already does. Reddit candidates come from actual community recommendations. Pepper comparing 6 community-endorsed places is the core differentiator. |
| Output format | 200+ word AI review | ≤55 word voice note | Long AI output reads like a press release, breaks the "friend" persona. Hard cap forces whole-sentence deletion — produces punchier, more trustworthy output. |
| Quota storage | In-memory only | JSON file persistence | Pure in-memory resets on every Railway redeploy — testers could bypass limits by triggering a redeploy. JSON file persists. |
| IP storage | Raw IP | SHA-256 hashed IP | Storing raw IPs is a GDPR/CCPA liability. SHA-256 with static salt is irreversible without the salt, still allows consistent per-user counting. |
| Subreddit targeting | `site:reddit.com/r/AskSF` | `site:reddit.com r/AskSF` keyword | Brave Search doesn't support subpath site: operators. Keyword approach works because subreddit names appear in thread text and URLs. |

---

## 7. How the Product Evolved

**Phase 1 — Single Google result + Pepper vibe check**  
Simple, fast. No differentiation from "ask ChatGPT about [place]." GPS as first interaction was trust-destroying. No location awareness.

**Phase 2 — Multi-candidate pool, Pepper picks best match**  
Better — Pepper had agency. But Google's top 3 are still Google's top 3. All popular, all obvious. No community signal. City picker replaced GPS.

**Phase 3 — Reddit as discovery SOURCE (not just validation)**  
Core insight: Reddit knows the quiet ramen spot locals love that Google ranks #12. Flipped the pipeline. Discovered Railway IPs blocked by Reddit → pivoted to Brave Search. Cap raised from 3 to 6 candidate names after thin pools in testing.

**Phase 4 — Constraint extraction system**  
Users asking for "wifi + quiet + walking distance" got recs that ignored all three. Built full constraint service (6 dimensions, 25+ signals). Constraints tighten Google Places search radius AND become a Pepper checklist she must address explicitly.

**Phase 5 — Trust + provenance for beta launch**  
Added Reddit source pills, Google Maps deep links, loading messages showing which subreddits are being read. Removed follow-up suggestion pills — Pepper wasn't reliably good at generating them.

---

## 8. Tech Stack

| Layer | Tech | Why |
|---|---|---|
| Frontend | React + Vite → Vercel | Zero-config builds, instant deploys, free tier |
| Backend | Node.js + Express → Railway | Minimal boilerplate, auto-deploys from GitHub |
| AI / LLM | Claude claude-sonnet-4-6 | Best instruction-following + persona maintenance; prompt caching reduces cost |
| Place data | Google Places API | Canonical place data, photos, reviews, Maps deep links |
| Reddit data | Brave Search API ($5/1k req) | Reddit direct API blocked on Railway IPs; Brave proxies Reddit via web search |
| Voice | ElevenLabs Creator plan | Most natural voice quality for conversational text; flat $11/mo covers beta volume |
| Rate limiting | Custom JSON file + SHA-256 hash | No database needed for 100-user beta; persists across Railway redeploys |

**Beta cost model (100 testers × 2 queries):**

| Service | Est. cost |
|---|---|
| Google Places API | ~$11.00 |
| Anthropic Claude | ~$0.60 |
| Brave Search | ~$0.90 |
| ElevenLabs TTS | $11.00/mo flat |
| Railway + Vercel | $0 (free tiers) |
| **Total** | **~$23** |

---

*Built April 2026 · Bay Area beta · 100 tester cap · 2 queries per user*

---

<sub>Pooja Bhatia · [Portfolio](https://poojabhatia.vercel.app) · [LinkedIn](https://linkedin.com/in/bhatiapooja43)</sub>
