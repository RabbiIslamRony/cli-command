# Debug Detective — Root Cause Finder

## Autonomous Execution Rules

- Do NOT ask permission before reading, editing, creating, or deleting files — just do it.
- Do NOT ask "should I proceed?" or "can I modify this?" — act autonomously.
- ONLY use AskUserQuestion when gathering requirements, QA info, or resolving genuine ambiguity about what the user wants.
- Once the user approves a plan, execute ALL steps without per-step confirmation.
- Sub-agents inherit these rules — they must also act without asking file-level permissions.

Trace bugs, errors, and unexpected behavior to their root cause. Works with any project — PHP, JS, WordPress, React, Node.js, or any stack.

## Step 0: Load Project Context

`CLAUDE.md` at the project root is automatically loaded by Claude Code. Use it to understand the project's tech stack, conventions, structure, and workflow throughout this command. Do NOT print or summarize it.

## Step 1: Gather Bug Details

Use AskUserQuestion to collect info. Do NOT skip.

### Question Set 1 — What's Happening?

1. **Bug Type** — "What type of issue are you experiencing?"
   - Options:
     - PHP error/warning/notice (white screen, fatal error, deprecated notice)
     - JavaScript error (console error, broken UI, not working)
     - Unexpected behavior (code runs but wrong result)
     - Performance issue (slow, timeout, high memory)
   - multiSelect: false

2. **Error Message** — "What is the error message? Please share the console/log output."
   - If user provides an error, parse it for: file path, line number, error type, stack trace.
   - If no error message, ask them to describe what happens vs what should happen.

3. **Reproducibility** — "Does the bug happen always or only sometimes?"
   - Options:
     - Always reproducible
     - Sometimes / intermittent
     - Only in production (not in local)
     - Only after specific action

### Question Set 2 — Context:

4. **When Started** — "When did this start happening?"
   - Options:
     - After a recent code change (I know which one)
     - After a recent code change (not sure which)
     - After updating WordPress/PHP/Node
     - Always been there — just noticed now

5. **Environment** — "Which environment is this happening in?"
   - Options:
     - Local development
     - Staging/testing server
     - Production
     - All environments
   - multiSelect: true

## Step 2: Launch Investigation Agents (Parallel)

Based on the bug type, launch the appropriate combination of agents:

### Agent 1: Error Trace Analyzer
Parse the error message/stack trace to find:
- **Exact file and line** where the error originates
- **Call stack** — trace the execution path that led to the error
- **Variable state** — what values could cause this error at that line
- Read the file at the error line and surrounding context (50 lines before/after)
- Check if the error line was recently changed: `git log -5 --follow -p -- <file>`
- Check if there are similar errors reported in the codebase (grep for similar patterns)

### Agent 2: Related Code Inspector
From the error location, trace dependencies:
- **Upstream** — what code calls the function that errored? (grep for function/method name)
- **Downstream** — what does the errored function call? Could the issue be in a dependency?
- **Data source** — where does the data come from? (DB query, API, user input, config)
- **Recent changes** — `git log --since="2 weeks ago" -- <related-files>` to find recent modifications
- **Similar patterns** — find other places in the codebase that do the same thing — do they also have this bug?

### Agent 3: Environment & Configuration Checker
Check for environment-related causes:
- **PHP version compatibility** — check for deprecated functions, type errors
- **WordPress version** — check for deprecated hooks, changed APIs
- **Plugin conflicts** — check if the code depends on other plugins/extensions
- **Database state** — check if required options/meta fields exist
- **File permissions** — check if files are readable/writable as needed
- **Cache issues** — check for stale transients, object cache, opcode cache

### Agent 4: Git Blame Detective
Find who/what introduced the bug:
- `git blame <error-file>` on the error lines
- `git log --all --oneline -- <error-file>` recent history
- Check if the bug was introduced by a merge conflict resolution
- Check if related files were changed in the same commit
- Find the PR/commit that likely introduced the issue

## Step 3: Root Cause Report

After all agents complete, present:

```
## Bug Investigation Report

### Error Summary
- **Error**: [error message]
- **Location**: [file:line]
- **Type**: [PHP Fatal / JS TypeError / Logic Error / etc.]

### Root Cause
[Clear explanation of WHY the bug happens — not just WHERE]

### Evidence
1. [file:line] — [what this code does wrong]
2. [related file:line] — [contributing factor]

### Introduced By
- **Commit**: [hash] by [author] on [date]
- **PR**: [if identifiable]
- **Change**: [what the commit changed that caused this]

### Recommended Fix
[Specific code change with file path and line numbers]

### Fix Risk Assessment
- Impact: [Low/Medium/High]
- Files to change: [list]
- Could break: [what else might be affected]

### Prevention
[How to prevent similar bugs — test case, validation, etc.]
```

## Step 4: Fix

Ask the user:

"Root cause has been identified. Should I apply the fix?"

Options:
- "Yes — fix it now"
- "Show me the fix first, I'll decide"
- "I'll fix it myself — thanks for the analysis"
- "Need more investigation"

If fixing, implement the change and suggest a test to verify the fix works.

## Important Rules

- NEVER guess the root cause — always trace with evidence
- ALWAYS read the actual code at the error location before theorizing
- ALWAYS check git history — most bugs come from recent changes
- NEVER modify code without user approval during investigation
- If multiple possible causes exist, rank them by likelihood and investigate top first
- If you can't find the root cause, say so honestly and suggest next debugging steps
- For WordPress: always check if the issue is in the theme, another plugin, or core — not just this plugin
