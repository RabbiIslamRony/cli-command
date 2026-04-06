# Bug Fix — Investigate, Validate & Apply

End-to-end bug fixing — collect info, investigate root cause, ask clarifying questions, validate the fix for security & standards, then apply.

## Step 0: Load Project Context

`CLAUDE.md` at the project root is automatically loaded by Claude Code. Use it to understand the project's tech stack, conventions, structure, and workflow throughout this command. Do NOT print or summarize it.

## Step 1: Collect Bug Info

Use AskUserQuestion to gather details. Ask ALL questions in a SINGLE AskUserQuestion call (multiple questions array).

### Questions:

1. **Bug Description** — "Bug ta ki? Ki hocchhe ar ki howa uchit chilo?"
   - Options:
     - PHP error / fatal / warning
     - JavaScript error / console error
     - UI/CSS broken or wrong
     - Wrong data / logic error
   - multiSelect: false

2. **Where** — "Kon page/feature e hocchhe? File path janle share koro."
   - Options:
     - I know the file/location (I'll share)
     - I know the page/feature but not the file
     - Not sure — need to find it
   - multiSelect: false

3. **Error Details** — "Error message, screenshot, ba steps to reproduce share koro."
   - Options:
     - I have an error message (I'll paste)
     - I can describe the steps to reproduce
     - I have a screenshot
     - No error — just wrong behavior
   - multiSelect: true

4. **Priority** — "Fix kokhon lagbe?"
   - Options:
     - Critical — production e broken
     - High — important feature affected
     - Medium — annoying but workaround ache
     - Low — minor issue
   - multiSelect: false

After getting answers, if anything is unclear or you need more specific info (file path, error message text, steps), ask ONE follow-up AskUserQuestion. Maximum 2 rounds of questions total — then start investigating.

## Step 2: Investigation (Parallel Agents)

Launch these agents IN PARALLEL based on the info collected:

### Agent 1: Bug Locator
Find the exact location and cause:
- If file path given: read the file, find the problematic code
- If only page/feature given: search the codebase for related files (templates, controllers, hooks, AJAX handlers)
- Trace the code execution path — what calls what
- Read error-related files with surrounding context (100 lines)
- `git log -10 --follow -p -- <file>` to check recent changes
- `git blame` on suspicious lines
- Report: exact file, line, what's wrong, and why

### Agent 2: Pattern & Context Analyzer
Understand how the codebase handles similar cases:
- Find similar functionality elsewhere in the project
- Check how the same pattern is implemented in other files
- Look for existing utility functions, helpers, or base classes that should be used
- Check if this is a recurring issue (grep for similar bugs/fixes in git log)
- Report: how the codebase normally handles this, what pattern to follow

### Agent 3: Environment & Dependency Check
Check external factors:
- PHP version compatibility of the affected code
- WordPress hooks/filters — are they firing correctly?
- Database queries — are they correct? Missing indexes?
- Dependencies — does this code depend on other plugins/features being active?
- Check if the issue is in compiled/built files vs source files
- Report: any environment factors contributing to the bug

## Step 3: Diagnosis & Fix Options

After all agents complete, present a clear diagnosis:

```
## Bug Diagnosis

### Problem
[One-line summary of what's wrong]

### Root Cause
[WHY it happens — not just where]

### Location
[file:line] — [what this code does wrong]

### Evidence
[Git blame, related code, pattern analysis]
```

Then present fix options using AskUserQuestion:

"Fix er jonno ei options ache:"

- Show 2-3 fix approaches (if multiple exist) with pros/cons
- If only one clear fix, show that with explanation
- Options:
  - Option A: [description] (Recommended)
  - Option B: [description] (if applicable)
  - Let me fix it myself — just show the code
  - Need more investigation

## Step 4: Pre-Apply Validation (Parallel Agents)

BEFORE applying any code change, launch these validation agents IN PARALLEL:

### Validation Agent 1: Security Review
- Check for XSS vulnerabilities (unescaped output)
- Check for SQL injection (direct variable in queries)
- Check for CSRF (missing nonce verification)
- Check for proper data sanitization (sanitize_text_field, esc_html, wp_kses, etc.)
- Check for authorization (current_user_can, capability checks)
- Check for direct file access protection
- Report: PASS or list of security issues to fix

### Validation Agent 2: Standards & Patterns
- Does the fix follow WordPress coding standards?
- Does the fix match the project's existing code patterns?
- Are proper hooks/filters used instead of direct modifications?
- Is proper i18n used for any user-facing strings?
- Are proper WordPress APIs used (WP_Query, wpdb->prepare, etc.)?
- Report: PASS or list of standards violations

### Validation Agent 3: Error & Edge Case Check
- Could this fix break anything else? (search for dependencies on the changed code)
- Are edge cases handled? (null, empty, wrong type, missing data)
- Does the fix handle both the bug scenario AND normal scenarios?
- If CSS/JS: does it work for RTL? Responsive? Accessibility?
- If PHP: does it handle WP multisite? Different user roles?
- Report: PASS or list of potential issues

### Validation Agent 4: Best Result Verification
- Is this the simplest fix that solves the problem?
- Is there unnecessary code that can be removed?
- Are there performance implications? (N+1 queries, unnecessary loops)
- Does the fix actually solve the reported bug? (trace the execution path with the fix applied)
- Report: PASS or suggestions for improvement

## Step 5: Apply Fix

After ALL validation agents pass:

1. If all agents report PASS — apply the fix immediately. Do NOT ask for permission again.
2. If any agent reports issues — fix those issues first, then apply. Show the user what additional changes were needed.
3. Apply ALL changes in one go — do not ask permission for each file separately.

When applying:
- Edit source files only (not compiled CSS/JS)
- If SCSS was changed, remind user to rebuild assets
- If multiple files need changes, make all changes without asking per-file permission

After applying, show a summary:

```
## Fix Applied

### Changes Made
- [file:line] — [what changed and why]

### Validation Results
- Security: PASS
- Standards: PASS
- Edge Cases: PASS
- Best Result: PASS

### Testing
[How to verify the fix works — specific steps]
```

Then ask using AskUserQuestion:

"Fix apply hoye geche. Ar kichu korte hobe?"

Options:
- All good — done
- Run `/push` to commit & push
- Something's not right — need adjustment
- Revert the changes

## Important Rules

- NEVER guess — always investigate with evidence before suggesting a fix
- NEVER apply changes without completing ALL validation agents first
- NEVER ask per-file permission — once user approves the fix approach, apply everything
- ALWAYS check git history — most bugs come from recent changes
- ALWAYS validate security before applying — WordPress plugins are public-facing
- If the fix is in SCSS/JS source, remind about build step
- If multiple valid fixes exist, present options — don't just pick one
- Keep user interaction minimal — ask smart questions upfront, don't go back and forth
- Maximum 2 rounds of questions before starting investigation
- If you genuinely cannot find the bug, say so and suggest manual debugging steps
- For WordPress: always verify the fix works with the WordPress way (hooks, filters, APIs)
