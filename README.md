# EYP PM: Claude Code workshop plugin

One Claude Code plugin, `pm`, that takes product managers through a connected exercise:

1. **Discovery.** Compare public customer, market, competitor, and repository evidence
   and choose one opportunity. `/pm:scout`
2. **Rapid prototyping.** Turn it into a small clickable prototype. `/pm:prototype`
3. **PRD.** Turn the approved prototype into an engineering handoff grounded in the
   product's repository, with an independent read-only review. `/pm:prd`

The workshop closes with a separate 30-minute capstone, `/pm:create-skill`, in which each
participant turns one of their own repeatable PM practices into a small project skill,
tests it once, and revises it once.

Claude supplies evidence, options, drafts, and challenges. The PM makes every accountable
product decision, and every stage records which was which.

The Discovery stage is built around a source ladder: keyless public feeds that most desk
research never touches. Forum search JSON, GitHub discussions ranked by upvotes and issues
ranked by reactions, Lemmy and Mastodon, Hacker News, Apple App Store review feeds,
Docker Hub and package download counts, and Wayback Machine snapshots of competitor pricing
pages from a year ago. Every item carries its URL, dates, and weight, and the stage ends
with a triangulation table showing which themes appear across several sources and a quote
bank of users' own words that follows the work into the PRD. No logins, no API keys, no
scraping.

This is a workshop kit for 15–20 participants working individually, not a production
product-management system. No programming is required and the selected product's source
code is never changed.

---

## For participants

New to Claude Code? Start with [docs/SETUP.md](docs/SETUP.md), a 20-minute setup guide
for first-time terminal users. Testing the whole flow? Follow
[docs/TEST-WALKTHROUGH.md](docs/TEST-WALKTHROUGH.md), a step-by-step script covering every
case and lane.

### Before the workshop (at least 24 hours ahead, about 20 minutes)

1. Install Claude Code on Windows or macOS and sign in. You need Git as well: on Windows,
   install Git for Windows; on macOS, run `xcode-select --install` in Terminal if `git
   --version` fails.
2. Create a **new empty folder** for the workshop, for example `pm-workshop` on your
   Desktop. Do not use an existing project folder.
3. Open a terminal in that folder (Windows: right-click the folder and choose *Open in Terminal*;
   macOS: right-click and choose *New Terminal at Folder*) and run `claude`.
4. Inside Claude Code, install the plugin:

   ```text
   /plugin marketplace add niancilyu-2/eyp-pm
   /plugin install pm@eyp-pm
   ```

5. Run `/pm:scout`. The first run is a readiness check. It ends with one of:
   - `Ready`: you are set.
   - `Ready with fallback`: you are set. Claude Design is not available, so the prototype
     will be a standalone HTML file instead. This is fine.
   - `Blocked`: follow the single next action it prints, then run `/pm:scout` again.
6. When asked, choose **stop after setup**. You may also pick your case now so its
   repository downloads today; you can still change it on the day.

Claude Code will ask permission the first time it fetches a website or runs a git command.
Choosing the option that stops asking for the rest of the session saves a lot of clicks.

### On the workshop day

Open a terminal in the same folder, start a **fresh** `claude` session, and run the
commands in order. Run `/clear` between stages; everything you approved is saved in files.

| Stage | Command | Time | You leave with |
|---|---|---|---|
| Discovery | `/pm:scout` | 60 min | `outputs/01-discovery.md`, one selected opportunity |
| Prototype | `/pm:prototype` | 60 min | `outputs/02-prototype.md`, `outputs/prototype/index.html` |
| PRD | `/pm:prd` | 45 min | `outputs/03-prd.md`, `outputs/03-prd-review.md` |
| Capstone | `/pm:create-skill` | 30 min | `.claude/skills/<your-skill>/SKILL.md`, `outputs/04-skill-summary.md` |

Each stage opens by telling you what you will practise, what Claude will do, and what you
decide. Each check-in shows Claude's recommendation and your decision side by side.

### The five cases

