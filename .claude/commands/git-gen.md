# Git Smart — Perfect Commit, Branch & PR Naming

## Autonomous Execution Rules

- Do NOT ask permission before reading, editing, creating, or deleting files — just do it.
- Do NOT ask "should I proceed?" or "can I modify this?" — act autonomously.
- ONLY use AskUserQuestion when gathering requirements, QA info, or resolving genuine ambiguity about what the user wants.
- Once the user approves a plan, execute ALL steps without per-step confirmation.
- Sub-agents inherit these rules — they must also act without asking file-level permissions.

Analyze your current changes and generate the perfect commit message, branch name, PR title, and PR description. Uses multiple agents to deeply understand what changed and why.

## Step 1: Gather Context

Use AskUserQuestion to collect context. Do NOT skip.

### Question Set 1 — What do you need?

1. **Task** — "What would you like to generate?"
   - Options:
     - Commit message only
     - Branch name only
     - PR title + description only
     - Everything (branch + commit + PR) (Recommended)
   - multiSelect: false

2. **Change Type** — "What type of change is this?"
   - Options:
     - New feature (feat)
     - Bug fix (fix)
     - Improvement/enhancement (improve)
     - Refactor (refactor)
     - Style/UI change (style)
     - Documentation (docs)
     - Build/config change (chore)
     - Security fix (security)
   - multiSelect: false

