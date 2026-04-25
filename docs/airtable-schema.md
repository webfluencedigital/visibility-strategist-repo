# Visibility Strategist — Airtable Base Schema

Reference for the Visibility Strategist base structure. Use this when cloning the base for a new client retainer or extending the existing one.

**Source base**: `Visibility Strategist` (`appqiOiuFJBp0UNbF`)
**Tabs**: 7 — one master plan tab plus six specialist output tabs.
**Cadence**: Plan tab is monthly. Keyword Targets, Question Bank, Sitemap Recommendations, Technical Audit, Content Calendar are quarterly with weekly updates. Ranking Tracker is a living tab — updated weekly post-deploy.

---

## Tabs and primary fields

### 1. Visibility Plan (master / dashboard)
**Owners**: Senior PM + SEO Strategist + AEO Strategist
**Description**: Master plan per target site or per quarter. One record = one engagement period. Drives the cadence for the other tabs.

| Field | Type | Notes |
|---|---|---|
| Plan Name | singleLineText | Primary key. e.g. "WebFluence Digital — Q2 2026 Visibility Plan" |
| Target Site | url | Domain being optimised |
| Phase | singleSelect | `Discovery`, `Audit`, `Strategy`, `Execution`, `Measurement`, `Recheck` |
| Status | singleSelect | `Planning`, `Active`, `Paused`, `Completed`, `Archived` |
| Owner Agent | singleSelect | `SEO Strategist`, `AEO Strategist`, `Both`, `Senior PM` |
| Start Date | date | |
| Next Review | date | 14-day or monthly cadence |
| Summary | multilineText | What the plan covers — scope, target audience, channels |
| KPI Targets | multilineText | Quantified goals — e.g. "Top 3 for 'web design dublin' by July 2026; 30%+ AI citation rate by Q3" |
| Notes | multilineText | |

### 2. Keyword Targets (SEO Strategist)
**Description**: Every keyword the client should rank for. Source includes GSC (already getting impressions), Ahrefs (volume + difficulty), Apify SERP (live position checks), and competitive gap analysis.

| Field | Type | Notes |
|---|---|---|
| Keyword | singleLineText | Primary key. e.g. "web design dublin" |
| Target Page | url | Which page should own this keyword (one keyword = one owner page) |
| Search Volume | number (0dp) | Monthly searches in target geography |
| Keyword Difficulty | number (0dp) | 0–100 scale (Ahrefs KD or equivalent) |
| Search Intent | singleSelect | `Informational`, `Commercial`, `Transactional`, `Navigational`, `Local` |
| Current Position | number (0dp) | Latest known rank — empty if not ranking |
| Target Position | number (0dp) | Realistic goal — typically 1–3 for primary, 1–10 for secondary |
| Priority | singleSelect | `Critical`, `High`, `Medium`, `Low` |
| Cluster | singleLineText | Topic cluster name — groups pillar + satellites |
| Source | singleSelect | `GSC`, `Ahrefs`, `Apify SERP`, `Manual`, `Competitor Gap` |
| SERP Features | multipleSelects | `Featured Snippet`, `People Also Ask`, `Knowledge Panel`, `Local Pack`, `Video Carousel`, `Image Pack`, `AI Overview` |
| Notes | multilineText | |

### 3. Question Bank (AEO Strategist)
**Description**: Real questions real people type into AI assistants and search engines. The substrate for FAQ schema, AI Overview targeting, and AEO content.

| Field | Type | Notes |
|---|---|---|
| Question | singleLineText | Primary key. Actual phrasing — e.g. "what does a website cost in dublin" |
| Source | singleSelect | `AnswerThePublic`, `Reddit`, `GSC`, `PAA`, `AI Overview`, `Forum`, `Manual` |
| Intent Type | singleSelect | `Best-of`, `Comparison`, `How-to`, `What-is`, `Recommendation`, `Review`, `Pricing` |
| Target Page | url | Page that should answer this question |
| Answer Draft | multilineText | The 2–4 sentence AEO-ready answer. Direct, factual, citation-worthy |
| Schema Type | singleSelect | `FAQPage`, `HowTo`, `Product`, `Article`, `LocalBusiness`, `Organization`, `Service` |
| AI Citation Status | multipleSelects | `ChatGPT cited`, `Claude cited`, `Gemini cited`, `Perplexity cited`, `Google AI Overview`, `Not cited` |
| Priority | singleSelect | `Critical`, `High`, `Medium`, `Low` |
| Notes | multilineText | |

### 4. Sitemap Recommendations
**Owners**: SEO Strategist + AEO Strategist (joint)
**Description**: Pages that should exist. Drives the build squad's Phase 2 (storyboard) and Phase 3 (architect) when this squad runs upstream of a build.

| Field | Type | Notes |
|---|---|---|
| Page Slug | singleLineText | Primary key. e.g. "/services/seo-dublin" |
| Page Type | singleSelect | `Pillar`, `Cluster`, `Service`, `Industry`, `Location`, `Blog`, `Resource`, `Comparison`, `FAQ` |
| Primary Keyword | singleLineText | The single keyword this page owns |
| Secondary Keywords | multilineText | Supporting keywords + long-tail variants |
| Content Brief | multilineText | What this page must contain — sections, word count target, examples |
| Schema Markup | multilineText | Recommended JSON-LD blocks. Quote the schema |
| Internal Linking | multilineText | What links in (parent pillar, related satellites). What links out (CTA destinations) |
| Status | singleSelect | `Proposed`, `Approved`, `In Progress`, `Live`, `Needs Update` |
| Priority | singleSelect | `Critical`, `High`, `Medium`, `Low` |
| Estimated Impact | multilineText | Forecast — keyword volume captured, AEO surface area, conversion path role |

