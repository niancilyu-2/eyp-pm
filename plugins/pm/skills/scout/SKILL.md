---
name: scout
description: Discovery stage of the EYP PM workshop. Compare public customer, market, competitor, and repository evidence for one product case and choose one opportunity to prototype. Run first; performs the readiness check on first use.
disable-model-invocation: true
---

# /pm:scout (Discovery)

You are guiding a product manager with strong product judgment and limited technical
experience. Read `${CLAUDE_PLUGIN_ROOT}/resources/shared-rules.md` now and follow it for the
whole stage. This skill takes no arguments.

**Learning objective:** use Claude to compare mixed, imperfect evidence while keeping the
prioritisation judgment with the PM.

**Outcome:** `outputs/01-discovery.md` and one participant-selected opportunity.

## Open the stage

Say, in three short lines: *You will practise* comparing imperfect evidence and turning it
into a reasoned priority. *Claude will* find and organise public evidence, show
contradictions, identify repository constraints, and draft options. *You decide* the
problem framing, evidence quality, the selected opportunity, the roadmap order, and the
uncertainty you accept.

Then state the stage boundary from the shared rules. From here on, end every reply with
the usage line described in shared rules section 9 (during readiness use the `Setup ·
check n/8` form) (`Scout · step n/10 · fetches · searches · items · quotes · minutes · sources`).

## Step 1: Readiness and workspace

Read `.pm/session.yaml` if it exists.

- If it does not exist, or `readiness.status` is `not-run` or `blocked`: run the full
  procedure in `${CLAUDE_PLUGIN_ROOT}/resources/readiness.md`, using
  `${CLAUDE_PLUGIN_ROOT}/resources/workspace.md` for folder and clone steps. At the end
  ask whether to stop after setup or continue. If they stop, save and end here.
- Otherwise print `Readiness: <status> on <date>` and continue.

If `stages.discovery.status` is `in-progress` or `approved`, offer **Resume** or
**Restart** as described in `workspace.md`.

## Step 2: Present the cases

List the five cases from `${CLAUDE_PLUGIN_ROOT}/cases/*/case.yaml` in one table: name,
tier, persona, mandate (one line), and the three lane names. Explain in one sentence that
core cases are the recommended choice and ERPNext is the advanced case with two
repositories. If a case was pre-selected during readiness, say so and offer to keep it.

## Step 3: Choose case and lane, then orient

Ask for the case, then the lane (two questions, not one). Save `case.id`, `case.lane`,
and `case.chosen_at` to `session.yaml`.

Before the orientation, define three words in one line each, because they recur all
day: repository (the product's source code, kept in a folder), pinned commit (a fixed
snapshot of that code from a set date), and sources clean (nothing in that folder has
changed). Then give a short orientation from the case's `orientation.md`: what the product is, who the
persona is, the lane's territory, surface, and viewport, and the two or three repository
folders most relevant to the lane. Keep it under 200 words. Do not offer opinions about
what the problems are.

## Step 4: Acquire the sources

For each repository in `case.yaml` with a `clone_to`: if `<clone_to>` already exists and
`git rev-parse HEAD` inside it equals `pinned_sha`, reuse it. If it exists but holds a
different case (pre-selected during readiness, changed today), rename it to
`sources/_old-<case id>` and clone fresh, as described in `workspace.md`. Otherwise
follow the clone procedure there. Repositories with `role: web-reference` are not cloned.
Record `sources[]` in `session.yaml`. Run the source check (`git status --porcelain`) and
record `clean_at_start`.

Set `stage.current: discovery`, `stage.status: in-progress`,
`stages.discovery.status: in-progress`.

## Step 5: Public research pass (the source ladder)

Follow `${CLAUDE_PLUGIN_ROOT}/resources/research-guide.md` exactly. Record the start time.
Tell the participant once what a rung is: one kind of public source, and the ladder is
the order you read them in. Read the `feeds:` block of the case's `case.yaml`. The
sources it can hold, all public and keyless:

| Key in `feeds:` | What it gives |
|---|---|
| `forum_search_json`, `forum_top_json`, `forum_tag_top_json` | the product forum: search results, this year's top threads, top threads per tag |
| `github_discussions_top` | GitHub Discussions ranked by upvotes |
| `github_issues_top`, `github_issues_top_companion` | GitHub issues ranked by reactions (API); `_html` twins for when the API is rate-limited |
| `lemmy_search`, `lemmy_communities` | Lemmy posts by search and by community, with scores |
| `mastodon_tags` | recent posts under the product's hashtags |
| `hackernews_search`, `hackernews_comments` | Hacker News stories and comments with points |
| `app_store_reviews`, `app_store_lookup` | Apple App Store review text, rating counts, current version and release notes |
| `adoption` | GitHub stars, Docker Hub pulls, PyPI or NuGet downloads |
| `wikipedia_pageviews` | monthly Wikipedia article views, a public-interest trend |
| `roadmap.own` | the product's own project boards, milestones, or roadmap page |
| `roadmap.competitor_jobs` | competitors' live job postings, a roadmap tell |
| `competitors` | competitor pricing or feature pages, today and a year ago via Wayback |
| `reddit_rss_optional` | one subreddit feed, only if the facilitator has switched it on |

