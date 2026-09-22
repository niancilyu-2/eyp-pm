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

- [Overview](#overview)
  - [Where it fits](#where-it-fits)
  - [What it does](#what-it-does)
- [Setup](#setup)
  - [Requirements](#requirements)
  - [Getting started](#getting-started)
- [Using it](#using-it)
  - [Choose a product and a focus area](#choose-a-product-and-a-focus-area)
  - [Principles and limits](#principles-and-limits)
- [Reference](#reference)
  - [Documentation](#documentation)
  - [Repository layout](#repository-layout)
  - [Licence and credits](#licence-and-credits)

## Overview

### Where it fits

A typical product lifecycle runs from vision and strategy through design to execution.
This plugin covers the discovery part of design that can be done from public evidence and
a product's own code, and the delivery PRD that comes out of it. Items marked covered are
the steps it performs.

```text
Vision
 ↓
Strategy
 ↓
Design
 └─ Discovery
     ├─ Customer insights          covered
     ├─ Competitor insights        covered
     ├─ Prototypes                 covered
     ├─ Experiments
     ├─ Feasibility validation     covered
     └─ Delivery PRD               covered
 ↓
Execution
```

What each covered step contains:

- **Customer insights.** Forum, GitHub, and community posts; app reviews and release notes;
  a verbatim quote bank; triangulation and a counter-evidence pass; a note on who the
  sources hear from and who is missing.
- **Competitor insights.** Competitor pricing and feature pages today and a year ago,
  competitors' job postings, the product's own roadmap, and adoption numbers.
- **Prototypes.** A clickable prototype of the chosen flow with two alternate states, a
  design critique, and one revision.
- **Feasibility validation.** Repository constraints with line references, likely code
  touchpoints, a read-only engineering review, and open questions for engineering.
- **Delivery PRD.** Requirements and acceptance criteria with IDs, states and behaviour, an
  engineering appendix, all traceable to the evidence.

Outside its scope: vision and strategy, experiments with real users (A/B tests, fake doors,
pilots), and execution. The customer insights come from public sources rather than
interviews or surveys, so they are a scan, not customer validation, and the PRD is marked
ready for engineering review, not ready to build.

### What it does


Four skills. Each runs only when you type it.

| Skill | What it does | You leave with | Details |
|---|---|---|---|
| `/pm:scout` | Readiness check on first run, then a research pass over a fixed sequence of public sources, repository constraints, three opportunities, and your choice of one | `outputs/01-discovery.md`, the discovery brief | [docs/skills/scout.md](docs/skills/scout.md) |
| `/pm:prototype` | One actor, one job, one flow. A clickable prototype in Claude Design or as a standalone HTML file, a design critique, one revision | `outputs/02-prototype.md`, `outputs/prototype/index.html` | [docs/skills/prototype.md](docs/skills/prototype.md) |
| `/pm:prd` | A delivery PRD grounded in the product's code, reviewed by a separate read-only agent that has not seen your conversation | `outputs/03-prd.md`, `outputs/03-prd-review.md` | [docs/skills/prd.md](docs/skills/prd.md) |
| `/pm:create-skill` | Turn one PM practice of your own into a small project skill, test it once in a fresh session, revise it once | `.claude/skills/<name>/SKILL.md`, `outputs/04-skill-summary.md` | [docs/skills/create-skill.md](docs/skills/create-skill.md) |

Each skill opens by saying what you will practise, what Claude will do, and what you
decide, and ends every reply with a status line such as
`Scout · step 5/10 · fetches 6/20 · 14 min`.

```mermaid
flowchart LR
  S["/pm:scout<br/>public-source research, 3 opportunities"] -->|"01-discovery.md<br/>discovery brief"| P["/pm:prototype<br/>one flow, HTML or Design"]
  P -->|"02-prototype.md<br/>index.html"| R["/pm:prd<br/>PRD + read-only review"]
  R --> H["03-prd.md, delivery PRD<br/>03-prd-review.md"]
  P -. "opportunity changed" .-> stale["prototype and PRD marked stale"]
  C["/pm:create-skill<br/>independent capstone"] --> K["SKILL.md<br/>04-skill-summary.md"]
```

## Setup

### Requirements


- Claude Code on Windows or macOS, with a Claude Pro, Max, Team, or Enterprise account.
- Git.
- Web fetch enabled. Web search and Claude Design are optional; both have fallbacks.

First time in a terminal? [docs/SETUP.md](docs/SETUP.md) walks through installing Claude
Code and Git and the handful of keys you need, in about 20 minutes.

### Getting started


1. Create a new empty folder and open a terminal in it.
2. Start `claude`, install the plugin with the two install lines above, and run `/pm:scout`.
3. The readiness check ends with `Ready`, `Ready with fallback`, or `Blocked` with one
   next action. Pick a product, or stop and come back later; either way, `/pm:scout` resumes.
4. Run the stages in order. Type `/clear` between them; each resumes from the files you
   approved.

[docs/TEST-WALKTHROUGH.md](docs/TEST-WALKTHROUGH.md) shows every stage step by step with
what to expect at each check-in.

## Using it

### Choose a product and a focus area


You work on one real open-source product as its PM, designing for one of the users that
product serves, and you pick one focus area within it. The tool calls a product package a case and a focus area a
lane: a part of the product you research for an hour, choose one opportunity in, and
prototype as a single flow. Each product's code is downloaded at a fixed snapshot from a
set date, and only the folders the focus areas need, tens of megabytes rather than
hundreds.

| Product | User you design for | Your brief on their behalf | Focus areas to choose from (screen you will prototype) |
|---|---|---|---|
| Immich | Household media steward | Make the product easier for less-technical family members without weakening privacy or media integrity | Backup and status confidence (mobile app); Finding and organizing media (desktop web); Private sharing and collaboration (mobile app) |
| Plane | Product-operations lead | Turn unstructured demand into aligned execution | Intake and triage; Planning and scheduling; Cross-project status readout (desktop web) |
| Home Assistant | Nontechnical household co-admin | Make routine configuration and recovery safer while preserving local control | Household dashboards and defaults (tablet web); Repairs, backups, and recovery from updates (mobile web); Automation troubleshooting and safe edits (desktop web) |
| Umbraco | Regional content-operations editor | Reduce rework between content creation and publication | Draft to publish; Pick and reuse media from the page; Languages and variants (desktop backoffice) |
| ERPNext (advanced) | Inventory or procurement operations manager | Resolve exceptions without weakening controls or auditability | Inventory quantity and valuation exceptions; Purchase-approval exceptions; Stock and Buying daily worklists (desktop web) |

ERPNext is the advanced option: it downloads the product and the Frappe framework it runs
on. The product packages hold boundaries and starting points only, never conclusions, so
the research is yours.

### Principles and limits


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

## Reference

### Documentation


| Doc | For |
|---|---|
| [docs/skills/](docs/skills/) | One page per skill: what it produces, how a run goes, the rules it follows |
| [docs/SETUP.md](docs/SETUP.md) | Installing Claude Code, Git, and the plugin, for first-time terminal users |
| [docs/TEST-WALKTHROUGH.md](docs/TEST-WALKTHROUGH.md) | Every stage and every product, step by step |
| [docs/FACILITATOR.md](docs/FACILITATOR.md) | Running it with a group: suggested schedule, checks, troubleshooting |

### Repository layout


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
├── LICENSE
└── README.md
```

Everything a skill needs at runtime lives under `plugins/pm`. To develop locally:

```bash
claude plugin validate . --strict
claude plugin validate ./plugins/pm --strict
claude --plugin-dir ./plugins/pm
```

### Licence and credits


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
