# Contributing

Thanks for considering a contribution. This is a young project (v0.1.0) — feedback, niche packs, and bug reports are all welcome.

## Quick paths

| You want to... | Open... |
|---|---|
| Report a bug | [Bug report issue](.github/ISSUE_TEMPLATE/bug_report.yml) |
| Request a feature | [Feature request issue](.github/ISSUE_TEMPLATE/feature_request.yml) |
| Add a niche pack (your industry's keyword + AEO patterns) | [Niche pack issue](.github/ISSUE_TEMPLATE/niche_pack.yml), then PR |
| Fix a typo / improve docs | PR directly, no issue needed |
| Suggest a connector integration | Discussion or feature request |

## Development setup

The plugin is a Cowork / Claude Code plugin. Local development:

1. Clone this repo.
2. Install via Cowork plugin manager pointing at the local folder, OR copy `visibility-strategist/` into your Cowork plugin directory.
3. Connect the upstream MCPs you'll be using (Airtable required; Supermetrics, Ahrefs, Apify recommended). See `docs/INSTALL.md`.
4. Test the skill: `/visibility-audit https://example.com` against a low-stakes site.

The plugin has no build step. Edits to `agents/*.md`, `skills/visibility-audit/SKILL.md`, and `deliverables/audit-template.html` take effect on the next plugin reload in Cowork.

## Niche packs (the highest-value contribution right now)

The agents currently ship with Dublin SME niche knowledge (dental, aesthetics, gym, restaurants, trades, property, professional services). Adding a niche pack means:

1. Fork the repo.
2. In `agents/seo-strategist.md`, find the **Standing knowledge — Dublin SME niches** section. Copy the table format and add your niche row(s). Patterns include: pillar keyword pattern, long-tail patterns, local pack signals.
3. In `agents/aeo-strategist.md`, find the **Standing knowledge — Dublin SME AEO patterns** section. Copy the table format and add your niche row(s). Patterns include: highest-leverage AEO content type, schema priority.
4. PR with a short note explaining the niche, your geography, and any sources you leaned on.

Goal: a `niches/` folder of pluggable per-niche knowledge that any installer can pull into their own fork. We're not there yet — niche knowledge currently lives inline in the agent files. PRs that propose a cleaner mechanism for this are welcome.

## Code style

- Markdown: keep tables aligned, prefer pipe tables over HTML, use `---` for hard separators.
- YAML frontmatter: keep `<example>` blocks **outside** the frontmatter (in the body, after the closing `---`). YAML parsers choke on them otherwise. See `agents/senior-pm.md` in `webfluence-squad` for the working pattern.
- Voice: direct, source-cited, no jargon. Avoid "synergy", "leverage", "ecosystem", "unlock" anywhere in agent or skill files. Plain English, peer-level.
- Numbers over adjectives. "GSC shows 1,247 impressions" beats "the page gets some impressions."

## Bigger changes — discuss first

Before opening a PR for changes that affect:
- The two-agent shape (e.g. proposing a third agent)
- The Airtable schema (adding/removing tabs or fields)
- The cannibalization gate logic
- The deliverable rendering pipeline

...please open a Discussion or Issue first. These are architectural decisions and the project's still small enough that one wrong turn matters.

## Code of conduct

Be respectful, be constructive, assume good faith. If you wouldn't say it to a colleague over coffee, don't say it here.

## Maintainer

[Dusan Walla](mailto:dusan.walla@webfluence.digital) — solo maintainer for now. Response times: usually within 48 hours, sometimes longer when shipping client work.
