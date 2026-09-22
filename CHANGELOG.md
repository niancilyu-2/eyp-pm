# Changelog

All notable changes to the `pm` plugin are recorded here. Bump `version` in
`plugins/pm/.claude-plugin/plugin.json` and `.claude-plugin/marketplace.json`
whenever a changed build is distributed, otherwise existing installs will not refresh.

## 0.7.7 (unreleased)

- PRD opens with a five-sentence Summary for engineering; header splits status, approval,
  and revision; soft caps of 10 requirements and 15 criteria with one observable outcome
  each; success measures name their mechanism or a dependency; persona is the role, not a
  synthetic name. Claude re-checks any new code claim the reviewer makes before proposing
  a disposition.

## 0.7.6

- PRD engineering appendix opens with a Feasibility read: constraints re-checked at their
  line ranges, touchpoints by label, layers not read, engineering-only questions. The
  reviewer checks it is present.

## 0.7.5

- Prototype walkthrough records one row per checklist area, Claude's check and the PM's
  click-through side by side; error states carry role="alert"; no clickable divs. Design
  brief embeds collapsed with demoted headings. When the PM overrides a recommendation,
  Claude asks once for the reason before recording.

## 0.7.4

- Prototype reads only the design paths the case lists and never guesses URLs; a 404 falls
  back to neutral defaults. Umbraco design notes pin the exact UUI token files.

## 0.7.3

- Quotes are Markdown blockquotes with one attribution line beneath, in the brief, at
  check-ins, and in the PRD. Prose paraphrases; composed sentences are never shown in
  quotation marks.

## 0.7.2

- Discovery brief restructured answer-first: the decision (chosen opportunity, your reason,
  the tipping lens, the others and why they wait) comes first, then what was learned, the
  three opportunities in detail, the evidence, assumptions, and the decision record.
  "Claude's reasoning" is now "Reasoning"; decision records use Recommendation and Your
  decision columns.

## 0.7.1

- Each opportunity carries a one-line evidence summary, and the Now recommendation names
  the lens that tipped it and says when it departs from the densest signal.

## 0.7.0

- Lanes re-cut from an evidence audit of each product (forum, GitHub, reviews, release
  history). Immich: lane 2 boundary tightened. Plane: two lanes reshaped so nothing relies
  on Pro-only features. Home Assistant: lanes renamed around household defaults, recovery
  from updates, and automation troubleshooting. Umbraco: media lane narrowed to the picker,
  validation folded into draft-to-publish, localization stands alone. ERPNext: lanes name
  serial and batch handling and reposting, the navigation lane becomes Stock and Buying
  worklists, and notes corrected for v16 general availability.

## 0.6.2

- Check-ins read as plain-language briefings: story first, three themes in a four-line
  shape, quotes, gaps. Tables and rigour vocabulary stay in the draft file. Orientation
  walks through how the chosen lane works in the product today and defines every product
  term. Priority check-in introduces each opportunity as a two-sentence story.

## 0.6.1

- The plugin no longer refers to a workshop, participants, or a facilitator anywhere. It
  addresses the PM directly, the stop-after-setup prompt says "continue later", the Reddit
  feed is a PM opt-in, and offline repository copies replace facilitator copies. All
  workshop material lives in `docs/FACILITATOR.md`.

## 0.6.0

- Repositioned as a distributable skill package. README is a package front page with a
  flow diagram and a traceability diagram; facilitation material moved to
  `docs/FACILITATOR.md`. Manifest descriptions updated.

## 0.5.1

- README rewritten as a participant-facing front page.
- Start time is written into the stage draft and re-read after a resume, so elapsed minutes
  survive a restart. Replies follow the same no-dash, no-arrow rule as files. Walkthrough
  notes the permission prompt when the capstone writes its skill file.

## 0.5.0

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
