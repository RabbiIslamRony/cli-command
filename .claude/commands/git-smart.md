# Git Smart — Perfect Commit, Branch & PR Naming

Analyze your current changes and generate the perfect commit message, branch name, PR title, and PR description. Uses multiple agents to deeply understand what changed and why.

## Step 1: Gather Context

Use AskUserQuestion to collect context. Do NOT skip.

### Question Set 1 — What do you need?

1. **Task** — "Ki generate korte chao?"
   - Options:
     - Commit message only
     - Branch name only
     - PR title + description only
     - Everything (branch + commit + PR) (Recommended)
   - multiSelect: false

2. **Change Type** — "Ei change ta ki dhoroner?"
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

3. **Ticket/Issue Reference** — "Kono issue number ba ticket ache? (e.g., #123, JIRA-456)"
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
What kind of change does this PR introduce?
<!-- Mark completed items with an [x] based on the Change Type from Step 1 -->
- [ ] Bugfix
- [ ] Security fix
- [ ] Improvement
- [ ] New Feature
- [ ] Refactoring (no functional changes, no api changes)
- [ ] Build related changes
- [ ] Text changes
- [ ] Other... Please describe:

## Description
How to reproduce the issue or how to test the changes

1. [Fill from Agent 3 impact analysis — what to test]
2. [Fill from Agent 1 change analysis — specific steps]
3. [Fill from Agent 1 — expected result]

## Any linked issues
Fixes #[Fill from Step 1 ticket reference, or leave blank]

## Checklist

- [ ] My code follows the [WordPress coding standards](https://make.wordpress.org/core/handbook/best-practices/coding-standards/)
```

**Template Rules:**
- Mark the correct PR Type checkbox with `[x]` based on the user's Change Type selection from Step 1:
  - Bug fix → `[x] Bugfix`
  - Security fix → `[x] Security fix`
  - Improvement/enhancement → `[x] Improvement`
  - New feature → `[x] New Feature`
  - Refactor → `[x] Refactoring`
  - Build/config → `[x] Build related changes`
  - Style/UI → `[x] Improvement`
  - Documentation → `[x] Text changes`
- Fill the Description steps from what agents discovered about the changes
- If agents found specific test steps, use those. Otherwise write clear steps based on what changed.
- If no linked issue, keep `Fixes #` with nothing after it (leave blank)
- ALWAYS keep the Checklist section as-is

## Step 4: User Selection

Present all generated options and ask:

"Kon gula use korbe? Kisu edit korte chaile bolo."

Options:
- "Use all as-is"
- "I want to edit some"
- "Regenerate with different focus"
- "Apply and create commit/PR now"

If "Apply and create commit/PR now":
1. Stage changes: `git add <relevant files>` (NOT `git add .`)
2. Create commit with the selected message
3. Push branch if needed
4. Create PR with `gh pr create` using selected title and description
5. Return the PR URL

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
