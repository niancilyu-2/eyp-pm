# Test walkthrough for non-technical testers

This is a step-by-step script for trying the whole workshop flow on your own laptop. You do
not need to know how to code. You will type a handful of commands, answer questions in
plain English, and look at the files that appear. Plan for about three hours if you do
every stage, or 45 minutes for setup plus Discovery only.

Everything Claude does is recorded in files you can open with any text editor. If anything
looks wrong, note the step number and what you saw; that is the most useful feedback.

## 1. What you need

Never used Claude Code? Do [SETUP.md](SETUP.md) first; it covers installing, signing in,
and the handful of keys you need. Then come back here.

- A laptop running Windows 10 or 11, or macOS.
- Claude Code installed and signed in. Your plan must allow web fetching. Web search and
  Claude Design are optional.
- Git. On Windows install Git for Windows. On macOS open Terminal, type
  `git --version`, and follow the prompt if it offers to install tools.
- An internet connection that can reach github.com.

## 2. Create the workshop folder and start Claude Code

1. Create a new empty folder, for example `pm-workshop` on your Desktop. Do not reuse an
   existing project folder.
2. Open a terminal in that folder.
   - Windows: right-click the folder and choose *Open in Terminal*.
   - macOS: right-click the folder and choose *New Terminal at Folder*.
3. Type `claude` and press Enter. Claude Code starts inside the folder.

## 3. Install the plugin

Type these two lines, one at a time, inside Claude Code:

```text
/plugin marketplace add niancilyu-2/eyp-pm
/plugin install pm@eyp-pm
```

Then type `/` and check that you can see `/pm:scout`, `/pm:prototype`, `/pm:prd`, and
`/pm:create-skill` in the list. If not, see section 10.

## 4. Readiness check (about 10 minutes)

Type `/pm:scout`. The first run is a readiness check. Claude will:

1. Tell you which plugin version you have.
2. Confirm the folder is empty and that it can write to it.
3. Check Git.
4. Try to open a small test page in your browser. Say whether a tab opened.
5. Fetch one public page and try one web search. If search fails with a policy error,
   that is a warning, not a stop. Copy the error into your notes.
6. Ask you to type `/design` on its own line and report what happened. A design tool
   starting means Claude Design is available. An "unknown command" message means it is
   not, and the HTML path will be used. Both are fine.
7. Offer to pick your case now so its repository downloads today. Say yes if you already
   know which one you want.
8. Print one of `Ready`, `Ready with fallback`, or `Blocked`, followed by the next action.

When asked, choose **stop after setup** if you want to test on a different day, or
**continue** to go straight into Discovery.

From this point on, every reply from Claude ends with a short status line such as
`Scout · step 5/10 · fetches 6/16 · searches 0/8 · items 5 · quotes 3/8 · 14 min · sources clean`.
If that line ever says `sources CHANGED`, stop and note it.

## 5. Choose a case and a lane

There are five products and three lanes each, fifteen combinations. Pick any one. Core
cases are the easier choice. ERPNext downloads two repositories and is the advanced case.

| Case | You play | Lane | Screen you will prototype | Download |
|---|---|---|---|---|
| **Immich** (core) | Household media steward | Backup and status confidence | Phone app, 390×844 | one repo, about 30 MB |
| | | Finding and organizing media | Desktop web, 1440×900 | |
| | | Private sharing and collaboration | Phone app, 390×844 | |
| **Plane** (core) | Product-operations lead | Intake and triage | Desktop web, 1440×900 | one repo, about 20 MB |
| | | Planning-to-execution visibility | Desktop web, 1440×900 | |
| | | Stakeholder progress | Desktop web, 1440×900 | |
| **Home Assistant** (core) | Nontechnical household co-admin | Dashboards | Tablet web, 1024×768 | one repo, under 60 MB; a second is read online only |
| | | Device and entity health and recovery | Phone web, 390×844 | |
| | | Automation creation and troubleshooting | Desktop web, 1440×900 | |
| **Umbraco** (core) | Regional content-operations editor | Draft to publish | Desktop backoffice, 1440×900 | one repo, under 60 MB |
| | | Media reuse | Desktop backoffice, 1440×900 | |
| | | Validation and localization | Desktop backoffice, 1440×900 | |
| **ERPNext** (advanced) | Inventory or procurement operations manager | Inventory exceptions | Desktop web, 1440×900 | two repos, under 60 MB each |
| | | Purchase-approval exceptions | Desktop web, 1440×900 | |
| | | Role-based daily navigation | Desktop web, 1440×900 | |

What differs between cases during research:

- Immich, Home Assistant, Umbraco, and ERPNext have a public forum Claude can search
  directly. Plane does not; its community is on Discord, which cannot be read.
- Immich, Plane, and Home Assistant have an official phone app, so App Store reviews are
  part of the research. Umbraco and ERPNext do not.
- ERPNext has no GitHub Discussions board, so Claude reads issues from two repositories
  instead.

If you want the fullest research demo, choose Immich or Home Assistant. If you want the
quickest run, choose Plane with the Intake and triage lane.

## 6. Discovery with `/pm:scout` (about 60 minutes)