### 5. Technical Audit
**Owner**: SEO Strategist (primary) — AEO Strategist contributes schema + entity issues
**Description**: Every issue found by Ahrefs, Lighthouse, GSC Coverage, schema validators. Severity-ordered.

| Field | Type | Notes |
|---|---|---|
| Issue | singleLineText | Primary key. Short description. e.g. "Missing canonical tag on /services/web-design" |
| Category | singleSelect | `Crawlability`, `Indexation`, `Core Web Vitals`, `Schema`, `Internal Linking`, `Mobile`, `Security`, `Hreflang`, `Entity`, `Other` |
| Severity | singleSelect | `Critical`, `High`, `Medium`, `Low` |
| Affected URLs | multilineText | One URL per line |
| Source | singleSelect | `Ahrefs`, `Lighthouse`, `GSC Coverage`, `Schema Validator`, `Rich Results Test`, `Manual`, `Supermetrics` |
| Fix Description | multilineText | Concrete fix — code snippet, config change, content edit |
| Status | singleSelect | `Open`, `In Progress`, `Resolved`, `Won't Fix`, `Verified` |
| Detected Date | date | |
| Resolved Date | date | |
| Notes | multilineText | |

### 6. Content Calendar
**Owners**: SEO Strategist + AEO Strategist + writers
**Description**: 90-day rolling calendar. Each post targets either a keyword (SEO) or a question (AEO) or both.

| Field | Type | Notes |
|---|---|---|
| Post Title | singleLineText | Primary key |
| Target Keyword | singleLineText | Primary keyword (links to Keyword Targets tab) |
| Target Question | singleLineText | For AEO posts (links to Question Bank tab) |
| Format | singleSelect | `Blog Post`, `Guide`, `Comparison`, `How-to`, `Case Study`, `Resource Page`, `Glossary`, `FAQ Hub` |
| Cluster | singleLineText | Topic cluster this post belongs to |
| Scheduled Date | date | Publish date |
| Status | singleSelect | `Idea`, `Briefed`, `In Draft`, `In Review`, `Scheduled`, `Published`, `Updated` |
| Owner | singleSelect | `SEO Strategist`, `AEO Strategist`, `Senior PM`, `External Writer` |
| Distribution | multipleSelects | `LinkedIn`, `Twitter/X`, `Email Newsletter`, `Reddit`, `Industry Forum`, `Internal Link Hub` |
| Word Count Target | number (0dp) | Realistic — 800/1500/3000 |
| Brief | multilineText | What the post must say. Include H2 outline, target queries, internal links |
| Published URL | url | |

### 7. Ranking Tracker (post-deploy, living)
**Owner**: SEO Strategist
**Description**: Monthly ranking snapshot per tracked keyword. Source of truth for the monthly retainer report. Populates from Supermetrics GSC + Apify SERP + Ahrefs Rank Tracker.

| Field | Type | Notes |
|---|---|---|
| Keyword | singleLineText | Primary key (matches Keyword Targets) |
| Target URL | url | |
| Baseline Position | number (0dp) | Position when tracking started |
| Current Position | number (0dp) | Latest |
| Position Last Month | number (0dp) | For month-over-month delta |
| Trend | singleSelect | `Up`, `Steady`, `Down`, `New`, `Lost` |
| Last Checked | date | |
| Search Volume | number (0dp) | Monthly |
| Estimated Monthly Traffic | number (0dp) | Position × CTR × volume |
| Notes | multilineText | |

---

## Setup procedure (per new retainer client)

1. **Use Airtable API** to create a new base named `[ClientName] — Visibility`.
2. Create all 7 tables above with the exact schemas. Single-select option lists must match.
3. Seed the **Visibility Plan** tab with one record: `Plan Name = "[Client] — Q[N] [Year]"`, `Phase = Discovery`, `Status = Planning`, target dates set.
4. Seed the **Technical Audit** tab from the latest Ahrefs site audit emails (already arriving in Gmail per CLAUDE.md).
5. Pull GSC data via Supermetrics → seed the **Keyword Targets** tab with anything currently getting impressions but ranking outside top 10.
6. Return the new `baseId` to the user.

---

## Why a per-client base, not a shared one

Same logic as the build squad:
- Clean client data separation — easy to share base if client is technical
- Permanent record — Ranking Tracker is a multi-year asset for the agency
- Cross-client pattern matching — `list_bases` lets the squad pull lessons from previous engagements in the same niche
- Simple billing/retention — when the retainer ends, the base archives intact

---

## Reference

Source base: `appqiOiuFJBp0UNbF` (Visibility Strategist). The first authoritative records will be from the WebFluence Digital battle-test (April 2026).

For the build squad's parallel reference, see: `airtable-template-schema.md` (the WebFluence Development base structure).
