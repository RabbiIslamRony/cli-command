# Claude Code Custom Commands

[![Contributions Welcome](https://img.shields.io/badge/contributions-welcome-brightgreen.svg)](CONTRIBUTING.md)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-blue.svg)](https://github.com/RabbiIslamRony/cli-command/pulls)

A collection of powerful AI-powered slash commands for [Claude Code](https://docs.anthropic.com/en/docs/claude-code). Boost your development workflow with smart planning, code review, git automation, debugging, and more.

> **Open source & community-driven** — contributions, new commands, and improvements are always welcome!

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

---

## Contributing

We love contributions! Whether you want to add a new command, improve an existing one, fix a bug, or enhance documentation — you're welcome here.

### Quick Start

```sh
# 1. Fork & clone
gh repo fork RabbiIslamRony/cli-command --clone
cd cli-command

# 2. Branch from development
git checkout development
git checkout -b feature/your-feature

# 3. Make changes & test locally

# 4. Submit PR → development branch
gh pr create --base development
```

> **All PRs must target the `development` branch.** PRs to `main` will be rejected.

### Ways to Contribute

| Contribution | Description |
| ------------- | ------------- |
| **New Command** | Create a `.md` file in `.claude/commands/` — it becomes a slash command |
| **Improve Command** | Make an existing command smarter, faster, or more accurate |
| **Reference Guide** | Add a new cheat sheet or reference doc |
| **Bug Fix** | Found something broken? Fix it and submit a PR |
| **Documentation** | Improve README, add examples, fix typos |
| **Ideas** | [Open an issue](https://github.com/RabbiIslamRony/cli-command/issues/new) with your suggestion |

### Adding a New Command

1. Create `.claude/commands/your-command.md`
2. Follow the existing command structure (load context → analyze → output)
3. Test locally by typing `/your-command` in Claude Code
4. Add it to the command table in this README
5. Submit a PR

See [CONTRIBUTING.md](CONTRIBUTING.md) for detailed guidelines, commit conventions, and more.

---

## Community

- **Issues** — [Report bugs or request features](https://github.com/RabbiIslamRony/cli-command/issues)
- **Discussions** — Share ideas, ask questions, show what you've built
- **Stars** — If this project helps you, give it a star to help others find it

### Good First Issues

New to the project? Look for issues labeled [`good first issue`](https://github.com/RabbiIslamRony/cli-command/labels/good%20first%20issue) — these are beginner-friendly tasks perfect for your first contribution.

---

## Contributors

Thanks to everyone who has contributed to this project!

<!-- Contributors will be listed here as they contribute -->

[![Contributors](https://contrib.rocks/image?repo=RabbiIslamRony/cli-command)](https://github.com/RabbiIslamRony/cli-command/graphs/contributors)

---

## License

MIT — see [LICENSE](LICENSE) for details.

## Code of Conduct

Please read our [Code of Conduct](CODE_OF_CONDUCT.md) before contributing. We want this to be a welcoming space for everyone.
