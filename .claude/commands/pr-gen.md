# PR Export — Generate PR Title & Description from Changes

Analyze specific commits or current running changes and export a PR title and description to `.claude/git-smart-output.json`.

## Step 1: Gather Context

Use AskUserQuestion to collect context. Do NOT skip.

### Questions

1. **Source** — "Kon changes theke PR generate korbo?"
   - Options:
     - Specific commit(s) (I'll provide hash)
     - Running changes (unstaged + staged diff)
     - Last commit on current branch
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

If user selected "Specific commit(s)", ask:

3. **Commit Hashes** — "Commit hash gula deo (comma separated, e.g. abc1234, def5678)"
   - Free text input (use Other option)
   - Options:
     - Let me type the hash(es)
   - multiSelect: false

## Step 2: Collect the Diff

Based on user's source selection:

### If "Specific commit(s)":
```bash
# For each provided commit hash
git show --stat <hash>
git show <hash> -- ':(exclude)assets/css/*' ':(exclude)assets/js/*'
```
Only look at source files (exclude compiled `assets/css/*.css` and `assets/js/*.js`). If multiple commits, analyze each one and combine.

### If "Running changes":
```bash
git status
git diff --staged -- ':(exclude)assets/css/*' ':(exclude)assets/js/*'
git diff -- ':(exclude)assets/css/*' ':(exclude)assets/js/*'
```

### If "Last commit":
```bash
git log -1 --oneline
git show HEAD --stat
git show HEAD -- ':(exclude)assets/css/*' ':(exclude)assets/js/*'
```

## Step 3: Check Project PR Conventions

```bash
gh pr list --limit 5 --json title 2>/dev/null
```

Learn the PR title style from recent PRs.

## Step 4: Generate PR Title & Description

Generate 3 PR title options ranked by best fit.

Then generate the PR description using EXACTLY this template:

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
[1-2 sentences: what this PR does and why]

## What Changed

### Function Changes
- [Each core logic change as a concise bullet]

### UX Changes
- [Each user-facing behavior/interaction change as a concise bullet]

### UI Changes
- [Each style/visual/layout change as a concise bullet]

## Files Changed

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
```

**Template Rules:**
- **PR Type**: Mark the correct checkbox with `[x]` based on user's Change Type selection.
- **Summary**: 1-2 sentences. First sentence = what changed. Second (optional) = why.
- **Function Changes**: Core logic changes. Each bullet starts with a verb. If none: "No function changes."
- **UX Changes**: User-facing behavior changes. If none: "No UX changes."
- **UI Changes**: Visual/style changes. If none: "No UI changes."
- **Files Changed**: Group by file type (PHP/JS/SCSS/Other). Only list source files — skip compiled outputs (`assets/css/*.css`, `assets/js/*.js`). Omit empty groups.
- **How to Test**: 3-5 numbered steps.
- **Linked Issues**: If none, keep `Fixes #` blank.
- **Checklist**: Always keep as-is.
- Max 1 line per bullet, no sub-bullets. Imperative mood. Keep total under 60 lines.

## Step 5: Present & Export

Present the PR title options and full description to the user.

Ask: "Kisu edit korte chaile bolo, nahole export kore dibo."

Options:
- Use as-is
- I want to edit some

After user confirms, save to `.claude/git-smart-output.json`:

```json
{
  "pr_title": "<selected PR title>",
  "pr_description": "<full PR description markdown>",
  "change_type": "<change type>",
  "source_commits": ["<commit hashes if provided, or 'running' or 'HEAD'>"],
  "timestamp": "<current ISO timestamp>"
}
```

Confirm: "PR title and description `.claude/git-smart-output.json`-e save hoise. `/ship` diye apply korte parbe."
