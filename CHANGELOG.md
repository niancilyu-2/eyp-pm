# Changelog

All notable changes to the `pm` plugin are recorded here. Bump `version` in
`plugins/pm/.claude-plugin/plugin.json` and `.claude-plugin/marketplace.json`
whenever a changed build is distributed, otherwise existing installs will not refresh.

## 0.5.0 (unreleased)

- Research rigour: skew per rung and a "Who we heard from" paragraph, counting rules with
  bases and one signal per underlying item, recurrence labels, a counter-evidence fetch per
  theme, and constraints that cite commit, path, line range, and a quoted line marked "to
  confirm with engineering". The PRD stage reopens each constraint before labelling
  anything Verified.

## 0.4.2

- Android reviews: `play_store_page` per case with an Android app; the participant pastes
  reviews from the browser and Claude logs them as participant-pasted evidence.

## 0.4.1

- Capstone repositioned as a facilitator demo and take-home exercise; time budget adjusted.

## 0.4.0

- Five more keyless sources per case: forum top and per-tag lists, Hacker News comments,
  App Store lookup with release notes, Wikipedia pageviews, and a roadmap rung combining
  the product's own boards with competitors' job postings. Fetch budget raised to 20.
- Scout skill lists every source key and what it gives.

## 0.3.2

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
