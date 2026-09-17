# Readiness check (first run of `/pm:scout`)

Run this the first time `/pm:scout` starts in a workspace, or whenever `readiness.status`
is `not-run` or `blocked`. It should take under ten minutes. Participants are asked to run
it at least 24 hours before the workshop so problems surface early.

Report only one of three words, then the next action:

| Result | Meaning |
|---|---|
| `Ready` | Everything works, including Claude Design. |
| `Ready with fallback` | Everything works except Claude Design; the HTML path will be used. |
| `Blocked` | Something required is missing. State the single next action. |

## Checks, in order

1. **Plugin version.** Read `${CLAUDE_PLUGIN_ROOT}/.claude-plugin/plugin.json` and say
   "You are running the pm plugin version X." (Facilitators use this to spot stale installs.)
2. **Workspace folder.** Apply the folder checks in `workspace.md`. Create `.claude/skills/`,
   `outputs/prototype/`, `sources/`, and `.pm/`. Write `.pm/session.yaml` with
   `readiness.status: not-run` if it does not exist.
3. **Git.** Run `git --version`. Missing git is `Blocked`; the next action is to install
   Git for Windows or Xcode command-line tools, then rerun.
4. **Open a local HTML file.** Write a tiny file `outputs/readiness.html` (one heading that
   says "Your browser can open workshop prototypes") and open it:
   `start outputs\readiness.html` on Windows, `open outputs/readiness.html` on macOS,
   `xdg-open` on Linux. Ask the participant whether a browser tab opened. Record
   `browser_test: confirmed | unconfirmed`. Unconfirmed is a warning, not a block;
   tell them they can double-click the file in their file manager later. Delete the file
   afterwards.
5. **Public web.** Fetch one public page you can verify, for example
   `https://github.com/immich-app/immich/releases`. If the fetch tool is unavailable or
   denied, record `web: unavailable`. This is `Blocked` for the workshop day unless the
   facilitator has said offline research packs exist; say so.
   While doing this, tell the participant: "Claude Code will ask permission the first time
   it fetches a new website or runs a git command. Choosing the option that stops asking
   for this session will save you many clicks tomorrow."
6. **Claude Design.** Explain in one sentence: Claude Design is an optional way to generate
   the prototype visually; it needs a Pro, Max, Team, or Enterprise plan, and on Enterprise
   an administrator must switch it on. Ask the participant to type `/design` on its own
   line and tell you what happened: a design tool started (`available`), an "unknown
   command" message appeared (`unavailable`), or they are unsure (`unknown`). Record it.
   `unavailable` or `unknown` means `Ready with fallback`, never `Blocked`.
7. **Pre-select a case (optional).** Show the five cases in one short table (name, persona,
   one-line mandate). Offer to pick one now so its repository can be downloaded today;
   they may change their mind on the day. If they pick, run the clone procedure in
   `workspace.md` and record `case.preselected: true`. If they decline, skip.
8. **Save.** Write `readiness` into `session.yaml` with `checked_at`, set
   `stage.current: setup`, and print the result line.

## After a successful check

Ask: "Stop here and continue on workshop day, or go straight into Discovery now?"

- **Stop:** set `readiness.stopped_after_setup: true`. Tell them: on workshop day, open a
  terminal in this same folder, start a fresh `claude` session, and run `/pm:scout`. It
  will resume at case selection.
- **Continue:** proceed to case selection in `/pm:scout`.

## Rerunning readiness

If `/pm:scout` starts and `readiness.status` is `ready` or `ready-with-fallback`, do not
repeat the checks. Print "Readiness: <status> on <date>" and continue. Repeat only when the
participant asks or when a check obviously fails during the stage (for example, web fetch
now denied).
