# Create PR — Stage, Commit, Push & Open PR from Fork

Create a pull request from the `rony` fork to `sovware/directorist` trunk with the full team PR template. Handles the complete flow: stage → branch → commit → push → PR.

## Step 1: Gather Context

Use AskUserQuestion to collect context. Do NOT skip.

### Questions

1. **Change Type** — "What type of change is this?"
   - Options:
     - New feature (feat)
     - Bug fix (fix)
     - Improvement/enhancement (improve)
     - Refactor (refactor)
     - Style/UI change (style)
     - Build/config change (chore)
     - Security fix (security)
   - multiSelect: false

2. **Ticket** — "Is there a related issue number or ticket?"
   - Options:
     - Yes (I'll provide it)
     - No ticket/issue
   - multiSelect: false

If user selected "Yes", ask for the issue number.

## Step 2: Analyze Changes

Run in parallel:

```bash
git status
git diff --staged
git diff
git log --oneline -10
```

Understand what changed, categorize by purpose (core logic, supporting, incidental).

## Step 3: Generate Branch Name

Based on change type and analysis, generate a branch name following project conventions:
- Format: `{type}/{kebab-case-description}` (e.g., `fix/range-slider-issue`, `add/auto-pr-review`)
- Types: `fix/`, `add/`, `improve/`, `feature/`, `style/`, `build/`
- All lowercase, hyphens for multi-word

Present 3 options and ask user to select.

## Step 4: Generate Commit Message

Follow project's conventional commit style:
```
{type}: {Short description in imperative mood}

{Body — what and why, not how}
```

Present to user for approval.

## Step 5: Generate PR Title & Description

Generate PR title (max 70 chars, follows project convention like `Fix:`, `Improve:`, `Add:`, `Build:`).

Generate PR description using EXACTLY this template:

```markdown
## PR Type
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

## What Changed

### Function Changes
<!-- Backend logic, PHP handlers, data processing, API changes -->
-

### UX Changes
<!-- Behavior, interaction, accessibility, responsiveness -->
-

### UI Changes
<!-- Visual, style, CSS, layout changes -->
-

## Files Changed
<!-- Group by type, source files only — skip compiled outputs like assets/css/*.css, assets/js/*.js -->

**PHP**
- `path/to/file.php` — [one-line reason]

**JavaScript**
- `path/to/file.js` — [one-line reason]

**SCSS / CSS**
- `path/to/file.scss` — [one-line reason]

## How to Test
1. [Setup/precondition step]
2. [Action step that exercises the change]
3. [Expected result to verify]

## Any linked issues
Fixes #

## Checklist

- [ ] My code follows the [WordPress coding standards](https://make.wordpress.org/core/handbook/best-practices/coding-standards/)

## Additional Context
> Do you want to add any additional context for reviewers?

🤖 Generated with [Claude Code](https://claude.com/claude-code)
```

**Template Rules:**
- **PR Type**: Mark correct checkbox with `[x]` based on change type.
- **Summary**: 1-2 sentences. What changed + why.
- **Function/UX/UI Changes**: Each bullet starts with a verb. Write "No X changes." if none.
- **Files Changed**: Group by type. Source files only — skip compiled `assets/css/*.css`, `assets/js/*.js`. Omit empty groups.
- **How to Test**: 3-5 numbered steps.
- **Checklist**: Always keep as-is.
- Max 1 line per bullet. Imperative mood. Total under 60 lines.

## Step 6: User Approval

Present everything (branch name, commit message, PR title, PR description) and ask:

"Does everything look good? I'll stage, commit, push, and create the PR."

Options:
- Use all as-is
- I want to edit some
- Cancel

## Step 7: Execute (only if user approved)

Run sequentially:

### 7.1 — Stage files
```bash
git add <relevant files>
```
Never stage `.env`, credentials, or secrets.

### 7.2 — Create branch
```bash
git checkout -b <selected-branch-name>
```
If branch exists, ask user whether to switch or pick a different name.

### 7.3 — Commit
```bash
git commit -m "<selected commit message>"
```

### 7.4 — Push to fork
```bash
git push -u rony <branch-name>
```

### 7.5 — Create PR
```bash
gh pr create \
  --repo sovware/directorist \
  --base trunk \
  --head RabbiIslamRony:<branch-name> \
  --title "<PR title>" \
  --body "<PR description>"
```

Use a HEREDOC for the body to preserve formatting.

### 7.6 — Report
Show the PR URL and any next steps.

## Important Rules

- ALWAYS analyze actual code changes — never guess from file names
- ALWAYS use the exact PR template above
- NEVER stage files containing secrets
- NEVER auto-push without user approval
- Push to `rony` remote (fork), PR targets `sovware/directorist` `trunk`
- Include `Co-Authored-By: Claude Opus 4.6 <noreply@anthropic.com>` in commit if Claude helped write the code