Claude asks two questions (case, then lane), gives a short tour of the product, and
downloads the repository. Then it asks for two or three search words for your lane and
starts the research pass. You will see it move through sources in order: the product
forum, GitHub discussions ranked by upvotes, GitHub issues ranked by reactions, Lemmy,
Mastodon, Hacker News, App Store reviews, adoption numbers, competitor pages compared with
their copy from a year ago, and official release notes. As it goes it shows one number or
quote per source and collects verbatim user quotes.

Two check-ins need your answers:

1. **Research check.** Claude shows coverage, a triangulation table, the two strongest
   quotes, contradictions, and gaps. You say continue, ask for one more targeted fetch, or
   narrow the framing.
2. **Priority check.** Claude proposes three opportunities in Now, Next, Later order, with
   a quote under each. You choose the Now item and give your reason in your own words.
   Your choice wins even if it differs from Claude's.

What to look for:

- Every claim is labelled Evidence, Inference, Assumption, or Constraint.
- Every Evidence item has a link and dates. Quotes name a role, not a username.
- Claude's recommendation and your decision are shown as two separate blocks.

Files that should now exist: `outputs/01-discovery.md` and `.pm/session.yaml`. Open the
first one and skim sections 2, 3b, 5, and 9.

Type `/clear` before the next stage.

## 7. Prototype with `/pm:prototype` (about 60 minutes)

Claude reads your discovery file, shows the quotes behind your chosen opportunity, and
asks short questions: one actor, one job, where the task starts and ends, the main action,
and two alternate states such as empty or error. Then the **flow check-in**: you approve
or edit the flow.

Claude reads the product's real design files, writes a design brief, and picks a path:

- **Claude Design available.** Claude prints one instruction: type `/design`, paste the
  brief, ask Design to export a single HTML file to `outputs/prototype/index.html`, then
  run `/pm:prototype` again. Do exactly that.
- **Not available.** Claude writes the HTML file itself and opens it.

Either way, double-click `outputs/prototype/index.html` in your file manager. Click through
the main path and use the small demo buttons to see the two alternate states. Try the Tab
key to move between controls.

Claude then walks the flow with you, lists findings, and proposes one change tied to
evidence. The **prototype check-in** asks you to approve the revised version and note any
trade-offs.

Files: `outputs/02-prototype.md` and `outputs/prototype/index.html`. Type `/clear`.

## 8. PRD with `/pm:prd` (about 45 minutes)

Claude reads the earlier files, looks at the real code for likely touchpoints, and drafts
the PRD. At the **scope check-in** you approve goals, non-goals, behaviour, alternate
states, and acceptance criteria, and pick two of three customer quotes for the problem
statement.

Claude then sends the draft to a separate read-only reviewer. It cannot see your
conversation and cannot change anything. It returns findings. At the **handoff check-in**
you go through each finding with Claude: fix it in the PRD, turn it into a nonblocking
engineering question, or accept it with a note. A blocking finding must be resolved.

What to look for:

- Technical statements are labelled Verified (with a file path), Inferred, or
  Engineering-owned.
- No estimates, dates, story points, or costs anywhere.
- The status at the top reads `ready-for-engineering-review`.

Files: `outputs/03-prd.md` and `outputs/03-prd-review.md`. Type `/clear`.

## 9. Capstone with `/pm:create-skill` (about 30 minutes)

This one is independent. You can run it without doing the other stages. Bring one
repeated task from your own job, for example turning meeting notes into a decision log,
or paste a checklist you already use.

Claude asks four questions one at a time, then shows a proposed skill contract at the
**scope check-in**. After you approve, it writes one file at
`.claude/skills/<name>/SKILL.md` and validates it.

To test it, open a second terminal window in the same folder, type `claude`, then type
`/skills` and check your new command is listed. Run it, for example
`/meeting-recap` followed by some sample notes. Copy the result and paste it back into the
first window. Claude reviews it, proposes at most one revision, and asks you to state what
the skill automates and what remains your judgment.

Files: `.claude/skills/<name>/SKILL.md` and `outputs/04-skill-summary.md`.

## 10. If something goes wrong

| What you see | What to do |
|---|---|
| The `/pm:` commands are missing | Type `/plugin marketplace update eyp-pm` and then `/plugin install pm@eyp-pm` again. |
| Claude keeps asking permission for websites or git | Choose the option that stops asking for this session. |
| Readiness says `Blocked` | Do the single next action it prints, then type `/pm:scout` again. |
| Web search gives a policy error | Continue. Research works without it. Send the error text to whoever manages your Claude account. |
| A source returns 403 or 429 | Normal on some networks. Claude logs a gap and moves on. |
| Download fails on Windows with a path-too-long message | Type `git config --global core.longpaths true` in a terminal and run `/pm:scout` again. |
| `/design` does nothing | Fine. Claude uses the HTML path. |
| The status line says `sources CHANGED` | Stop and note the step. Do not try to fix it. |
| You lost your place | Type `/clear` and run the current stage command again. It resumes from the saved files. |
| Your capstone skill is not in `/skills` | It must be tested from a new session in the same folder. Check that the folder name matches the `name` line in the file. |

## 11. What to report back

For each stage, three lines are enough:

1. Did you finish, and how long did it take?
2. One thing Claude did that helped, and one decision you kept for yourself.
3. Anything confusing, slow, or wrong, with the step number.

Also send the five files from `outputs/` if you are comfortable sharing them. They contain
only public information and synthetic data.
