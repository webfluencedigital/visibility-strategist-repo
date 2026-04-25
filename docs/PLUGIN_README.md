# Visibility Strategist

The retainer half of the WebFluence agency. Two paired specialist agents that take a target site (and optionally a target client niche) and produce an ongoing visibility programme — keyword strategy, AEO question coverage, schema, technical audit, content calendar, and monthly ranking tracking.

Pairs with [`webfluence-squad`](https://github.com/dusanwalla/webfluence-squad) when a build is happening alongside. Runs solo on existing sites for retainer-only engagements.

Built for WebFluence Digital. Reusable on any new client retainer.

---

## What's in the box

| Component | Count | Purpose |
|---|---|---|
| Skills | 1 | `visibility-audit` |
| Agents | 2 | `seo-strategist`, `aeo-strategist` |
| MCP servers | 0 | Uses existing connectors at the user level |
| Deliverable templates | 1 | WebFluence-branded `audit-template.html` (renderable to PDF) |

---

## The squad

| Agent | Role | Owns Airtable tabs | Color |
|---|---|---|---|
| `seo-strategist` | Traditional search rankings. Keyword research, topical clusters, technical audits, cannibalization checks, link strategy. Wave 1 of AI-driven acquisition. | Keyword Targets · Technical Audit · (joint) Sitemap Recommendations · Content Calendar · Ranking Tracker | cyan |
| `aeo-strategist` | AI citation visibility. Multi-platform audits (ChatGPT, Claude, Gemini, Perplexity), question mining, FAQ/HowTo schema, entity optimisation, AI Overview targeting. Wave 2 + standing knowledge of Wave 3 (WebMCP / agentic task completion). | Question Bank · (joint) Sitemap Recommendations · Content Calendar | yellow |

Both agents write into the **Visibility Strategist** Airtable base (`appqiOiuFJBp0UNbF`) in their assigned tabs. The master **Visibility Plan** tab is jointly owned.

---

## Setup

The plugin uses your existing **Airtable**, **Ahrefs**, **Apify**, **Supermetrics**, and **Gmail** connectors. No environment variables, no credentials inside the plugin. If any of these aren't already connected at the Cowork user level, connect them via Settings → Integrations.

**Tools the agents lean on:**

- **Supermetrics** — primary data source for Google Search Console + Google Analytics. Gives the squad authoritative ground truth on what's already ranking and what's getting impressions but not clicks.
- **Ahrefs** — keyword volume, difficulty, backlink profile, scheduled site audits.
- **Apify** — `apify/google-trends-scraper`, `sovereigntaylor/google-search-scraper` (for SERP + People Also Ask), `trudax/reddit-scraper-lite` (for AEO question mining), `habit.zhou/keyword-suggest-multi` (AnswerThePublic equivalent).
- **WebFetch + WebSearch** — Wikidata, Google Knowledge Graph, schema validators, AI Overview spot checks.
- **Bash** — Lighthouse runs (`npx lighthouse [url]`).
- **Airtable** — output destination for all records.

---

## How to fire the squad

When a new retainer client comes in, or when running a visibility audit on an existing site:

```
/visibility-audit [url]
```

That's it. The skill dispatches both agents in parallel:
- **seo-strategist** runs the technical + keyword + cannibalization audit, produces records in Keyword Targets and Technical Audit, and contributes to Sitemap Recommendations.
- **aeo-strategist** runs the multi-platform AI citation audit, mines questions from real platforms, produces records in Question Bank, and contributes to Sitemap Recommendations.
- Both write to a shared **Visibility Plan** record that summarises the engagement.

The deliverable PDF (WebFluence-branded) renders from the latest plan + supporting tab records.

---

## Pipeline order

```
   Target site → /visibility-audit ──┬─→ seo-strategist  ─┐
                                      └─→ aeo-strategist ─┤
                                                           ↓
                                              Joint Sitemap Recommendations
                                                           ↓
                                              Branded PDF deliverable
                                                           ↓
                                              (Optional) Hand to senior-pm
                                              for build-squad pre-deploy input
```

**Phase 1 (parallel)** — both agents run off the target URL simultaneously.

**Phase 2** — agents converge on Sitemap Recommendations (joint output: which pages should exist, with primary keyword + AEO question coverage + schema spec).

**Phase 3 (post-deploy, ongoing)** — Ranking Tracker tab updates weekly. Monthly retainer report generated from the same template.

---

## Integration with webfluence-squad (the build squad)

When a client buys both a build AND a retainer, the visibility squad runs **upstream** of the build squad's Phase 2 (storyboard) and Phase 3 (architect). Specifically:

- Sitemap Recommendations → fed to `visual-storyteller` as the page list to scene
- Primary Keyword + Schema Markup per page → fed to `ux-architect` as the URL slug + JSON-LD spec
- Top AEO questions → fed to `visual-storyteller` as required FAQ scene content

Senior PM in webfluence-squad gates this consultation — it only happens if the brief includes ongoing rankings as scope.

---

## Voice and rules — extracted, not invented

The agents pattern-match the existing webfluence-squad agents in tone (direct, confident, no-jargon, peer-level) and adapt established patterns from the agency-agents repo (`marketing-seo-specialist`, `marketing-ai-citation-strategist`, `marketing-agentic-search-optimizer`) into WebFluence-specific structure. The Dublin SME niche knowledge is baked in.

Standing rules:
- **White-hat only.** No link schemes, cloaking, keyword stuffing.
- **Cannibalization audit is mandatory** before any keyword reassignment.
- **Multi-platform AEO** — ChatGPT, Claude, Gemini, and Perplexity, never just one.
- **Quantified targets only.** "Top 3 by July" not "improve rankings."
- **WebMCP awareness** — the agents know about Wave 3 (agentic task completion) and flag opportunities, even though full WebMCP implementation is an upsell beyond the squad's scope.

---

## Versioning

`v0.1.0` — April 2026. Initial release. Battle-tested on WebFluence Digital's own site (`webfluence.digital` baseline + `webfluence-digital.vercel.app` forward planning).
