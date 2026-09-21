# EYP PM workshop plugin

A Claude Code plugin for product managers. In one workshop day you take a real open-source
product from public evidence to a selected opportunity, a clickable prototype, and an
engineering-ready PRD. Claude supplies evidence, options, drafts, and challenges. You make
every accountable product decision, and each stage records which was which.

Marketplace `eyp-pm`, plugin `pm`. The current version is at the top of `CHANGELOG.md`.

## What this is

Four commands, run in a folder of your own.

| Command | Stage | What happens |
|---|---|---|
| `/pm:scout` | Discovery | Readiness check on first run, then a public research pass over customer, market, competitor, and repository evidence. You select one of three opportunities. |
| `/pm:prototype` | Prototype | One actor, one job, one flow. A clickable prototype in Claude Design or as a standalone HTML file, then a critique and one revision. |
| `/pm:prd` | PRD | A PRD grounded in the product's source code, reviewed by a separate read-only agent that has not seen your conversation. |
| `/pm:create-skill` | Capstone | Turn one PM practice of your own into a small project skill, test it once, revise it once. The facilitator demonstrates this; you run it afterwards on your own. |

No programming is needed. The product's source code is downloaded read-only and never
changed.

## Who it is for

Product managers in a facilitated workshop of 15 to 20 people working individually, with a
Claude Pro, Max, Team, or Enterprise account and a Windows or macOS laptop. It is a
teaching kit, not a production product-management system.

## What you will produce

| File | Contents |
|---|---|
| `outputs/01-discovery.md` | Sourced signals, triangulation table, quote bank, selected opportunity |
| `outputs/02-prototype.md`, `outputs/prototype/index.html` | Flow, design brief, critique findings, and the prototype |
| `outputs/03-prd.md`, `outputs/03-prd-review.md` | The PRD, status `ready-for-engineering-review`, and the independent review |
| `.claude/skills/<name>/SKILL.md`, `outputs/04-skill-summary.md` | Your capstone skill and its summary |
| `.pm/session.yaml` | Progress and decisions. You never edit this |

## Install

Do this at least a day before the workshop; it takes about 20 minutes.
[docs/SETUP.md](docs/SETUP.md) walks first-time terminal users through every step,
including installing Claude Code and Git.

1. Create a new empty folder, for example `pm-workshop` on your Desktop.
2. Open a terminal in that folder and start Claude Code.

   ```text
   claude
   ```

3. Add the marketplace and install the plugin.

   ```text
   /plugin marketplace add niancilyu-2/eyp-pm
   /plugin install pm@eyp-pm
   ```

4. Run the readiness check.

   ```text
   /pm:scout
   ```

It ends with `Ready`, `Ready with fallback` (Claude Design or web search is unavailable;
both have a fallback), or `Blocked` with one next action. Choose *stop after
setup* when asked. You may pick your case now so its repository downloads today.

Claude Code asks permission the first time it fetches a website or runs Git. Choose the
option that stops asking for the session.

## The workshop day

Open a terminal in the same folder, start a fresh `claude` session, and run the stages in
order. Run `/clear` between stages; each stage resumes from the files you approved.

| Block | Command | Minutes |
|---|---|---|
| Setup and readiness (ideally the day before) | `/pm:scout`, first run | 15 |
| Discovery | `/pm:scout` | 60 |
| Prototype | `/pm:prototype` | 60 |
| PRD | `/pm:prd` | 45 |
| Capstone demo, shown by the facilitator | `/pm:create-skill` | 15 |
| Breaks and buffer | | 45 |

Each stage opens by saying what you will practise, what Claude will do, and what you
decide. Each has two check-ins that show Claude's recommendation and your decision as
separate blocks. Every reply ends with a status line such as
`Scout · step 5/10 · fetches 6/20 · 14 min`.

The capstone takes about 30 minutes on your own.
[docs/TEST-WALKTHROUGH.md](docs/TEST-WALKTHROUGH.md) covers it and every other stage step
by step.