| Case | Persona and mandate | Lanes |
|---|---|---|
| **Immich** (core) | Household media steward; make the product easier for less-technical family members without weakening privacy or media integrity | Backup and status confidence · Finding and organizing media · Private sharing and collaboration |
| **Plane** (core) | Product-operations lead; turn unstructured demand into aligned execution | Intake and triage · Planning-to-execution visibility · Stakeholder progress |
| **Home Assistant** (core) | Nontechnical household co-admin; make routine configuration and recovery safer while preserving local control | Dashboards · Device and entity health and recovery · Automation creation and troubleshooting |
| **Umbraco** (core) | Regional content-operations editor; reduce rework between content creation and publication | Draft to publish · Media reuse · Validation and localization |
| **ERPNext** (advanced) | Inventory or procurement operations manager; resolve exceptions without weakening controls or auditability | Inventory exceptions · Purchase-approval exceptions · Role-based daily navigation |

Each case is pinned to a specific commit of its public repository. Only the folders the
case needs are downloaded (roughly 15–60 MB rather than hundreds), and the download is
read-only throughout.

### Your workshop folder

```text
pm-workshop/
├── .claude/skills/        # your capstone skill lands here
├── sources/               # read-only downloads of the product repository
├── outputs/               # everything you produce
│   ├── 01-discovery.md
│   ├── 02-prototype.md
│   ├── prototype/index.html
│   ├── 03-prd.md
│   ├── 03-prd-review.md
│   └── 04-skill-summary.md
└── .pm/session.yaml       # progress and decisions; you never edit this
```

---

## For facilitators

### Day-before checklist

- Confirm every participant reported `Ready` or `Ready with fallback`.
- If your organisation is on an Enterprise plan and you want Claude Design, an owner must
  enable it in *Organization settings*, then *Artifacts* beforehand. Without it, every participant
  gets the HTML path, which works fine.
- Prepare offline copies of the five sparse clones (zip each `sources/primary` and
  `sources/companion` folder from a completed readiness run) in case the venue network
  blocks GitHub. `/pm:scout` knows how to accept a facilitator copy and verifies the commit.
- Have the plugin version handy: participants see it at the start of `/pm:scout`.
- Check that web search is allowed for participants' accounts. On Google Cloud Vertex AI
  deployments an organisation policy can disallow `web_search` for the model; the readiness
  check reports `search: unavailable` and quotes the error. The source ladder works on fetch
  alone, so this is a warning, but ask the administrator to allow it if you can.
- Expect GitHub API rate limits. Twenty laptops on one wifi share about 60 unauthenticated
  API calls an hour. The scout falls back to GitHub's HTML search pages automatically; if
  you see many 403s early, that is why.
- Decide whether to allow the Reddit RSS switch. Claude Code's fetch tool refuses reddit.com;
  the only Reddit path is one `curl` of a subreddit RSS feed, off by default.

### Time budget

Setup 15 · Discovery 60 · Prototype 60 · PRD 45 · Capstone 30 · breaks and buffer 30.
Claude cannot see a clock; the skills count actions and you call time. A stage cut short
saves a clearly labelled *provisional* file rather than a fabricated one.

### Troubleshooting

| Symptom | What to do |
|---|---|
| `/pm:` commands missing | `/plugin marketplace update eyp-pm`, then `/plugin install pm@eyp-pm` again. Check the version printed by `/pm:scout` matches this repository. |
| Changes you published are not appearing | Bump `version` in both `plugins/pm/.claude-plugin/plugin.json` and `.claude-plugin/marketplace.json`; installs refresh only on a version change. |
| Clone fails on Windows with a path-too-long error | The clone procedure sets `core.longpaths true`; if it still fails, run `git config --global core.longpaths true` and rerun `/pm:scout`. |
| GitHub unreachable | Give the participant your zipped copy of the case's `sources/` folder; `/pm:scout` verifies the commit and records the copy. |
| Web fetch denied or unavailable | Research cannot run. Check the account's web tool settings; as a last resort pair the participant with someone whose fetch works and share findings verbally. |
| Web search returns a policy error | Fetch still works, so the source ladder runs. Forward the quoted error to the administrator (on Vertex AI: allow `web_search` for the model). |
| A feed rung returns 403 or 429 | Expected for some hosts on some networks. The skill logs a gap and moves on; do not retry in a loop. |
| `/design` does nothing | Readiness records `unavailable`; `/pm:prototype` uses the HTML path automatically. |
| `sources/` shows modified files | The skill stops and explains. Do not reset it silently; re-clone into a fresh folder if needed. |
| Session got long or confused | `/clear` and rerun the current stage command; it resumes from saved files. |
| Capstone skill not listed in `/skills` | It must be tested from a **new** session in the same folder. Check the folder name equals the `name` field and that `claude plugin validate .claude/skills --strict` passes. |

