# Claude Code Custom Commands

Type `/` in Claude Code to see all commands.

## Quick Reference

| Command | What it does |
|---------|-------------|
| `/plan` | Feature plan |
| `/design` | Design to code |
| `/git-gen` | Generate commit, branch, PR |
| `/pr-gen` | Generate PR title + description |
| `/ship` | Stage, format, commit, push, PR |
| `/push` | Quick commit & push |
| `/review` | Code review |
| `/check` | Pre-merge validation |
| `/simplify` | Code simplification |
| `/refactor` | Code modernization |
| `/perf` | Performance audit |
| `/debug` | Bug root cause |
| `/build` | Build optimization |
| `/release` | Version bump & changelog |

---

## All Commands

### Planning & Design

| Command | Ki kore |
|---------|---------|
| `/plan` | Codebase analyze kore feature implementation plan banay. Architecture, dependencies, risks — sob dekhay. |
| `/design` | Figma URL, screenshot, ba image theke pixel-perfect code generate kore. Colors, spacing, responsive — sob handle kore. |

### Git & PR Workflow

| Command | Ki kore |
|---------|---------|
| `/git-gen` | Current changes analyze kore commit message, branch name, PR title & description generate kore. Project conventions follow kore. |
| `/pr-gen` | Specific commit hash ba running changes theke PR title + description generate kore export kore. Quick PR description lagle ei ta use koro. |
| `/ship` | `/git-gen` ba `/pr-gen` er output niye stage, format, commit, branch create & push kore dey. Full workflow. |
| `/push` | Quick push — stage, auto commit message, push. No formatting, no PR, no extra steps. Fast commit & push. |

### Code Quality

| Command | Ki kore |
|---------|---------|
| `/review` | Current branch er changes review kore — code quality, security (XSS, SQL injection, nonce), ar UX (a11y, responsive, i18n). |
| `/check` | Merge korar age last validation — PHP syntax, SCSS, JS, template integrity, build status. PASS/FAIL report dey. |
| `/simplify` | Changed code review kore reuse, quality, ar efficiency improve kore. |

### Refactoring & Performance

| Command | Ki kore |
|---------|---------|
| `/refactor` | Code smells detect kore (duplication, dead code, deep nesting) ar safely modernize kore. |
| `/perf` | Speed audit — DB queries, frontend bundle, caching, AJAX performance analyze kore quick wins suggest kore. |
| `/debug` | Bug er root cause trace kore — error logs, git blame, dependencies sob check kore report dey. |

### Build & Release

| Command | Ki kore |
|---------|---------|
| `/build` | Build system (Webpack, Vite, Gulp) diagnose ar optimize kore — bundle size, deps, CSS pipeline. |
| `/release` | Version bump, changelog generate, release notes — full release process manage kore. |

---

## Typical Workflow

```text
/plan      -- Feature plan koro
  code...  -- Code likho
/pr-gen    -- PR description generate koro (specific commits theke)
/review    -- Code review koro
  fix...   -- Review feedback fix koro
/check     -- Last validation
/ship      -- Commit, branch, push koro
/release   -- Release ready hole version bump koro
```

### Quick Scenarios

PR banate chai:

```text
/git-gen   -- Everything generate kore (branch + commit + PR)
```

Specific commits er PR description chai:

```text
/pr-gen    -- Commit hash dile shudhu PR title + description dey
```

Merge korar age check:

```text
/check     -- Sob file validate kore PASS/FAIL report dey
```

Performance slow lagche:

```text
/perf      -- Bottleneck khuje quick wins suggest kore
```

---

## Add Your Own Command

1. `.claude/commands/` folder-e notun `.md` file create koro
2. Markdown-e instructions likho — Claude Code prompt hishebe follow korbe
3. Filename (without `.md`) = command name
4. `/your-command` diye run koro
