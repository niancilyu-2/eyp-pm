# `.pm/session.yaml` schema

The PM never edits this file. Skills own it. Keep it flat, small, and valid YAML.
Before every write: read the whole file, change only the keys you own, keep `decisions`
append-only, then write the whole file back. If the file is unreadable, do not guess: show
the PM the error, offer to rebuild it from the approved output files, and record
that you did so under `notes`.

```yaml
schema: 1
plugin_version: "0.1.0"          # from plugin.json at the time of setup
created_at: "2026-10-01T09:12:00Z"
workspace: "/absolute/path/to/workspace"

readiness:
  status: ready                  # ready | ready-with-fallback | blocked | not-run
  checked_at: "2026-10-01T09:14:00Z"
  git: ok                        # ok | missing
  web: ok                        # ok | unavailable            (fetch tool)
  search: ok                     # ok | unavailable            (web search tool)
  feeds: [fetch, search]         # which of the two worked
  reddit_rss: false              # opt-in; true only if the PM asked for Reddit RSS
  browser_test: confirmed        # confirmed | unconfirmed
  design: unknown                # available | unavailable | unknown  (PM typed /design)
  stopped_after_setup: true      # PM chose to stop after readiness
  notes: []

case:
  id: immich                     # null until chosen
  lane: backup-status-confidence # null until chosen
  chosen_at: "2026-10-01T09:20:00Z"
  preselected: true              # chosen during readiness; may be changed on the day

sources:
  - role: primary                # primary | companion
    owner_repo: immich-app/immich
    clone_to: sources/primary
    pinned_sha: 9994eb3bedc77a63e652cb4195d843433696d036
    verified_sha: 9994eb3bedc77a63e652cb4195d843433696d036   # from git rev-parse HEAD after clone
    method: sparse-clone         # sparse-clone | offline-copy
    clean_at_start: true         # git status --porcelain empty at the start of the last stage
    clean_at_end: true

stage:
  current: discovery             # setup | discovery | prototype | prd | done
  status: in-progress            # not-started | in-progress | approved | stale

stages:
  discovery:
    status: approved             # not-started | in-progress | approved | stale
    output: outputs/01-discovery.md
    revision: 1                  # increments on each approved save
    approved_at: "2026-10-01T10:15:00Z"
    selected_opportunity: OPP-002
    provisional: false           # true when saved under a time cut or with weak coverage
  prototype:
    status: not-started          # not-started | in-progress | awaiting-design | approved | stale
    output: outputs/02-prototype.md
    html: outputs/prototype/index.html
    path: null                   # design | html-fallback
    brief: .pm/design-brief.md
    revision: 0
    approved_at: null
  prd:
    status: not-started
    output: outputs/03-prd.md
    review: outputs/03-prd-review.md
    revision: 0
    approved_at: null
  capstone:                      # informational only; /pm:create-skill does NOT write this file
    note: "See outputs/04-skill-summary.md if present."

stale: []                        # e.g. [prototype, prd]

decisions:                       # append-only
  - id: D-001
    stage: discovery
    checkin: research-check      # research-check | priority-check | flow-check | prototype-check | scope-check | handoff-check
    claude_recommended: "Coverage is thin on mobile backup; continue with framing X"
    pm_decided: "Continue; accept the gap"
    reason: "Gap is documented; enough to prototype"
    at: "2026-10-01T09:50:00Z"

notes: []                        # free-text events worth remembering, newest last
```

## Rules of ownership

| Key | Written by |
|---|---|
| `schema`, `plugin_version`, `created_at`, `workspace`, `readiness`, `case`, `sources` | `/pm:scout` |
| `stage`, `stale` | any stage skill that changes the current stage |
| `stages.discovery` | `/pm:scout` |
| `stages.prototype`, `readiness.design` | `/pm:prototype` |
| `stages.prd` | `/pm:prd` |
| `decisions`, `notes` | any skill (append only) |
| anything | never `/pm:create-skill` |
