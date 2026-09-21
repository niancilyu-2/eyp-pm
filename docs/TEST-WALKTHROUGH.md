# Test walkthrough

How to run the whole flow yourself. Readiness and Discovery take about 75 minutes. The
three workshop stages take about three hours; the capstone is a 30-minute exercise you do
afterwards. Everything Claude produces lands in files you can open in
any text editor.

Not installed yet? Do [SETUP.md](SETUP.md) first.

## 1. Start

Open a terminal in your `pm-workshop` folder and type `claude`.

## 2. Readiness

Type `/pm:scout`. On the first run it:

1. prints the plugin version
2. checks the folder and Git
3. opens a test page in your browser and asks whether it appeared
4. fetches one public page and tries one web search
5. asks you to type `/design` and say what happened
6. offers to pick your case now so the download happens today
7. prints `Ready`, `Ready with fallback`, or `Blocked`

Choose *stop after setup* to continue another day, or *continue* to go straight on.

From here every reply ends with a status line:
`Scout · step 5/10 · fetches 6/20 · searches 0/8 · items 5 · quotes 3/8 · 14 min · sources clean`.
If it ever says `sources CHANGED`, stop and note the step.

## 3. Pick a case and lane

Fifteen combinations. Core cases are easier. ERPNext downloads two repositories.

| Case | You play | Lane | Prototype screen | Download |
|---|---|---|---|---|
| **Immich** (core) | Household media steward | Backup and status confidence | Phone, 390×844 | one repo, about 30 MB |
| | | Finding and organizing media | Desktop, 1440×900 | |
| | | Private sharing and collaboration | Phone, 390×844 | |
| **Plane** (core) | Product-operations lead | Intake and triage | Desktop, 1440×900 | one repo, about 20 MB |
| | | Planning-to-execution visibility | Desktop, 1440×900 | |
| | | Stakeholder progress | Desktop, 1440×900 | |
| **Home Assistant** (core) | Household co-admin | Dashboards | Tablet, 1024×768 | one repo, under 60 MB |
| | | Device and entity health and recovery | Phone, 390×844 | |
| | | Automation creation and troubleshooting | Desktop, 1440×900 | |
| **Umbraco** (core) | Content-operations editor | Draft to publish | Desktop, 1440×900 | one repo, under 60 MB |
| | | Media reuse | Desktop, 1440×900 | |
| | | Validation and localization | Desktop, 1440×900 | |
| **ERPNext** (advanced) | Inventory or procurement manager | Inventory exceptions | Desktop, 1440×900 | two repos, under 60 MB each |
| | | Purchase-approval exceptions | Desktop, 1440×900 | |
| | | Role-based daily navigation | Desktop, 1440×900 | |

Research differs a little by case. Immich, Home Assistant, Umbraco, and ERPNext have a
searchable forum; Plane's community is on Discord, which Claude cannot read. Immich,
Plane, and Home Assistant have a phone app, so App Store reviews are included. ERPNext has
no GitHub Discussions board.

Fullest research demo: Immich or Home Assistant. Quickest run: Plane, Intake and triage.

## 4. Discovery: `/pm:scout` (60 min)

Claude asks for the case, then the lane, gives a short tour, downloads the repository,
and asks for two or three search words. Then it works through the sources in order: forum,
GitHub discussions by upvotes, GitHub issues by reactions, Lemmy, Mastodon, Hacker News,
App Store reviews, adoption numbers, competitor pages against their copy from a year ago,
release notes. It shows one number or quote per source and collects user quotes as it
goes.

Two check-ins:

1. **Research check.** Coverage, a triangulation table, the two strongest quotes,
   contradictions, gaps. You say continue, ask for one more fetch, or narrow the framing.
2. **Priority check.** Three opportunities in Now, Next, Later order with a quote under
   each. You pick the Now item and give your reason.

Check: every claim is labelled Evidence, Inference, Assumption, or Constraint; every
Evidence item has a link and dates; Claude's recommendation and your decision are separate
blocks.

Files: `outputs/01-discovery.md`, `.pm/session.yaml`. Type `/clear`.

## 5. Prototype: `/pm:prototype` (60 min)

Claude shows the quotes behind your chosen opportunity and asks: one actor, one job, where
the task starts and ends, the main action, two alternate states. **Flow check-in:** approve
or edit.

Claude reads the product's design files and writes a brief. Then one of two paths:

- **Claude Design available.** Claude prints one instruction: type `/design`, paste the
  brief, ask Design to export a single HTML file to `outputs/prototype/index.html`, then
  run `/pm:prototype` again.
- **Not available.** Claude writes the HTML file itself and opens it.

Double-click `outputs/prototype/index.html`. Click through the main path, use the demo
buttons to see the two alternate states, try Tab to move between controls.

Claude walks the flow with you, lists findings, and proposes one change tied to evidence.
**Prototype check-in:** approve the revised version, note any trade-offs.

Files: `outputs/02-prototype.md`, `outputs/prototype/index.html`. Type `/clear`.

## 6. PRD: `/pm:prd` (45 min)

Claude reads the earlier files, looks at the code for likely touchpoints, and drafts the
PRD. **Scope check-in:** approve goals, non-goals, behaviour, alternate states, acceptance
criteria, and pick two of three customer quotes.

A separate read-only reviewer checks the draft and returns findings. **Handoff check-in:**
for each finding, fix it in the PRD, turn it into an engineering question, or accept it
with a note. Blocking findings must be fixed.

Check: technical statements are labelled Verified with a file path, Inferred, or
Engineering-owned; no estimates, delivery dates, or story points; status reads
`ready-for-engineering-review`.

Files: `outputs/03-prd.md`, `outputs/03-prd-review.md`. Type `/clear`.

## 7. Capstone: `/pm:create-skill` (30 min, on your own after the workshop)

The facilitator demonstrates this stage on the day; you run it yourself afterwards in the
same folder. Independent of the other stages. Bring one repeated task from your job, or paste a
checklist you already use.

Claude asks four questions, shows a proposed skill at the **scope check-in**, then writes
`.claude/skills/<name>/SKILL.md` and validates it. Claude Code asks permission before
writing that file even if you allowed edits earlier; choose Yes.

To test: open a second terminal in the same folder, type `claude`, type `/skills` and
find your command, run it with sample input. Paste the result back into the first window.
Claude proposes at most one revision and asks what the skill automates and what stays with
you.

Files: `.claude/skills/<name>/SKILL.md`, `outputs/04-skill-summary.md`.

## 8. Problems

| You see | Do |
|---|---|
| `/pm:` commands missing | `/plugin marketplace update eyp-pm`, then `/plugin install pm@eyp-pm` |
| Repeated permission prompts | Choose the option that stops asking for the session |
| `Blocked` | Do the action printed, then `/pm:scout` again |
| Web search policy error | Continue. Send the error text to whoever manages your account |
| A source returns 403 or 429 | Claude logs a gap and moves on |
| Path-too-long error on Windows | `git config --global core.longpaths true`, then `/pm:scout` |
| `/design` does nothing | Claude uses the HTML path |
| `sources CHANGED` in the status line | Stop and note the step |
| Lost your place | `/clear`, then the current stage command. It resumes from the files |
| Capstone skill not in `/skills` | Test from a new session in the same folder. The folder name must match the `name` line in the file |

## 9. Report back

Per stage:

1. Finished? How long?
2. One thing Claude did that helped, one decision you kept.
3. Anything confusing or wrong, with the step number.

Send the files in `outputs/` if you can. They contain only public information and made-up
data.
