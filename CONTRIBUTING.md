# Contributing to Claude Code Custom Commands

Thank you for your interest in contributing! This project grows through community contributions — whether it's a new command, a bug fix, documentation improvement, or a feature idea.

## How to Contribute

### 1. Fork & Clone

```sh
# Fork via GitHub UI or CLI
gh repo fork RabbiIslamRony/cli-command --clone
cd cli-command
```

### 2. Create a Branch

Always branch from `development`:

```sh
git checkout development
git pull origin development
git checkout -b feature/your-feature-name
```

**Branch naming conventions:**

| Type | Format | Example |
|------|--------|---------|
| New command | `feature/command-name` | `feature/command-lint` |
| Bug fix | `fix/description` | `fix/plan-context-loading` |
| Docs | `docs/description` | `docs/add-examples` |
| Improvement | `improve/description` | `improve/review-output` |

### 3. Make Your Changes

- Follow the existing code style and patterns
- Test your changes locally in Claude Code
- Keep changes focused — one feature or fix per PR

### 4. Submit a Pull Request

```sh
git add .
git commit -m "feat: add /lint command for code linting"
git push -u origin feature/your-feature-name
```

Then create a PR **targeting the `development` branch** (not `main`):

```sh
gh pr create --base development
```

> **Important:** All PRs must target the `development` branch. PRs to `main` will be rejected by CI.

---

## Adding a New Command

New commands are the most common contribution. Here's how:

### Step 1 — Create the command file

```sh
# Create your command file
touch .claude/commands/your-command.md
```

### Step 2 — Follow the command structure

Every command should follow this pattern:

```markdown
# Command Name

## Description
What this command does (1-2 sentences).

## Instructions
1. Load context — Read `.claude/context.yaml` if available
2. Gather input — Ask the user what they need
3. Analyze — Run analysis agents in parallel where possible
4. Output — Generate clear, actionable results
5. Execute — Apply changes only after user confirmation
```

### Step 3 — Test it locally

Copy the command to your project's `.claude/commands/` folder and run it:

```
/your-command
```

### Step 4 — Update documentation

- Add the command to the table in `README.md` under the appropriate section
- Include a one-line description

---

## Improving Existing Commands

Found a way to make a command better? Great! When modifying existing commands:

- Don't break existing behavior
- Test with multiple project types if possible
- Describe what changed and why in your PR

---

## Adding Reference Guides

Want to add a new reference guide (like `git-aliases.md` or `github-commands.md`)?

1. Create a new `.md` file in the repo root
2. Follow the existing format — clear headings, tables, code blocks
3. Add a link in the **Bonus: Reference Guides** section of `README.md`

---

## Commit Message Format

Use [Conventional Commits](https://www.conventionalcommits.org/):

| Type | When to use |
|------|-------------|
| `feat:` | New command or feature |
| `fix:` | Bug fix |
| `docs:` | Documentation only |
| `improve:` | Enhancement to existing command |
| `chore:` | Build, config, or maintenance |

**Examples:**

```
feat: add /lint command for code style checking
fix: resolve context loading issue in /plan command
docs: add Docker workflow examples
improve: enhance /review security analysis output
```

---

## Reporting Issues

Found a bug or have an idea? [Open an issue](https://github.com/RabbiIslamRony/cli-command/issues/new):

- **Bug report** — Describe what happened, what you expected, and steps to reproduce
- **Feature request** — Describe the command or improvement you'd like to see
- **Question** — Ask anything about using or contributing to the project

---

## Code of Conduct

Be respectful and constructive. We welcome contributors of all skill levels and backgrounds. See [CODE_OF_CONDUCT.md](CODE_OF_CONDUCT.md) for details.

---

## Need Help?

- Check existing [issues](https://github.com/RabbiIslamRony/cli-command/issues) for similar questions
- Open a new issue if you're stuck
- Tag your issue with `help wanted` or `good first issue` if applicable

Thank you for helping make Claude Code commands better for everyone!
