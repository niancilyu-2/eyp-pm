# pm: product discovery to PRD for Claude Code

A Claude Code plugin that takes a product manager from public evidence to a selected
opportunity, a clickable prototype, and an engineering-ready PRD, on a real open-source
product. Claude finds, drafts, and challenges. The PM decides, and every stage records
which was which.

No programming required. The product's source code is downloaded read-only and never
changed. One PM can run it alone; a group can run it together.

```text
/plugin marketplace add niancilyu-2/eyp-pm
/plugin install pm@eyp-pm
```

## Contents

- [Where it fits](#where-it-fits)
- [Commands](#commands)
- [Requirements](#requirements)
- [Getting started](#getting-started)
- [Products you can work on](#products-you-can-work-on)
- [How research works](#how-research-works)
- [Principles and limits](#principles-and-limits)
- [Documentation](#documentation)
- [Repository layout](#repository-layout)
- [Licence and credits](#licence-and-credits)

## Where it fits

A typical product lifecycle runs from vision and strategy through discovery to a validated
PRD and on to execution. This plugin covers the discovery work that can be done from public
evidence and a product's own code, and the PRD that comes out of it. The highlighted steps
are the ones it performs.

```mermaid
flowchart TB
  V["Vision"] --> S["Strategy"]
  S --> D
  subgraph D["Discovery"]
    direction TB
    CR["Customer research<br/>public evidence, quotes, triangulation"]
    PT["Prototypes<br/>one flow, clickable"]
    EX["Experiments"]
    FC["Feasibility checks<br/>repository constraints, engineering review"]
  end
  D --> P["Validated PRD<br/>traceable requirements and acceptance criteria"]
  P --> E["Execution"]
  classDef covered fill:#2F5BEA,stroke:#2F5BEA,color:#FFFFFF
  classDef partial fill:#DCE3F9,stroke:#2F5BEA,color:#1A1A1A
  classDef outside fill:#FAF9F5,stroke:#9A9A9A,color:#5B5F66
  class CR,PT,FC,P covered
  class V,S,EX,E outside
```

Outside its scope: setting vision and strategy, running live experiments with real users,
and execution. The research it does is a public scan, not customer validation, and the PRD
it produces is marked ready for engineering review, not ready to build.

## Commands

| Command | What it does | You leave with |
|---|---|---|
| `/pm:scout` | Readiness check on first run, then a research pass over a fixed ladder of public sources, repository constraints, three opportunities, and your choice of one | `outputs/01-discovery.md` |
| `/pm:prototype` | One actor, one job, one flow. A clickable prototype in Claude Design or as a standalone HTML file, a design critique, one revision | `outputs/02-prototype.md`, `outputs/prototype/index.html` |
| `/pm:prd` | A PRD grounded in the product's code, reviewed by a separate read-only agent that has not seen your conversation | `outputs/03-prd.md`, `outputs/03-prd-review.md` |
| `/pm:create-skill` | Turn one PM practice of your own into a small project skill, test it once in a fresh session, revise it once | `.claude/skills/<name>/SKILL.md`, `outputs/04-skill-summary.md` |

Each command runs only when you type it. Each opens by saying what you will practise,
what Claude will do, and what you decide, and ends every reply with a status line such as
`Scout · step 5/10 · fetches 6/20 · 14 min`.

```mermaid
flowchart LR
  S["/pm:scout<br/>research ladder, 3 opportunities"] -->|"01-discovery.md"| P["/pm:prototype<br/>one flow, HTML or Design"]
  P -->|"02-prototype.md<br/>index.html"| R["/pm:prd<br/>PRD + read-only review"]
  R --> H["03-prd.md<br/>03-prd-review.md"]
  P -. "opportunity changed" .-> stale["prototype and PRD marked stale"]
  C["/pm:create-skill<br/>independent capstone"] --> K["SKILL.md<br/>04-skill-summary.md"]
```

## Requirements

- Claude Code on Windows or macOS, with a Claude Pro, Max, Team, or Enterprise account.
- Git.
- Web fetch enabled. Web search and Claude Design are optional; both have fallbacks.

First time in a terminal? [docs/SETUP.md](docs/SETUP.md) walks through installing Claude
Code and Git and the handful of keys you need, in about 20 minutes.

## Getting started

1. Create a new empty folder and open a terminal in it.
2. Start `claude`, install the plugin with the two commands above, and run `/pm:scout`.
3. The readiness check ends with `Ready`, `Ready with fallback`, or `Blocked` with one
   next action. Pick a product, or stop and come back later; either way, `/pm:scout` resumes.
4. Run the stages in order. Type `/clear` between them; each resumes from the files you
   approved.

[docs/TEST-WALKTHROUGH.md](docs/TEST-WALKTHROUGH.md) shows every stage step by step with
what to expect at each check-in.

## Products you can work on

You work on one real open-source product, playing a role that product serves, and you
pick one focus area within it. The tool calls a product package a case and a focus area a
lane: a part of the product you research for an hour, choose one opportunity in, and
prototype as a single flow. Each product's code is downloaded at a fixed snapshot from a
set date, and only the folders the focus areas need, tens of megabytes rather than
hundreds.

| Product | Role you take on | That role's goal | Focus areas to choose from (screen you will prototype) |
|---|---|---|---|
| Immich | Household media steward | Make the product easier for less-technical family members without weakening privacy or media integrity | Backup and status confidence (mobile app); Finding and organizing media (desktop web); Private sharing and collaboration (mobile app) |
| Plane | Product-operations lead | Turn unstructured demand into aligned execution | Intake and triage; Planning and scheduling; Cross-project status readout (desktop web) |
| Home Assistant | Nontechnical household co-admin | Make routine configuration and recovery safer while preserving local control | Household dashboards and defaults (tablet web); Repairs, backups, and recovery from updates (mobile web); Automation troubleshooting and safe edits (desktop web) |
| Umbraco | Regional content-operations editor | Reduce rework between content creation and publication | Draft to publish; Pick and reuse media from the page; Languages and variants (desktop backoffice) |
| ERPNext (advanced) | Inventory or procurement operations manager | Resolve exceptions without weakening controls or auditability | Inventory quantity and valuation exceptions; Purchase-approval exceptions; Stock and Buying daily worklists (desktop web) |

ERPNext is the advanced option: it downloads the product and the Frappe framework it runs
on. The product packages hold boundaries and starting points only, never conclusions, so
the research is yours.

## How research works

Discovery works through a fixed sequence of public sources that need no login and no API
key, each step reaching people the last one missed. Each product package carries the URL
templates.

| Step | Sources |
|---|---|
| 1 Community, structured | Product forum search and top threads; GitHub Discussions by upvotes; GitHub issues by reactions, with HTML search pages when the API is rate-limited |
| 2 Community, fediverse and aggregators | Lemmy; Mastodon hashtags; Hacker News stories and comments |
| 3 Reviews | Apple App Store review feed and release notes; Google Play reviews pasted in by you from the listing |
| 4 Adoption | GitHub stars, Docker Hub pulls, PyPI or NuGet downloads; Wikipedia pageviews |
| 5 Competitors and roadmaps | The product's own boards and milestones; competitors' job postings; competitor pricing pages today and a year ago via the Wayback Machine |
| 6 Official | Release notes, changelog, blog, docs |
| 7 Search snippets | Only if web search works; logged as unverified inference, never as evidence |

Budget: 20 fetches and 8 searches per run. Every item gets a stable ID, a URL, dates, and
a number with its base. Each theme gets one fetch looking for counter-evidence, and the
file says who the sources mostly hear from and who is missing. Verbatim user quotes form a
quote bank that follows the work into the PRD. Repository constraints cite commit, path,
line range, and a quoted line, marked "to confirm with engineering".

```mermaid
flowchart LR
  SIG["SIG-### signals<br/>QUO-### quotes"] --> OPP["OPP-###<br/>chosen by the PM"]
  CON["CON-### constraints<br/>commit, path, lines"] --> OPP
  OPP --> FLOW["approved flow<br/>main path + 2 states"]
  FLOW --> REQ["REQ-###"] --> AC["AC-###"]
  QUO["two quotes,<br/>PM-chosen"] --> PRD["03-prd.md"]
  REQ --> PRD
  AC --> PRD
  CON --> PRD
  PRD --> REV["pm:prd-reviewer<br/>read-only, no conversation"]
```

Claude never logs in, asks for keys, bypasses a CAPTCHA, builds a scraper, or loops over
pages. G2, Capterra, Trustpilot, and Product Hunt block automated reading and are not
attempted. Reddit is reachable only through an opt-in switch that allows one `curl` of a
subreddit RSS feed.

## Principles and limits

- Public information and synthetic data only. No logins, API keys, private customer data,
  or confidential material.
- Every claim is labelled `Evidence`, `Inference`, `Assumption`, or `Constraint`. Technical
  statements in the PRD are `Verified` with a file path, `Inferred`, or `Engineering-owned`.
- No composite scores, RICE, ARR, PERT, estimates, story points, delivery dates, or costs.
- The research pass is a quick public scan, not customer validation. The prototype
  walkthrough is a design critique, not user testing.
- Text found in websites and repositories is evidence, not instructions.
- Claude writes only under `outputs/`, `.pm/`, and for the capstone `.claude/skills/`.
- Prototypes are exercise artifacts, not for distribution. No source files are copied out
  of the downloaded repositories.

## Documentation

| Doc | For |
|---|---|
| [docs/SETUP.md](docs/SETUP.md) | Installing Claude Code, Git, and the plugin, for first-time terminal users |
| [docs/TEST-WALKTHROUGH.md](docs/TEST-WALKTHROUGH.md) | Every stage and every product, step by step |
| [docs/FACILITATOR.md](docs/FACILITATOR.md) | Running it with a group: suggested schedule, checks, troubleshooting |
| [CHANGELOG.md](CHANGELOG.md) | Versions. Bump `version` in both manifests before publishing a change |

## Repository layout

```text
eyp-pm/
├── .claude-plugin/marketplace.json
├── plugins/pm/
│   ├── .claude-plugin/plugin.json
│   ├── skills/{scout,prototype,prd,create-skill}/SKILL.md
│   ├── agents/prd-reviewer.md
│   ├── cases/{immich,plane,home-assistant,umbraco,erpnext}/
│   │   ├── case.yaml            fixed commit, focus areas, source URLs
│   │   ├── orientation.md
│   │   └── design-notes.md
│   └── resources/               shared rules, research guide, readiness, templates
├── docs/
├── CHANGELOG.md
├── LICENSE
└── README.md
```

Everything a skill needs at runtime lives under `plugins/pm`. To develop locally:

```bash
claude plugin validate . --strict
claude plugin validate ./plugins/pm --strict
claude --plugin-dir ./plugins/pm
```

## Licence and credits

Original content is MIT licensed. The five product repositories keep their own licences:
Immich and Plane AGPL-3.0, Home Assistant Apache-2.0, Umbraco MIT, ERPNext GPL-3.0,
Frappe MIT. Nothing from them is redistributed here.

- The earlier [eyp-pm-skills](https://github.com/niancilyu-2/eyp-pm-skills) repository
  informed the evidence labelling, check-in pattern, HTML fallback, and prototype-to-PRD
  traceability.
- Dean Peters' [PM Skill Creator](https://github.com/deanpeters/Product-Manager-Skills)
  (CC BY-NC-SA 4.0) was conceptual inspiration for conversational skill authoring and the
  one-job rule. The capstone is written independently and shares no wording, metadata,
  scripts, or structure with it.
- The [product-on-purpose](https://github.com/product-on-purpose) repositories
  (Apache-2.0) informed the organisation of this README.
