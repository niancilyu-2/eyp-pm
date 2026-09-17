# Changelog

All notable changes to the `pm` plugin are recorded here. Bump `version` in
`plugins/pm/.claude-plugin/plugin.json` and `.claude-plugin/marketplace.json`
whenever a changed build is distributed, otherwise existing installs will not refresh.

## 0.2.0 (unreleased)

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