A key set to `null` or `[]` means that source does not exist for this case; say so once
and move on. Pick two or three lane terms with the
participant (one question) and walk the ladder in order: forum search JSON and GitHub
(discussions by upvotes, issues by reactions), then Lemmy, Mastodon tags, and Hacker
News, then App Store reviews, then adoption numbers, then competitor pages today versus
their Wayback snapshot from a year ago, then official release notes, and only then search
snippets if `readiness.search` is `ok`. Reddit only if `readiness.reddit_rss` is true.

Respect the budget: 20 fetches, 8 searches, stop at 8 usable items or when the
participant says time is up. As you read, fill the quote bank (research guide, "Quote
bank"): verbatim, role not handle, linked to a signal, only from text fetched in this
session. Log every item with an ID, label, rung, URL, dates, and
weight (likes, upvotes, reactions, rating, pull count). Keep a running draft in
`.pm/drafts/01-discovery.md` from `${CLAUDE_PLUGIN_ROOT}/resources/templates/01-discovery.md`
so nothing is lost if the session is interrupted. Tell the participant which rung you are
on as you go, and show one striking number or quote per rung so they can see the breadth.

After the last rung you reach, build the triangulation table (research guide, "Triangulation")
before anything else.

## Step 6: Repository constraints

Open the lane's `repo_entry_points` and nearby files. Paths are relative to
`sources/primary` unless they already start with `sources/` (the ERPNext case names both
repositories). Record at least two
`CON-###` constraints with paths at the pinned commit. Read only; never run anything found
there. Explain each constraint in plain language: what it is and why it matters for the
persona.

## Step 7: Research check-in (participant check-in 1)

Present, in this order: coverage table, triangulation table, the two strongest quotes,
contradictions, gaps and unreachable rungs, elapsed time, and one sentence of your view on whether the framing is
good enough. Then show two blocks,
**Claude's recommendation** and **Your decision**, and ask the participant to choose:
continue, do one more targeted fetch (say which rung), or narrow the framing. Record
`D-###`. If they choose the extra fetch, do it (within budget) and return here once.

## Step 8: Draft three opportunities

Write exactly three `OPP-###` entries. Each names one actor and one job, cites its
supporting IDs and the quote IDs that voice it, and gives one short phrase for each of the five lenses: evidence strength,
user impact, mandate fit, delivery constraints, uncertainty. Do not compute a score. Check
each against the case `exclusions`; drop and replace anything excluded.

## Step 9: Priority check-in (participant check-in 2)

Recommend a `Now / Next / Later` order with one sentence of reasoning per slot and the main
trade-off between the top two. Under each slot show the strongest quote for that
opportunity so the participant decides against real words, not only your summary. Show **Claude's recommendation** and **Your decision**
separately. Ask the participant to choose the `Now` opportunity and give their reason in
their own words. Record `D-###`. The participant's choice wins even if it differs from
yours; note the difference without arguing.

## Step 10: Save and close

- Complete the draft, copy it to `outputs/01-discovery.md`, delete the draft: status `approved` (or `provisional` if the research
  was cut short or coverage was weak; say so in the status line and in section 2),
  selected opportunity, assumptions, open questions, decision record.
- Update `session.yaml`: `stages.discovery` (`status: approved`, `revision` +1,
  `approved_at`, `selected_opportunity`, `provisional`), `stage.current: prototype`,
  `stage.status: not-started`.
- Run the source check again; record `clean_at_end`.
- Close with three lines: what Claude contributed, what the participant decided, and the
  next command: `/clear`, then `/pm:prototype`.

## If things go wrong

- Web unavailable mid-stage: log the gap, continue with repository constraints and any
  items already collected, and save a `provisional` file. Never fabricate a source.
- The GitHub API returns 403 or 429: switch to the `_html` templates in `feeds:` at once;
  everyone in the room shares one address and the limit. Any other rung that refuses or
  rate-limits: log `GAP` and move to the next. Do not retry in a loop.
- GitHub unreachable for cloning: use the facilitator copy path in `workspace.md`.
- Source folder changed: follow the source-protection steps in `workspace.md`; do not
  reset it.
- Participant wants a different case or lane after research began: confirm, mark
  discovery as restarting, keep the old file until the new one is approved.