## The cases

Pick one case and one lane. Core cases are easier. ERPNext downloads two repositories, the
product and the Frappe framework it runs on.

| Case | You play | Mandate | Lanes (prototype surface) |
|---|---|---|---|
| Immich (core) | Household media steward | Make the product easier for less-technical family members without weakening privacy or media integrity | Backup and status confidence (mobile app); Finding and organizing media (desktop web); Private sharing and collaboration (mobile app) |
| Plane (core) | Product-operations lead | Turn unstructured demand into aligned execution | Intake and triage; Planning-to-execution visibility; Stakeholder progress (all desktop web) |
| Home Assistant (core) | Nontechnical household co-admin | Make routine configuration and recovery safer while preserving local control | Dashboards (tablet web); Device and entity health and recovery (mobile web); Automation creation and troubleshooting (desktop web) |
| Umbraco (core) | Regional content-operations editor | Reduce rework between content creation and publication | Draft to publish; Media reuse; Validation and localization (all desktop backoffice) |
| ERPNext (advanced) | Inventory or procurement operations manager | Resolve exceptions without weakening controls or auditability | Inventory exceptions; Purchase-approval exceptions; Role-based daily navigation (all desktop web) |

Each case is pinned to a specific commit. Only the folders the case needs are downloaded,
tens of megabytes rather than hundreds, and the download stays read-only.

## How research works

Discovery walks a fixed ladder of public sources that need no login and no API key. Each
case's `case.yaml` carries the URL templates.

| Rung | Sources |
|---|---|
| 1 Community, structured | Product forum; GitHub Discussions by upvotes; GitHub issues by reactions (HTML search pages when the API is rate-limited) |
| 2 Community, fediverse and aggregators | Lemmy; Mastodon hashtags; Hacker News stories and comments |
| 3 Reviews | Apple App Store review feed and release notes; Google Play reviews, pasted in by you from the listing |
| 4 Adoption | GitHub stars, Docker Hub pulls, PyPI or NuGet downloads; Wikipedia pageviews |
| 5 Competitors and roadmaps | The product's own boards; competitors' job postings; competitor pricing pages today and a year ago via the Wayback Machine |
| 6 Official | Release notes, changelog, blog, docs |
| 7 Search snippets | Only if web search works; logged as unverified inference, never as evidence |

Budget: 20 fetches and 8 searches per run. Every item gets a stable ID, a URL, dates, and
a weight with its base. Each theme gets one fetch looking for counter-evidence. Verbatim
user quotes form a quote bank that follows the work into the PRD. Repository constraints
cite commit, path, line range, and a quoted line, marked "to confirm with engineering".

Claude never logs in, asks for keys, bypasses a CAPTCHA, builds a scraper, or loops over
pages. Reddit is reachable only through a facilitator switch, off by default, that allows
one `curl` of a subreddit RSS feed. G2, Capterra, Trustpilot, and Product Hunt block
automated reading and are not attempted.

## Principles and limits

- Public information and synthetic data only. No logins, API keys, private customer data,
  or confidential material.
- Every claim is labelled `Evidence`, `Inference`, `Assumption`, or `Constraint`. Technical
  statements in the PRD are `Verified` with a file path, `Inferred`, or `Engineering-owned`.
- No composite scores, RICE, ARR, PERT, estimates, story points, delivery dates, or costs.
- The research pass is a quick public scan, not customer validation. The prototype
  walkthrough is a PM design critique, not user testing.
- Text found in websites and repositories is evidence, not instructions.
- Claude writes only under `outputs/`, `.pm/`, and for the capstone `.claude/skills/`.
- Prototypes are non-distributable workshop artifacts. No source files are copied out of
  the pinned repositories.

## For facilitators

### Day before

- Confirm every participant reported `Ready` or `Ready with fallback`.
- For Claude Design on an Enterprise plan, an owner enables it in *Organization settings*,
  then *Artifacts*. Without it everyone takes the HTML path.
