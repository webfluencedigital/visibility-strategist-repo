---
name: visibility-audit
description: This skill should be used when the user wants to audit a target site's visibility across both traditional search rankings AND AI recommendation engines. Triggers on phrases like "audit visibility", "/visibility-audit [url]", "rank this site", "what should I add to my site for SEO", "check our AI citations", "audit our AEO", "audit visibility for [client]", "run visibility on [url]". Dispatches both seo-strategist and aeo-strategist in parallel and writes structured output to the Visibility Strategist Airtable base.
metadata:
  version: "0.1.0"
---

# Visibility Audit

Run a full visibility audit on a target site — traditional search rankings (SEO) plus AI recommendation engine citations (AEO) — and produce a structured plan in the Visibility Strategist Airtable base, plus a WebFluence-branded PDF deliverable.

## When to use

Trigger phrases:

- "audit visibility for [url]"
- "/visibility-audit [url]"
- "rank this site: [url]"
- "what should I add to my site for SEO"
- "check our AI citations"
- "audit our AEO"
- "run visibility on [client]"
- "is [client] visible to ChatGPT?"
- "where do we rank for [keyword]"

## What this skill does

This skill orchestrates the two paired Visibility Strategist agents:
1. Reads or creates a **Visibility Plan** record in the base (`appqiOiuFJBp0UNbF`).
2. Dispatches **seo-strategist** and **aeo-strategist** in parallel with the target URL and Plan record ID.
3. Each agent writes its output to its assigned tabs (Keyword Targets, Technical Audit, Question Bank) and contributes jointly to Sitemap Recommendations and Content Calendar.
4. Once both agents return, renders the WebFluence-branded `audit-template.html` from the Plan + supporting tab records and produces a PDF for the user.

## How to execute this skill

### Step 1 — Identify the target

The user provides:
- A URL (e.g. `https://shelbournedentalclinic.ie`)
- Optionally a client name and niche (Dental, Aesthetics, Gym, Restaurants, Trades, Property, Professional services)
- Optionally an existing Visibility Plan record ID to resume

If no Plan record ID, create one in the Visibility Plan tab:
- `Plan Name`: `[Client/Domain] — [Quarter] [Year] Visibility Audit`
- `Target Site`: the URL
- `Phase`: `Discovery`
- `Status`: `Active`
- `Owner Agent`: `Both`
- `Start Date`: today
- `Next Review`: today + 14 days
- `Summary`: one-sentence scope
- `KPI Targets`: empty for now — agents will populate after baseline pull

### Step 2 — Verify required connectors

Before dispatching agents, verify these are connected at the user level:
- Airtable (mandatory — output destination)
- Supermetrics (recommended — gives GSC ground truth)
- Ahrefs (recommended — keyword volumes, site audits)
- Apify (mandatory — SERP, Reddit, keyword suggestions)

If any recommended connector is missing, note it in the Plan record's Notes field but proceed. The agents have fallbacks but produce stronger output with all connectors live.

### Step 3 — Dispatch agents in parallel

Use the Task tool with two parallel calls in a single message:

```
Task 1: subagent_type "seo-strategist", prompt:
  "Run the rankings half of the visibility audit on [URL].
   Visibility Plan record ID: [recXXX]
   Niche: [Dental / Gym / etc.]
   Pull baseline KPIs, run cannibalization gate, write Keyword Targets,
   Technical Audit, contribute to Sitemap Recommendations and Content Calendar.
   Seed Ranking Tracker with Critical + High keyword baselines.
   Return when handoff protocol satisfied."

Task 2: subagent_type "aeo-strategist", prompt:
  "Run the AI citation half of the visibility audit on [URL].
   Visibility Plan record ID: [recXXX]
   Niche: [Dental / Gym / etc.]
   Generate 30+ test prompts, query ChatGPT/Claude/Gemini/Perplexity,
   identify lost prompts, write Question Bank records, write Citation Audit
   Scorecard to Plan notes, generate Fix Pack.
   Coordinate with seo-strategist on Sitemap Recommendations.
   Return when handoff protocol satisfied."
```

Run both concurrently. Agents communicate through the shared Plan record and Sitemap Recommendations tab — neither blocks the other.

### Step 4 — Joint coordination on Sitemap Recommendations

After both agents return, verify the Sitemap Recommendations tab:
- Every Critical/High keyword has a target page
- Every Critical AEO question has a target page
- Page Type distribution makes sense (mix of Pillar, Cluster, Service, FAQ, Comparison)
- No two records share the same Primary Keyword (cannibalization gate downstream)

If conflicts exist, dispatch one of the agents (typically seo-strategist) to resolve.

### Step 5 — Render the branded PDF deliverable

Render `deliverables/audit-template.html` (in this plugin folder) with the Plan + tab records as variables. Convert to PDF via wkhtmltopdf or chromium-headless. Save to `Dev-Projects/visibility-deliverables/[client-slug]-visibility-audit-[YYYY-MM-DD].pdf`.

If `wkhtmltopdf` isn't available in the bash sandbox, fall back to chromium-headless: `chromium-browser --headless --disable-gpu --print-to-pdf=output.pdf input.html`.

### Step 6 — Report back

When done, summarise to the user:
- Visibility Plan record ID + URL
- Counts: keywords targeted, technical issues found, questions banked, sitemap pages proposed, content posts briefed
- Citation Audit scorecard summary (overall citation rate, gap to top competitor)
- Top 3 P1 fixes (cross-cutting from both agents)
- Path to the rendered PDF deliverable
- Next Review date (14 days out)

## Common scenarios

**Scenario A — Cold audit on existing site**
User points at a domain. Skill creates Plan, dispatches both agents, produces baseline + first-round fix pack. The most common case.

**Scenario B — Pre-build audit (visibility upstream of build squad)**
User is about to start a new client website build via webfluence-squad. Visibility audit runs first to determine what pages should exist. Sitemap Recommendations + Keyword Targets feed into the build squad's Phase 2 (visual-storyteller) and Phase 3 (ux-architect). Skill should mark the Plan record's Notes with `Pre-build engagement — feed Sitemap Recommendations to webfluence-squad senior-pm`.

**Scenario C — 14-day recheck**
User wants to verify a previous fix pack moved the citation rate. Skill skips Step 1's Plan creation, finds the existing Plan record, dispatches aeo-strategist with `mode: recheck` to re-run the original prompt set. Compares against baseline. Updates Plan with delta.

**Scenario D — Monthly retainer report**
User is generating the monthly client report. Skill pulls the latest Plan + Ranking Tracker + Technical Audit (Resolved this month) + Content Calendar (Published this month) and renders the same audit-template.html with month-over-month deltas highlighted. Saves as `[client-slug]-visibility-monthly-[YYYY-MM].pdf`.

## Reference

- seo-strategist agent: `agents/seo-strategist.md`
- aeo-strategist agent: `agents/aeo-strategist.md`
- Branded deliverable template: `deliverables/audit-template.html`
- Airtable schema reference: `Dev-Projects/squad-prompts/visibility-base-schema.md`
- Visibility Strategist base ID: `appqiOiuFJBp0UNbF`
