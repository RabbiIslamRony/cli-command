# Claude Code Custom Commands

Type `/` in Claude Code to see all commands.

## Quick Reference

| Command | What it does |
|---------|-------------|
| `/context` | Extract & save project context |
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
| `/bugfix` | Full bug fix (investigate + validate + apply) |
| `/debug` | Bug root cause |
| `/build` | Build optimization |
| `/release` | Version bump & changelog |

---

## All Commands

### Context

| Command | What it does |
|---------|---------|
| `/context` | Auto-scans your project to extract tech stack, structure, workflow, and conventions. Generates `CLAUDE.md` at the project root — automatically loaded by Claude Code in every conversation. |

### Planning & Design

| Command | What it does |
|---------|---------|
| `/plan` | Analyzes codebase and creates a feature implementation plan. Covers architecture, dependencies, and risks. |
| `/design` | Generates pixel-perfect code from a Figma URL, screenshot, or image. Handles colors, spacing, responsive layout, and accessibility. |

### Git & PR Workflow

| Command | What it does |
|---------|---------|
| `/git-gen` | Analyzes current changes and generates commit message, branch name, PR title & description. Follows project conventions. |
| `/pr-gen` | Generates PR title + description from specific commit hashes or current changes. Use this when you just need a quick PR description. |
| `/ship` | Takes `/git-gen` or `/pr-gen` output and runs the full workflow — stage, format, commit, create branch & push. |
| `/push` | Quick push — stage, auto-generate commit message, push. No formatting, no PR, no extra steps. |

### Code Quality

| Command | What it does |
|---------|---------|
| `/review` | Reviews current branch changes — code quality, security (XSS, SQL injection, nonce validation), and UX (accessibility, responsive, i18n). |
| `/check` | Final validation before merge — PHP syntax, SCSS, JS, template integrity, build status. Returns a PASS/FAIL report. |
| `/simplify` | Reviews changed code for reuse opportunities, quality improvements, and efficiency gains. |

### Refactoring & Performance

| Command | What it does |
|---------|---------|
| `/refactor` | Detects code smells (duplication, dead code, deep nesting) and safely modernizes the code. |
| `/perf` | Speed audit — analyzes DB queries, frontend bundle, caching, and AJAX performance. Suggests quick wins. |
| `/bugfix` | Full bug fix lifecycle — investigates the issue, validates against security/standards/edge-cases, then applies the fix. |
| `/debug` | Traces bug root cause — checks error logs, git blame, and dependencies. Returns an evidence-based report. |

### Build & Release

| Command | What it does |
|---------|---------|
| `/build` | Diagnoses and optimizes build system (Webpack, Vite, Gulp) — bundle size, dependencies, CSS pipeline. |
| `/release` | Manages the full release process — version bump, changelog generation, and release notes. |

---

## Typical Workflow

```text
/context   ← Extract project context (first time only)
/plan      ← Plan your feature
  code...  ← Write the code
/pr-gen    ← Generate PR description (from specific commits)
/review    ← Review your changes
  fix...   ← Address review feedback
/check     ← Final validation
/ship      ← Commit, branch, push
/release   ← Version bump when ready to release
```

### Quick Scenarios

**Creating a PR:**

```text
/git-gen   ← Generates everything (branch + commit + PR)
```

**PR description from specific commits:**

```text
/pr-gen    ← Give it commit hashes, get PR title + description
```

**Pre-merge check:**

```text
/check     ← Validates all files, returns PASS/FAIL report
```

**Performance feels slow:**

```text
/perf      ← Finds bottlenecks, suggests quick wins
```

---

## Add Your Own Command

1. Create a new `.md` file in `.claude/commands/`
2. Write your instructions in markdown — Claude Code will follow them as a prompt
3. The filename (without `.md`) becomes the command name
4. Run it with `/your-command`
