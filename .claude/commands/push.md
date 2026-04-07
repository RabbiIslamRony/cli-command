# Git Push — Quick Stage, Commit & Push

## Autonomous Execution Rules

- Do NOT ask permission before reading, editing, creating, or deleting files — just do it.
- Do NOT ask "should I proceed?" or "can I modify this?" — act autonomously.
- ONLY use AskUserQuestion when gathering requirements, QA info, or resolving genuine ambiguity about what the user wants.
- Once the user approves a plan, execute ALL steps without per-step confirmation.
- Sub-agents inherit these rules — they must also act without asking file-level permissions.

Simple and fast — stage all changes, commit with a smart message, and push to current or specific branch. No formatting, no PR, no extra steps.

## Step 1: Check Changes

```bash
git status
git diff --stat
```

If there are no changes (clean working tree), tell the user "No changes found — nothing to commit." and stop.

## Step 2: Generate Commit Message

Read the actual diff to understand what changed:

```bash
git diff -- ':(exclude)assets/css/*' ':(exclude)assets/js/*'
git diff --staged -- ':(exclude)assets/css/*' ':(exclude)assets/js/*'
```

Also check recent commit style:

```bash
git log --oneline -5
```

Generate a short, meaningful commit message based on:
- What actually changed in the source files
- Follow the project's existing commit message style (from git log)
- Keep it concise (1 line, max 72 chars)

Show the user the commit message and ask using AskUserQuestion:

"Does this commit message look good?"

- Options:
  - Yes, use this
  - No, I'll write my own (then ask for their message)

## Step 3: Stage & Commit

```bash
git add .
git commit -m "<commit message>"
```

If commit fails, show the error and ask the user how to proceed. Do NOT amend.

## Step 4: Push

Check current branch and remotes:

```bash
git branch --show-current
git remote -v
```

Ask the user using AskUserQuestion:

"Where should I push to?"

- Options:
  - Push to origin/<current-branch> (Recommended)
  - Push to a different branch (then ask branch name)
  - Don't push now

If user selects push:

```bash
git push -u <remote> <branch>
```

If push fails (e.g., no upstream, rejected), show the error and ask the user what to do. Never force push without explicit approval.

## Important Rules

- NEVER format code — this command is for quick push only
- NEVER create PR — use `/git-gen` + `/ship` for that
- NEVER force push without explicit user approval
- NEVER stage files that contain secrets (.env, credentials, API keys)
- ALWAYS show the commit message to the user before committing
- ALWAYS ask before pushing — never auto-push
- Keep it fast — minimum questions, maximum speed