### Hardening ideas not in V1

- A plugin hook that blocks any write under `sources/` (currently guarded by
  `git status` checks and instructions).
- A workspace `.claude/settings.local.json` written by readiness that pre-approves web
  fetch, git, and writes under `outputs/`, to remove permission prompts.
- An automated capstone test using `claude -p "/<slug> …"` from the workshop folder,
  which is a fresh session and returns the result without a second terminal.

---

## Repository layout

```text
eyp-pm/
├── .claude-plugin/marketplace.json
├── plugins/pm/
│   ├── .claude-plugin/plugin.json
│   ├── skills/{scout,prototype,prd,create-skill}/SKILL.md
│   ├── agents/prd-reviewer.md
│   ├── cases/{immich,plane,home-assistant,umbraco,erpnext}/{case.yaml,orientation.md,design-notes.md}
│   └── resources/                 # shared rules, procedures, templates
├── README.md · CHANGELOG.md · LICENSE
```

Everything a skill needs at runtime lives under `plugins/pm` and is resolved with
`${CLAUDE_PLUGIN_ROOT}`.

### Local development

```bash
claude plugin validate . --strict
claude plugin validate ./plugins/pm --strict
claude --plugin-dir ./plugins/pm      # then type / to see the pm: commands
```

## Principles and limits

- Public information and synthetic data only. No logins, API keys, private customer data,
  or confidential material.
- Evidence is labelled `Evidence`, `Inference`, `Assumption`, or `Constraint`; technical
  statements are `Verified`, `Inferred`, or `Engineering-owned`. No composite scores, RICE,
  ARR, PERT, estimates, story points, dates, or costs.
- The research pass is a quick public scan, not customer validation or representative
  social listening. It reads published feeds and APIs that need no key, respects rate
  limits, and never logs in, bypasses a CAPTCHA, or loops over pages. The prototype walkthrough is a PM design critique, not user testing.
- Prototypes are non-distributable workshop artifacts. Source files are never copied out of
  the pinned repositories, which keep their own licences (Immich and Plane AGPL-3.0, Home
  Assistant Apache-2.0, Umbraco MIT, ERPNext GPL-3.0, Frappe MIT).

## Licence and credits

Original `eyp-pm` content is MIT licensed (see `LICENSE`). The earlier
[eyp-pm-skills](https://github.com/niancilyu-2/eyp-pm-skills) repository informed the
evidence labelling, check-in pattern, HTML fallback, and prototype-to-PRD traceability.
Dean Peters' [PM Skill Creator](https://github.com/deanpeters/Product-Manager-Skills)
(CC BY-NC-SA 4.0) was conceptual inspiration for conversational skill authoring and the
one-job rule; the capstone here is written independently and shares no wording, metadata,
scripts, or structure with it.

## References

- [Claude Code skills](https://code.claude.com/docs/en/skills)
- [Claude Code plugins](https://code.claude.com/docs/en/plugins)
- [Plugin marketplaces](https://code.claude.com/docs/en/plugin-marketplaces)
- [Plugin reference](https://code.claude.com/docs/en/plugins-reference)
- [Claude Design](https://support.claude.com/en/articles/14604416-get-started-with-claude-design)
