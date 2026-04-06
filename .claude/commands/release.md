# Release Manager — Version Bump, Changelog & Release Checklist

Manage the full release process — version bumps, changelog generation, release notes, tagging, and pre-release validation. Works with any project using git.

## Step 0: Load Project Context

Check if `.claude/context.yaml` exists. If yes, read it silently — use it to understand the project's tech stack, conventions, structure, and workflow throughout this command. Do NOT print or summarize it. If the file does not exist, skip this step.

## Step 1: Gather Release Info

Use AskUserQuestion to collect info. Do NOT skip.

### Question Set 1 — Release Type:

1. **Release Type** — "Ki dhoroner release?"
   - Options:
     - Major (breaking changes, big features) — X.0.0
     - Minor (new features, no breaking changes) — x.X.0
     - Patch (bug fixes only) — x.x.X
     - Pre-release (alpha/beta/rc)
   - multiSelect: false

2. **Release From** — "Kon branch theke release hobe?"
   - Options:
     - Current branch
     - development branch
     - trunk/main
     - Custom branch (I'll specify)

3. **Target** — "Release kothay jabe?"
   - Options:
     - WordPress.org (plugin repo)
     - GitHub release only
     - Both WordPress.org + GitHub
     - Internal/staging only

## Step 2: Launch Analysis Agents (Parallel)

Launch **4 agents in parallel**:

### Agent 1: Changelog Generator
Analyze all commits since the last release tag:
- Run `git tag --sort=-version:refname | head -5` to find the last release
- Run `git log <last-tag>..HEAD --oneline --no-merges` to get all commits
- Categorize commits into:
  - **New Features** (feat:, feature/, added, new)
  - **Bug Fixes** (fix:, fix/, fixed, bugfix)
  - **Improvements** (improve, update, enhance, refactor)
  - **Security** (security, vulnerability, sanitize, escape)
  - **Breaking Changes** (breaking, removed, deprecated)
  - **Other** (chore, docs, build, style)
- Filter out noise commits (merge commits, conflict fixes, build-only commits)
- Generate a clean, user-facing changelog in markdown

### Agent 2: Version File Scanner
Find all files that contain version numbers and need updating:
- Check common locations:
  - `package.json` (version field)
  - Main plugin file header (`Version: X.X.X`)
  - `readme.txt` / `README.md` (`Stable tag:`, changelog section)
  - `config.php` or constants file (`DIRECTORIST_VERSION`, `PLUGIN_VERSION`)
  - `composer.json` (if exists)
  - `blocks/package.json` (if exists)
  - Any file with hardcoded version strings
- List all files with current version and what they need to be updated to
- Check for version mismatches (files with different versions)

### Agent 3: Pre-release Validator
Run critical checks before release:
- **No debug code** — grep for `console.log`, `error_log`, `var_dump`, `print_r`, `wp_die('debug`
- **No merge conflict markers** — grep for `<<<<<<<`, `=======`, `>>>>>>>`
- **No TODO/FIXME** in changed files since last release
- **Build is up to date** — check if source files are newer than compiled files
- **Translation file** — is `.pot` file up to date?
- **Readme** — does `readme.txt` have the correct "Tested up to" WordPress version?
- **PHP compatibility** — any PHP 7.x only code that breaks on PHP 8.x or vice versa?
- **No uncommitted changes** — `git status` is clean

### Agent 4: Release Notes Writer
Create professional release notes:
- **Short summary** (1-2 sentences) — what's the headline of this release?
- **Feature highlights** — top 3-5 user-facing changes with descriptions
- **Bug fixes** — list with brief descriptions
- **Security fixes** — if any (without revealing vulnerability details)
- **Breaking changes** — if any, with migration instructions
- **Contributors** — list of unique commit authors since last release
- Format for: GitHub release body, WordPress.org readme changelog, and internal announcement

## Step 3: Release Report

```
## Release Report: vX.X.X

### Current Version: X.X.X → New Version: X.X.X

### Pre-release Checks
- [ ] No debug code
- [ ] No merge conflict markers
- [ ] Build is up to date
- [ ] Translation file updated
- [ ] Readme updated
- [ ] All tests passing
- [ ] No uncommitted changes

### Files to Update
| # | File | Current Version | New Version |
|---|------|----------------|-------------|

### Changelog
#### New Features
- ...

#### Bug Fixes
- ...

#### Improvements
- ...

### Release Notes (for GitHub/WordPress.org)
[Professional formatted release notes]

### Breaking Changes
[None / list with migration steps]
```

## Step 4: Execute Release

Ask: "Shob thik ache? Release proceed korbo?"
- "Yes — bump versions and prepare release"
- "Fix pre-release issues first"
- "Modify changelog/notes"
- "Cancel — not ready yet"

If proceeding:
1. Update all version numbers in identified files
2. Update changelog in readme.txt
3. Create a release commit: `chore: bump version to X.X.X`
4. Suggest creating a git tag: `git tag vX.X.X`
5. If production build needed, remind to run `npm run prod`
6. If WordPress.org, remind about SVN deployment steps

## Important Rules

- NEVER auto-push or auto-tag without explicit user approval
- NEVER skip pre-release validation — even if user says "just release it"
- ALWAYS show the changelog for user review before committing
- ALWAYS check for version mismatches across files
- Semantic versioning: breaking change = major, new feature = minor, fix = patch
- Keep changelog user-facing — developers don't care about "fixed typo in comment"
- Security fixes should be mentioned but not detailed (no exploit instructions)
- If pre-release checks fail, block the release and show what needs fixing
