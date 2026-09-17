# Technical setup for participants

Read this before the workshop, or use it on the day if you are setting up for the first
time. It assumes you have never used Claude Code or a terminal. Allow 20 to 30 minutes.
If you get stuck, note the step number and ask the facilitator.

## What you are installing

Claude Code is Claude running inside a text window on your laptop called a terminal. You
type in plain English, Claude replies in the same window, and it can read and write files
in one folder you choose. For this workshop you will type a few commands that start with a
slash, such as `/pm:scout`, and otherwise answer questions in ordinary sentences.

## Step 1. Check your account

Claude Code needs a Claude Pro, Max, Team, or Enterprise plan. The free plan does not
include it. If your company provides Claude, your administrator may have set up a
company sign-in; the facilitator will tell you which to use. You will sign in through your
normal browser in step 4.

## Step 2. Open a terminal

**Windows.** Press the Windows key, type `Terminal`, and open *Terminal* (on older
machines, *Windows PowerShell*). A window opens with a line ending in `>`. If the line
starts with `PS`, you are in PowerShell, which is what you want.

**macOS.** Press Command and Space, type `Terminal`, press Enter. A window opens with a
line ending in `%` or `$`.

You type commands at that line and press Enter to run them. Nothing happens until you
press Enter. To paste, use Ctrl+V on Windows or Command+V on macOS.

## Step 3. Install Claude Code

Copy one line, paste it into the terminal, press Enter, and wait until the prompt returns.

**Windows (PowerShell):**

```powershell
irm https://claude.ai/install.ps1 | iex
```

**macOS:**

```bash
curl -fsSL https://claude.ai/install.sh | bash
```

Then **close the terminal window and open a new one** so it learns where Claude Code
lives. Check the install:

```text
claude --version
```

You should see a version number followed by `(Claude Code)`. Native installs update
themselves in the background, so you will not need to do this again.

If you prefer a package manager: `winget install Anthropic.ClaudeCode` on Windows or
`brew install --cask claude-code` on macOS.

## Step 4. Install Git

Claude uses Git to download the product you will study. Check whether it is already
there:

```text
git --version
```

If you see a version number, skip ahead. Otherwise:

- **Windows.** Download Git for Windows from https://git-scm.com/downloads/win, run the
  installer, and accept every default. Close and reopen the terminal afterwards.
- **macOS.** The check above usually offers to install the tools. Click *Install* and wait.

## Step 5. Sign in

Type `claude` and press Enter. The first time, Claude Code opens your browser to sign in.
Sign in with the account from step 1 and return to the terminal. If the browser does not
open, the terminal shows a link you can copy into a browser yourself.

Claude Code will ask whether you trust the folder you are in. For now say yes; you are
only in your home folder. Type `/exit` to leave. You will not need to sign in again. If
you ever need to switch accounts, type `/login` inside Claude Code.

## Step 6. Learn the ten things you need

| You want to | Do this |
|---|---|
| Send a message | Type it and press Enter |
| Start a new line without sending | Shift+Enter. If that sends instead, type a backslash `\` and then Enter |
| See the commands available | Type `/` and look at the list. Start typing to filter it |
| Stop Claude while it is working | Press Escape |
| Clear the conversation between stages | Type `/clear` |
| Quit Claude Code | Type `/exit` |
| Paste text | Ctrl+V on Windows, Command+V on macOS. A long paste collapses into a small `[Pasted text]` label; that is normal |
| Repeat something you typed earlier | Press the up arrow |
| Answer a question with options | Use the arrow keys and Enter, or type your own answer |
| Answer a permission prompt | See below |

**Permission prompts.** Claude asks before it fetches a website, runs a Git command, or
writes a file for the first time. You will see choices like *Yes*, *Yes, and don't ask
again this session*, and *No*. During the workshop, choose the option that stops asking
for the session. Claude only works inside your workshop folder and never changes the
downloaded product code.

**Do not press Shift+Tab.** It switches permission modes, which is not needed for the
workshop. If the bottom of the screen shows a mode you did not choose, press Shift+Tab
until it returns to the default, or quit and start again.

## Step 7. Create the workshop folder and install the plugin

1. Create a new empty folder, for example `pm-workshop` on your Desktop.
2. Open a terminal in that folder. Windows: right-click the folder and choose *Open in
   Terminal*. macOS: right-click the folder and choose *New Terminal at Folder*. Or in any
   terminal type `cd ` followed by a space, drag the folder onto the window, and press
   Enter.
3. Type `claude` and press Enter. Say yes to trusting the folder.
4. Type these two lines, one at a time:

   ```text
   /plugin marketplace add niancilyu-2/eyp-pm
   /plugin install pm@eyp-pm
   ```

5. Type `/` and confirm you see `/pm:scout` in the list.
6. Type `/pm:scout`. This runs a ten-minute readiness check and ends with `Ready`,
   `Ready with fallback`, or `Blocked`. Both `Ready` results are fine. For `Blocked`, do
   the one action it prints and run `/pm:scout` again.
7. When asked, choose *stop after setup*. You are done until the workshop.

## Pre-session checklist

- [ ] `claude --version` prints a version.
- [ ] `git --version` prints a version.
- [ ] You signed in once and `claude` starts without asking again.
- [ ] A folder called `pm-workshop` exists and is empty apart from what the readiness check created.
- [ ] `/pm:scout` reported `Ready` or `Ready with fallback`.
- [ ] You know how to open a terminal in that folder.
- [ ] Laptop charger packed. The research stage keeps the laptop busy.

## Workshop-day card

Open a terminal in `pm-workshop`, type `claude`, then:

| Stage | Type | Then |
|---|---|---|
| Discovery | `/pm:scout` | answer questions, decide the Now item |
| between stages | `/clear` | |
| Prototype | `/pm:prototype` | approve the flow, click the prototype |
| between stages | `/clear` | |
| PRD | `/pm:prd` | approve scope, settle review findings |
| between stages | `/clear` | |
| Capstone | `/pm:create-skill` | describe one repeated task, test it in a second window |

Every reply ends with a status line such as `Scout · step 5/10 · fetches 6/16 · … · sources clean`.
It tells you where you are.

## If something goes wrong

| What you see | What to do |
|---|---|
| `claude` is not recognized or command not found | Close the terminal, open a new one, try again. If it persists, rerun the install line from step 3 |
| `The token '&&' is not a valid statement separator` | You are in PowerShell. Use the PowerShell install line, not the CMD one |
| `'irm' is not recognized` | You are in CMD, not PowerShell. Open *Terminal* or *Windows PowerShell* instead |
| Browser sign-in never completes | Copy the link from the terminal into a browser where you are signed in to Claude. On a company laptop, ask whether a VPN or proxy blocks claude.ai |
| Sign-in says the plan does not include Claude Code | You are on a free plan or the wrong account. Sign out with `/logout` and sign in with the workshop account |
| `/pm:` commands missing after install | Type `/plugin marketplace update eyp-pm`, then `/plugin install pm@eyp-pm` again |
| Readiness says Git is missing | Do step 4, close and reopen the terminal, run `/pm:scout` again |
| Download fails with a path-too-long message on Windows | Type `git config --global core.longpaths true` in the terminal, then run `/pm:scout` again |
| Web search reports a policy error | Continue. The workshop works without it. Send the error text to whoever manages your Claude account |
| The screen looks stuck | Press Escape once. If nothing changes, type `/exit`, start `claude` again, and rerun the stage command; it resumes from saved files |

Full walkthrough of every stage and case: [TEST-WALKTHROUGH.md](TEST-WALKTHROUGH.md).