3. **Ticket/Issue Reference** — "Is there a related issue number or ticket? (e.g., #123, JIRA-456)"
   - Options:
     - Yes (I'll provide it)
     - No ticket/issue
   - If yes, ask for the reference number.

## Step 2: Launch Analysis Agents (Parallel)

Launch **3 agents in parallel**:

### Agent 1: Change Analyzer
Deeply understand what actually changed:
- Run `git status` to see all modified/added/deleted files
- Run `git diff --staged` to see staged changes (if any)
- Run `git diff` to see unstaged changes
- Run `git diff trunk...HEAD` if on a feature branch (to see all branch changes)
- For EACH changed file, understand:
  - What was added/removed/modified
  - WHY it was changed (read surrounding context)
  - What feature/fix this contributes to
- Categorize changes:
  - Core logic changes (the main change)
  - Supporting changes (tests, styles, config)
  - Incidental changes (formatting, imports)

Output: A structured summary of what changed, organized by purpose not by file.

### Agent 2: Project Convention Scanner
Learn the project's naming conventions:
- Run `git log --oneline -30` to see recent commit message patterns
- Run `git branch -a --sort=-committerdate | head -20` to see branch naming patterns
- Check if there's a commit message convention:
  - Conventional commits (feat:, fix:, chore:)
  - Jira-style (PROJ-123: description)
  - Free-form (short description)
  - Imperative mood ("add" not "added")
- Check branch naming:
  - feature/description, fix/description
  - feature/TICKET-description
  - kebab-case vs snake_case
- Check PR patterns:
  - Run `gh pr list --limit 10 --json title,body 2>/dev/null` to see recent PR titles
  - Short titles vs descriptive titles
  - PR body template/format

Output: Detected conventions with examples from the project's history.

### Agent 3: Impact & Context Summarizer
Understand the broader context:
- What user-facing behavior changed?
- What technical changes were made?
- Are there breaking changes?
- Are there related issues or PRs?
- What areas of the codebase were affected?
- Check `git log --oneline` for related recent commits that provide context

Output: A summary suitable for someone reviewing the PR who hasn't seen the code.

## Step 3: Generate Names

After all agents complete, generate the following based on the analysis and project conventions:

### Branch Name
Generate 3 options ranked by best fit:
```
1. [best match to project convention]
2. [alternative]
3. [alternative]
```

Rules:
- Follow the project's existing branch naming convention
- Include the change type prefix (feature/, fix/, improve/, etc.)
- Keep it short but descriptive (max 50 chars)
- Use kebab-case (lowercase, hyphens)
- Include ticket number if provided

### Commit Message(s)
If there's one logical change, generate one commit:
```
[type]: [short description in imperative mood]

[body — what and why, not how]

[footer — ticket reference, breaking changes]
```

If there are multiple logical changes that should be separate commits, suggest splitting:
```
Commit 1: [type]: [description]
Commit 2: [type]: [description]
```

Rules:
- Follow the project's existing commit message convention
- Subject line: imperative mood, max 72 chars, no period
- Body: explain WHAT changed and WHY, not HOW (the diff shows how)
- Reference issues/tickets if provided
- If breaking changes, add `BREAKING CHANGE:` footer

### PR Title
Generate 3 options:
```
1. [best — concise, describes the user-facing change]
2. [alternative]
3. [alternative]
```

Rules:
- Max 70 characters
- Describe the user-facing change, not the implementation
- Include change type prefix if project convention uses it
- Include ticket reference if provided

### PR Description

**IMPORTANT:** Use EXACTLY this template format. Fill in what the agents found. Leave blank what they couldn't determine. NEVER change the template structure.

```markdown
## PR Type
<!-- Mark completed items with an [x] -->
- [ ] Bugfix
- [ ] Security fix
- [ ] Improvement
- [ ] New Feature
- [ ] Refactoring (no functional changes, no api changes)
- [ ] Build related changes
- [ ] Text changes
- [ ] Other... Please describe:

## Summary
<!-- 1-2 sentences: what this PR does and why -->
[Fill from Agent 3 — one-line summary of what this PR delivers and the motivation behind it]

## What Changed

### Function Changes
<!-- Backend logic, PHP handlers, data processing, API changes -->
- [Fill from Agent 1 — each core logic change as a concise bullet]

### UX Changes
<!-- Behavior, interaction, accessibility, responsiveness -->
- [Fill from Agent 3 — each user-facing behavior/interaction change as a concise bullet]

### UI Changes
<!-- Visual, style, CSS, layout changes -->
- [Fill from Agent 1 — each style/visual/layout change as a concise bullet]

## Files Changed
<!-- Group by type, source files only — skip compiled outputs like assets/css/*.css, assets/js/*.js -->

**PHP**
- `path/to/file.php` — [one-line reason]

**JavaScript**
- `path/to/file.js` — [one-line reason]

**SCSS / CSS**
- `path/to/file.scss` — [one-line reason]

## How to Test
1. [Fill from Agent 3 — setup/precondition step]
2. [Fill from Agent 1 — action step that exercises the change]
3. [Fill from Agent 1/3 — expected result to verify]

## Any linked issues
Fixes #[Fill from Step 1 ticket reference, or leave blank]

## Checklist

- [ ] My code follows the [WordPress coding standards](https://make.wordpress.org/core/handbook/best-practices/coding-standards/)

## Additional Context
> Do you want to add any additional context for reviewers? (e.g., screenshots, design references, deployment notes, or anything else worth mentioning)
```

**Template Rules:**
- **PR Type**: Mark the correct checkbox with `[x]` based on the user's Change Type selection from Step 1:
  - Bug fix → `[x] Bugfix`
  - Security fix → `[x] Security fix`
  - Improvement/enhancement → `[x] Improvement`
  - New feature → `[x] New Feature`
  - Refactor → `[x] Refactoring`
  - Build/config → `[x] Build related changes`
  - Style/UI → `[x] Improvement`
  - Documentation → `[x] Text changes`
- **Summary**: 1-2 sentences from Agent 3. First sentence = what changed. Second (optional) = why. Do NOT repeat the PR title.
- **Function Changes**: From Agent 1 core logic changes. Each bullet starts with a verb (Add, Update, Fix, Remove). If none → write "No function changes."
- **UX Changes**: From Agent 3 user-facing behavior analysis. Describe what the user experiences differently. If none → write "No UX changes."
- **UI Changes**: From Agent 1 style/supporting changes. Reference visual outcomes (e.g., "Fix popover alignment on mobile" not "Update CSS"). If none → write "No UI changes."
- **Files Changed**: From Agent 1 git diff. Group by file type (PHP/JS/SCSS/Other). Each file gets a 1-line reason after em-dash. Omit empty groups. Omit compiled build outputs (`assets/css/*.css`, `assets/js/*.js`) — only list source files.
- **How to Test**: 3-5 numbered steps from Agent 3. Setup → action → expected result.
- **Linked Issues**: From Step 1 ticket. If none, keep `Fixes #` with nothing after it.
- **Checklist**: ALWAYS keep as-is. Never modify.
- **General**: Max 1 line per bullet, no sub-bullets. Imperative mood. Keep total PR description under 60 lines. No code snippets in PR body.

## Step 4: User Selection

Present all generated options and ask:

"Which options would you like to use? Let me know if you'd like to edit anything."

Options:
- "Use all as-is"
- "I want to edit some"
- "Regenerate with different focus"

After the user confirms their selections (or uses as-is), **save the final selections to `.claude/git-smart-output.json`** using the Write tool:

```json
{
  "branch_name": "<selected branch name>",
  "commit_message": "<selected full commit message including body>",
  "pr_title": "<selected PR title>",
  "pr_description": "<selected PR description - full markdown>",
  "change_type": "<change type from Step 1>",
  "timestamp": "<current ISO timestamp>"
}
```

This file allows `/ship` to pick up the output even in a different session.

Then ask:

"Would you like me to apply these changes on your behalf? (git add, format, commit, branch, push)"

Options:
- **Yes** — Proceed with the full apply workflow (Step 5 below)
- **No** — Stop here. Output saved to `.claude/git-smart-output.json`. User can run `/ship` later to apply them.

## Step 5: Apply Workflow (only if user said Yes in Step 4)

Execute these steps sequentially. Ask the user at each QA checkpoint.

### 5.1 — Stage all changes
```bash
git add .
```

### 5.2 — Format code (QA checkpoint)
Ask the user: "Should I run yarn format & composer format?"
- If **Yes**: Run `yarn format` first, then `composer format`
- If **No**: Skip to 5.3
- If either command fails, show the error and ask the user how to proceed

### 5.3 — Re-stage after formatting
Run `git status` to check if formatting created new changes.
- If there are new changes: run `git add .`
- If there are unexpected issues: ask the user before proceeding

### 5.4 — Commit
Use the commit message selected/confirmed in Step 4.
```bash
git commit -m "<selected commit message>"
```

### 5.5 — Create branch
Create a new branch using the branch name selected in Step 4:
```bash
git checkout -b <selected-branch-name>
```
If the branch already exists, ask the user whether to switch to it or pick a different name.

### 5.6 — Push to remote (QA checkpoint)
First, list available remotes and remote branches:
```bash
git remote -v
git branch -r
```

Ask the user:
"Which remote branch should I push to? Select from below or provide a custom one."

Show options based on detected remotes (e.g., `origin/<branch-name>`, other remotes if they exist).
Options should include:
- Push to `origin/<current-branch-name>` (default)
- Push to a different remote
- Don't push now

If user selects a remote:
```bash
git push -u <remote> <branch-name>
```

## Important Rules

- ALWAYS analyze actual code changes — never guess from file names alone
- ALWAYS follow the project's existing conventions — don't impose new ones
- ALWAYS use the exact PR description template above — never modify its structure
- NEVER stage files that contain secrets (.env, credentials, API keys)
- NEVER auto-push or create PR without explicit user approval
- Commit messages should be meaningful — not "fix stuff" or "update code"
- One commit should represent one logical change
- If changes are too large for one PR, suggest splitting
- PR descriptions should help reviewers — include context they wouldn't have
- ALWAYS mention breaking changes prominently
- ALWAYS fill what you can from agent analysis, leave blank what you can't determine
