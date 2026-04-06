# Claude Code Custom Commands

Custom slash commands for development workflow automation.

## How to Run

All commands use the `/project:` prefix in Claude Code:

```
/project:command-name
```

Example: `/project:git-smart`, `/project:pr-review`, `/project:plan`

Type `/project:` in Claude Code and autocomplete will show all available commands.

---

## Quick Reference

| Run Command | Agents | Purpose |
|-------------|--------|---------|
| `/project:plan` | 4 | Feature planning before coding |
| `/project:design-to-code` | 3 | Figma/screenshot to pixel-perfect code |
| `/project:git-smart` | 3 | Perfect commit, branch, PR naming |
| `/project:pr-review` | 3 | Code quality, security, UX review |
| `/project:final-check` | 5 | Pre-merge zero-error validation |
| `/project:release` | 4 | Version bump, changelog, release |
| `/project:debug` | 4 | Bug investigation & root cause |
| `/project:performance` | 5 | Speed optimization & profiling |
| `/project:refactor` | 4 | Safe code modernization |
| `/project:build-optimize` | 5 | Build system diagnose/optimize |

**Full Workflow:** `/project:plan` > code > `/project:git-smart` > `/project:pr-review` > fix > `/project:final-check` > `/project:release`

---

## Command Details

### `/plan`

**File:** `plan.md` | **Agents:** 4

Smart feature planning — analyze codebase before writing any code.

Agents:
1. **Project Architecture Scanner** — directory structure, design patterns, data flow
2. **Related Code Finder** — similar features, reusable components, files to modify
3. **Dependency & Impact Analyzer** — what breaks, reverse dependencies, DB migrations
4. **Best Practices Checker** — coding conventions, standards, git workflow

Outputs: detailed implementation plan with files, steps, risks, and improvement suggestions.

```
/plan
```

---

### `/design-to-code`

**File:** `design-to-code.md` | **Agents:** 3

Convert reference designs into pixel-perfect code.

Agents:
1. **Design Token Extraction** — colors, typography, spacing, shadows
2. **Component Structure** — HTML hierarchy, BEM classes, existing component reuse
3. **Responsive Layout** — breakpoint planning, overflow handling

Supports: Figma URLs, screenshots, image files, image URLs.

```
/design-to-code
```

---

### `/git-smart`

**File:** `git-smart.md` | **Agents:** 3

Analyze changes and generate perfect commit messages, branch names, PR titles, and PR descriptions.

Agents:
1. **Change Analyzer** — deep diff analysis, categorize changes by purpose
2. **Project Convention Scanner** — learn commit/branch/PR patterns from git history
3. **Impact & Context Summarizer** — user-facing changes, breaking changes, review context

Outputs: 3 branch name options, commit message(s), 3 PR title options, full PR description with testing checklist. Can auto-create commit and PR with approval.

```
/git-smart
```

---

### `/pr-review`

**File:** `pr-review.md` | **Agents:** 3

Comprehensive PR review on the current branch.

Agents:
1. **Code Quality** — duplication, dead code, naming, coding patterns
2. **Security** — nonce verification, XSS, SQL injection, CSRF, capability checks
3. **UX** — accessibility, responsive design, i18n, edge cases, UI consistency

Auto-detects changes vs `trunk`.

```
/pr-review
```

---

### `/final-check`

**File:** `final-check.md` | **Agents:** 5

Pre-deploy validation — ensures zero errors before merge.

Agents:
1. **PHP Syntax & Errors** — syntax check, escaping, sanitization, nonce/capability checks
2. **SCSS/CSS Validation** — syntax, hardcoded colors, RTL, z-index conflicts
3. **JavaScript Validation** — syntax, console.log leftovers, browser compatibility
4. **Template Integrity** — balanced tags, i18n strings, HTML escaping, ABSPATH guards
5. **Build & Integration** — compiled files up to date, merge conflicts, debug code, secrets

Outputs: PASS / FAIL / WARNINGS report.

```
/final-check
```

---

### `/release`

**File:** `release.md` | **Agents:** 4

Manage the full release process — version bumps, changelog, tagging.

Agents:
1. **Changelog Generator** — categorize commits into features, fixes, improvements
2. **Version File Scanner** — find all files with version numbers to update
3. **Pre-release Validator** — debug code, conflicts, build status, translation
4. **Release Notes Writer** — professional notes for GitHub/WordPress.org

Supports: major, minor, patch, pre-release. WordPress.org + GitHub.

```
/release
```

---

### `/debug`

**File:** `debug.md` | **Agents:** 4

Bug investigation — trace errors to root cause.

Agents:
1. **Error Trace Analyzer** — parse stack trace, read error location, check recent changes
2. **Related Code Inspector** — trace upstream/downstream dependencies, data sources
3. **Environment Checker** — PHP version, WordPress version, plugin conflicts, DB state
4. **Git Blame Detective** — find the commit/PR that introduced the bug

Outputs: root cause report with evidence, recommended fix, and prevention steps.

```
/debug
```

---

### `/performance`

**File:** `performance.md` | **Agents:** 5

Speed optimization and performance profiling.

Agents:
1. **PHP/Backend Analyzer** — DB queries, N+1 problems, caching, bottlenecks
2. **Frontend Analyzer** — bundle sizes, render-blocking scripts, images, DOM complexity
3. **Database Query Profiler** — slow queries, redundant queries, missing indexes
4. **Caching Strategy** — transients, object cache, fragment cache, HTTP cache
5. **AJAX & API Performance** — response time, payload size, request batching

Outputs: performance report with bottleneck ranking, quick wins, and before/after metrics.

```
/performance
```

---

### `/refactor`

**File:** `refactor.md` | **Agents:** 4

Safe code modernization — detect smells and refactor without breaking things.

Agents:
1. **Code Smell Detector** — duplication, long methods, god classes, dead code, deep nesting
2. **Pattern Analyzer** — existing conventions, design patterns, naming standards
3. **Dependency Mapper** — class dependencies, hook connections, external consumers
4. **Improvement Scanner** — PHP modernization, WordPress modern APIs, early returns

Outputs: refactoring plan with safety assessment, then executes with step-by-step verification.

```
/refactor
```

---

### `/build-optimize`

**File:** `build-optimize.md` | **Agents:** 5

Build system specialist — diagnose, optimize, or set up build pipelines.

Works with any tech stack — Webpack, Vite, Gulp, Rollup, WordPress, React, Vue, Next.js, or new projects from scratch.

Agents:
1. **Bundler Config Auditor** — loader chains, plugins, entry points, optimizations
2. **Bundle Size Analyzer** — largest files, duplicate deps, tree shaking, vendor splitting
3. **Dependency Checker** — outdated packages, version conflicts, security
4. **CSS Pipeline Analyzer** — compilation chain, RTL, source maps, unused imports
5. **Production Build & Distribution** — archive integrity, source leaks, missing files

```
/build-optimize
```

---

## How to Add New Commands

1. Create a new `.md` file in this folder (`.claude/commands/`)
2. Write instructions in markdown — Claude Code follows these as a prompt
3. The filename (without `.md`) becomes the slash command name
4. Update this README with the new command documentation

## Sharing

Copy the `.claude/commands/` folder to any project to use these commands there.
