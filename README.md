# Claude Code Custom Commands

A collection of powerful AI-powered slash commands for [Claude Code](https://docs.anthropic.com/en/docs/claude-code). Boost your development workflow with smart planning, code review, git automation, debugging, and more.

## Installation

Copy the `.claude/commands/` folder into your project:

```sh
# Clone this repo
git clone https://github.com/RabbiIslamRony/cli-command.git

# Copy commands to your project
cp -r cli-command/.claude/commands/ your-project/.claude/commands/
```

Or copy into your home directory to make commands available globally:

```sh
# Global install (available in all projects)
cp -r cli-command/.claude/commands/ ~/.claude/commands/
```

> After copying, type `/` in Claude Code to see all available commands.

## Commands

### Context & Planning

| Command | Description |
|---------|-------------|
| `/context` | Auto-scan project to extract tech stack, structure, and conventions. Saves to `.claude/context.yaml` for other commands to use. |
| `/plan` | Analyze codebase and create a feature implementation plan with architecture, dependencies, and risk assessment. |

### Design

| Command | Description |
|---------|-------------|
| `/design` | Convert Figma URL, screenshot, or image into pixel-perfect code. Handles colors, spacing, responsive layout, and accessibility. |

### Git & PR Workflow

| Command | Description |
|---------|-------------|
| `/git-gen` | Analyze current changes and generate commit message, branch name, PR title & description following project conventions. |
| `/pr-gen` | Generate PR title + description from specific commits. Quick option when you just need a PR description. |
| `/ship` | Full workflow — stage, format, commit, create branch, push, and open PR using generated output. |
| `/push` | Quick commit & push — no formatting, no PR, no extra steps. For fast iterations. |

### Code Quality

| Command | Description |
|---------|-------------|
| `/review` | Review current branch changes — code quality, security (XSS, SQL injection, CSRF), and UX (accessibility, responsive, i18n). |
| `/check` | Pre-merge validation — PHP syntax, SCSS, JS, template integrity, build status. Returns PASS/FAIL report. |
| `/simplify` | Review changed code for reuse opportunities, quality improvements, and efficiency gains. |

### Refactoring & Performance

| Command | Description |
|---------|-------------|
| `/refactor` | Detect code smells (duplication, dead code, deep nesting) and safely modernize with backward compatibility. |
| `/perf` | Performance audit — DB queries, frontend bundle, caching, AJAX. Finds bottlenecks and suggests quick wins. |
| `/bugfix` | Full bug fix lifecycle — investigate, validate (security/standards/edge-cases), and apply fix. |
| `/debug` | Trace bug root cause — error logs, git blame, dependency analysis. Returns evidence-based report. |

### Build & Release

| Command | Description |
|---------|-------------|
| `/build` | Diagnose and optimize build system (Webpack, Vite, Gulp) — bundle size, dependencies, CSS pipeline. |
| `/release` | Version bump, changelog generation, release notes — full release process management. |

## Typical Workflow

```
/context       ← Extract project context (first time only)
/plan          ← Plan your feature
  ...code...   ← Write the code
/review        ← Review your changes
  ...fix...    ← Address review feedback
/check         ← Final validation
/git-gen       ← Generate commit, branch, PR info
/ship          ← Commit, push, and create PR
```

### Quick Scenarios

**Need a PR fast?**
```
/pr-gen        ← Give it commit hashes, get PR title + description
```

**Last check before merge?**
```
/check         ← Validates all files, returns PASS/FAIL
```

**App feeling slow?**
```
/perf          ← Finds bottlenecks, suggests quick wins
```

## How It Works

Each command is a markdown prompt file in `.claude/commands/`. When you type `/command-name` in Claude Code, it loads the corresponding `.md` file as instructions.

Commands follow a consistent pattern:
1. **Load context** — Reads `.claude/context.yaml` if available
2. **Gather requirements** — Asks targeted questions via interactive prompts
3. **Parallel analysis** — Launches multiple analysis agents simultaneously for speed
4. **Generate output** — Synthesizes findings into actionable results
5. **Execute with approval** — Applies changes only after your confirmation

## Add Your Own Commands

1. Create a new `.md` file in `.claude/commands/`
2. Write your prompt instructions in markdown
3. The filename (without `.md`) becomes the command name
4. Run it with `/your-command`

## Bonus: Reference Guides

This repo also includes handy reference guides:

- [git-aliases.md](git-aliases.md) — Useful git aliases for everyday shortcuts
- [github-commands.md](github-commands.md) — Comprehensive GitHub CLI & Git command reference

## License

MIT
