# Changelog

All notable changes to the `pm` plugin are recorded here. Bump `version` in
`plugins/pm/.claude-plugin/plugin.json` and `.claude-plugin/marketplace.json`
whenever a changed build is distributed, otherwise existing installs will not refresh.

## 0.3.2 (unreleased)

- GitHub issues rung falls back to HTML search pages when the shared-IP API limit is hit.

## 0.3.1

- Fixes from a cold-read review and a simulated participant run: drafts live in
  `.pm/drafts/` and are promoted on approval; clone reuse and case-switch handling; fetch
  budget arithmetic for competitor comparisons; coverage-based stop rule; schema and
  ownership corrections; plain-language definitions for repository, pinned commit, sources
  clean, and rung; check-ins capped at about 300 words and the PRD scope check split in two;
  record only observed participant actions; quote answers verbatim.

## 0.3.0

- Quote bank: verbatim user quotes (`QUO-###`) captured during the research pass from
  fetched text only, stored in the discovery file, shown at the priority and flow
  check-ins, and carried into the PRD as a PM-chosen "Voice of the customer" block. The
  reviewer checks quote provenance.

## 0.2.0

- Discovery source ladder: per-case `feeds:` block with verified keyless endpoints (Discourse
  search JSON, GitHub discussions and reaction-ranked issues, Lemmy, Mastodon tags, Hacker
  News, Apple App Store review feeds, adoption counts, Wayback competitor snapshots).
- Triangulation table in the discovery file and research check-in.
- Readiness tests web search separately from fetch and records a Reddit RSS facilitator switch.
- Usage line at the end of every reply: stage, step, budget used, elapsed time, source status.

## 0.1.0

- Initial workshop kit: `/pm:scout`, `/pm:prototype`, `/pm:prd`, `/pm:create-skill`.
- Read-only `pm:prd-reviewer` agent.
- Five case packages: Immich, Plane, Home Assistant, Umbraco, ERPNext.
