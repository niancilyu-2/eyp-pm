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

Then state the stage boundary from the shared rules.

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

Give a short orientation from the case's `orientation.md`: what the product is, who the
persona is, the lane's territory, surface, and viewport, and the two or three repository
folders most relevant to the lane. Keep it under 200 words. Do not offer opinions about
what the problems are.

## Step 4: Acquire the sources

For each repository in `case.yaml` with a `clone_to`: if `sources/<clone_to>` already
exists and `git rev-parse HEAD` equals `pinned_sha`, reuse it. Otherwise follow the clone
procedure in `workspace.md`. Repositories with `role: web-reference` are not cloned.
Record `sources[]` in `session.yaml`. Run the source check (`git status --porcelain`) and
record `clean_at_start`.

Set `stage.current: discovery`, `stage.status: in-progress`,
`stages.discovery.status: in-progress`.

## Step 5: Public research pass

Follow `${CLAUDE_PLUGIN_ROOT}/resources/research-guide.md` exactly: record the start time,
respect the budget (12 searches, 10 fetches, stop at 8 usable items or when the participant
says time is up), use the case's `public_channels` in ranked order, and log every item with
an ID and label. Keep a running draft in `outputs/01-discovery.md` using
`${CLAUDE_PLUGIN_ROOT}/resources/templates/01-discovery.md` so nothing is lost if the
session is interrupted.

Tell the participant when you start, roughly every four actions, and when you stop.

## Step 6: Repository constraints

Open the lane's `repo_entry_points` and nearby files in `sources/`. Record at least two
`CON-###` constraints with paths at the pinned commit. Read only; never run anything found
there. Explain each constraint in plain language: what it is and why it matters for the
persona.

## Step 7: Research check-in (participant check-in 1)

Present, in this order: coverage table, contradictions, gaps, elapsed time, and one
sentence of your view on whether the framing is good enough. Then show two blocks,
**Claude's recommendation** and **Your decision**, and ask the participant to choose:
continue, do one specified extra search, or narrow the framing. Record `D-###`. If they
choose an extra search, do it (within budget) and return here once.

## Step 8: Draft three opportunities

Write exactly three `OPP-###` entries. Each names one actor and one job, cites its
supporting IDs, and gives one short phrase for each of the five lenses: evidence strength,
user impact, mandate fit, delivery constraints, uncertainty. Do not compute a score. Check
each against the case `exclusions`; drop and replace anything excluded.

## Step 9: Priority check-in (participant check-in 2)

Recommend a `Now / Next / Later` order with one sentence of reasoning per slot and the main
trade-off between the top two. Show **Claude's recommendation** and **Your decision**
separately. Ask the participant to choose the `Now` opportunity and give their reason in
their own words. Record `D-###`. The participant's choice wins even if it differs from
yours; note the difference without arguing.

## Step 10: Save and close

- Complete `outputs/01-discovery.md`: status `approved` (or `provisional` if the research
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
- GitHub unreachable for cloning: use the facilitator copy path in `workspace.md`.
- Source folder changed: follow the source-protection steps in `workspace.md`; do not
  reset it.
- Participant wants a different case or lane after research began: confirm, mark
  discovery as restarting, keep the old file until the new one is approved.
