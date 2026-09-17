---
name: prototype
description: Prototype stage of the EYP PM workshop. Turn the selected opportunity into one focused user flow and a clickable prototype using Claude Design or a standalone HTML file, then improve it through a design critique. Run after /pm:scout.
disable-model-invocation: true
---

# /pm:prototype (Rapid prototyping)

You are guiding a product manager with limited technical experience. Read
`${CLAUDE_PLUGIN_ROOT}/resources/shared-rules.md` now and follow it for the whole stage.
This skill takes no arguments.

**Learning objective:** turn a selected opportunity into a focused flow and improve it
through conversational critique.

**Outcome:** `outputs/02-prototype.md` and a clickable `outputs/prototype/index.html`.

## Open the stage

Three lines: *You will practise* shaping one flow and improving it through critique.
*Claude will* propose a flow, generate the prototype, find missing states, and suggest
usability improvements. *You decide* the actor and job, the scope, the experience
trade-offs, and which revisions to accept.

State the stage boundary. Then re-hydrate:

- Read `.pm/session.yaml`. If `stages.discovery.status` is not `approved` (a `provisional`
  discovery counts as approved), or `prototype` is listed in `stale`, stop and say which
  skill to run.
- Read `outputs/01-discovery.md`, the selected `OPP-###`, and the case's `case.yaml`,
  `design-notes.md`, and lane entry (surface, viewport, `repo_entry_points`).
- If `stages.prototype.status` is `awaiting-design`, skip to **Step 6b (resume after
  Design)**. If `in-progress` or `approved`, offer Resume or Restart.
- Run the source check and record `clean_at_start`. Set `stage.current: prototype`,
  `stages.prototype.status: in-progress`.

## Step 1: Confirm the flow (short conversational questions)

Ask, one at a time or in pairs, with options drawn from the opportunity and the lane:

1. Who is the one actor, and what is the one job in their words?
2. Where does the task start, and what does "done" look like?
3. What is the single main action?
4. Which two alternate states matter most? Offer: empty, error, permission, recovery,
   loading. Explain each in a few words.

Constrain everything to the lane's surface and viewport, one main path, and two states.
If the participant proposes more, explain the workshop limit and ask which to keep.

## Step 2: Flow check-in (participant check-in 1)

Show the proposed actor, job, start and end, main path (3–7 numbered steps), and the two
states. Present **Claude's recommendation** (including anything you would cut or add) and
**Your decision**. The participant approves or edits. Record `D-###`.

## Step 3: Inspect the repository's UI patterns (read-only)

Using `design-notes.md` and the lane's entry points, open the design tokens, shared
components, layouts, and public UI references. Note what you actually verified (paths at
the pinned commit) versus what you are inferring. Do not copy source files into
`outputs/`. Run nothing.

## Step 4: Write the design brief

Fill `${CLAUDE_PLUGIN_ROOT}/resources/design-brief.md` and save it as
`.pm/design-brief.md`. Show it briefly and ask for a one-word go-ahead. Start
`outputs/02-prototype.md` from `${CLAUDE_PLUGIN_ROOT}/resources/templates/02-prototype.md`
with sections 1–6 filled.

## Step 5: Choose the generation path

Read `readiness.design` from `session.yaml`.

- `available` → **Step 6a (Claude Design)**. Mention that the HTML path is available if
  Design gives trouble.
- `unavailable` → **Step 7 (HTML)**. Say the HTML path is being used and why.
- `unknown` → ask the participant to type `/design` once and report; record the answer,
  then route as above.

## Step 6a: Claude Design

Claude Design runs inside Claude Code as `/design`, but this skill cannot start it for you.
Give the participant **one** instruction, exactly:

> Type `/design` and paste the brief I saved at `.pm/design-brief.md` (I will print it
> below). When the design is ready, ask Design to export it as a single standalone HTML
> file saved to `outputs/prototype/index.html`. Then run `/pm:prototype` again and I will
> continue from here.

Print the brief. Set `stages.prototype.status: awaiting-design`, `stages.prototype.path:
design`, append a note, and end the turn.

## Step 6b: Resume after Design

Check for `outputs/prototype/index.html`.

- If it exists: open it, confirm it is a single self-contained file that opens from the
  file system without network requests (search it for `http://` and `https://` script,
  link, font, or image references; note any you find). If it depends on the network or is
  not a single file, ask Design once more for a standalone export or offer the HTML path.
  Then continue to **Step 8**.
- If it does not exist: ask whether Design produced a file elsewhere (offer to copy it
  into place) or whether to switch to the HTML path. Do not depend on a Design share
  link; the PRD stage needs the file.

## Step 7: HTML fallback

Build `outputs/prototype/index.html` yourself following
`${CLAUDE_PLUGIN_ROOT}/resources/prototype-html-guide.md`: one file, no network, no
build step, lane viewport, main path clickable end to end, both states reachable,
keyboard-usable, synthetic data, "Prototype · synthetic data" badge. Use verified tokens
and patterns from Step 3 and list them in section 6 of the notes. Set
`stages.prototype.path: html-fallback`. Open the file for the participant (`start`,
`open`, or `xdg-open`).

## Step 8: Walkthrough (PM design critique)

Open the file for the participant if not already open. Walk the main path and each state
together, using the checklist in `prototype-html-guide.md`: labels, navigation, keyboard,
state clarity, obvious contrast, scope. Record findings in section 8 of the notes. This is
a design critique, not customer validation; say so once.

## Step 9: One evidence-tied revision

Propose one change tied to a discovery item (`SIG-###`/`CON-###`) or a walkthrough
finding. Make at least one change after approval, in the HTML (either path; if Design
produced the file, edit the exported HTML directly or ask the participant to make the
change in Design and re-export). Record before → after in section 9.

## Step 10: Prototype check-in (participant check-in 2)

Show the findings, the revision made, remaining limitations, and where the prototype
differs from the product UI. Present **Claude's recommendation** and **Your decision**.
The participant approves the revised experience and records deliberate trade-offs.
Record `D-###`.

## Step 11: Save and close

- Complete `outputs/02-prototype.md` (status `approved`, all 11 sections).
- Update `session.yaml`: `stages.prototype` (`status: approved`, `revision` +1,
  `approved_at`, `path`), `stage.current: prd`, `stage.status: not-started`.
- Source check; record `clean_at_end`.
- Close with three lines: what Claude contributed, what the participant decided, next
  command: `/clear`, then `/pm:prd`.

## If things go wrong

- Design produces nothing reusable: switch to the HTML path; note it in section 7.
- The participant changes actor, job, or states after approval: save the change, mark
  `prd` stale if it exists, and re-run from Step 2.
- Visual similarity is not a completion gate. A clear, testable flow is.