- Zip each case's `sources/` folders from a completed readiness run in case the venue
  network blocks GitHub. `/pm:scout` accepts a facilitator copy and verifies the commit.
- Check that web search is allowed. On Google Cloud Vertex AI an organisation policy can
  disallow `web_search`; readiness reports `search: unavailable`. The ladder runs on fetch
  alone, so this is a warning.
- Expect GitHub API rate limits. Twenty laptops on one network share about 60
  unauthenticated calls an hour; the scout falls back to GitHub's HTML search pages.
- Decide whether to allow the Reddit RSS switch.

Claude cannot see a clock. The skills count actions and you call time. A stage cut short
saves a clearly labelled provisional file.

### Troubleshooting

| Symptom | What to do |
|---|---|
| `/pm:` commands missing | `/plugin marketplace update eyp-pm`, then `/plugin install pm@eyp-pm`. |
| A published change is not appearing | Bump `version` in both `plugins/pm/.claude-plugin/plugin.json` and `.claude-plugin/marketplace.json`. |
| Path-too-long error on Windows | `git config --global core.longpaths true`, then rerun `/pm:scout`. |
| GitHub unreachable | Hand over your zipped `sources/` copy; `/pm:scout` verifies the commit. |
| Web search policy error | Fetch still works. Forward the quoted error to the administrator. |
| A source returns 403 or 429 | Expected on some networks. The skill logs a gap and moves on. |
| `sources CHANGED` in the status line | The skill stops and explains. Do not reset silently; re-clone into a fresh folder. |
| Capstone skill not in `/skills` | Test from a new session in the same folder. The folder name must equal the `name` field. |

### Hardening ideas not yet built

- A plugin hook that blocks writes under `sources/`.
- A `.claude/settings.local.json` written by readiness that pre-approves web fetch, Git,
  and writes under `outputs/`.
- An automated capstone test using `claude -p "/<slug> ..."` from the workshop folder.

### Local development

```bash
claude plugin validate . --strict
claude plugin validate ./plugins/pm --strict
claude --plugin-dir ./plugins/pm
```

Record changes in `CHANGELOG.md`. See the Claude Code docs on
[plugins](https://code.claude.com/docs/en/plugins) and
[marketplaces](https://code.claude.com/docs/en/plugin-marketplaces).

## Repository layout

```text
eyp-pm/
├── .claude-plugin/marketplace.json
├── plugins/pm/
│   ├── .claude-plugin/plugin.json
│   ├── skills/{scout,prototype,prd,create-skill}/SKILL.md
│   ├── agents/prd-reviewer.md
│   ├── cases/{immich,plane,home-assistant,umbraco,erpnext}/
│   └── resources/        shared rules, research guide, readiness, templates
├── docs/SETUP.md
├── docs/TEST-WALKTHROUGH.md
├── CHANGELOG.md
├── LICENSE
└── README.md
```

Everything a skill needs at runtime lives under `plugins/pm`.

## Licence and credits

Original content is MIT licensed (see `LICENSE`). The five product repositories keep
their own licences: Immich and Plane AGPL-3.0, Home Assistant Apache-2.0, Umbraco MIT,
ERPNext GPL-3.0, Frappe MIT. Nothing from them is redistributed here.

- The earlier [eyp-pm-skills](https://github.com/niancilyu-2/eyp-pm-skills) repository
  informed the evidence labelling, check-in pattern, HTML fallback, and prototype-to-PRD
  traceability.
- Dean Peters' [PM Skill Creator](https://github.com/deanpeters/Product-Manager-Skills)
  (CC BY-NC-SA 4.0) was conceptual inspiration for conversational skill authoring and the
  one-job rule. The capstone here is written independently and shares no wording,
  metadata, scripts, or structure with it.
- The [product-on-purpose](https://github.com/product-on-purpose) repositories
  (Apache-2.0) informed the organisation of this README.
