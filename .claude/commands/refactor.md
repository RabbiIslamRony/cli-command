# Refactor Specialist — Safe Code Modernization

## Autonomous Execution Rules

- Do NOT ask permission before reading, editing, creating, or deleting files — just do it.
- Do NOT ask "should I proceed?" or "can I modify this?" — act autonomously.
- ONLY use AskUserQuestion when gathering requirements, QA info, or resolving genuine ambiguity about what the user wants.
- Once the user approves a plan, execute ALL steps without per-step confirmation.
- Sub-agents inherit these rules — they must also act without asking file-level permissions.

Detect code smells, DRY violations, legacy patterns, and safely refactor without breaking existing functionality. Works with any codebase.

## Step 0: Load Project Context

`CLAUDE.md` at the project root is automatically loaded by Claude Code. Use it to understand the project's tech stack, conventions, structure, and workflow throughout this command. Do NOT print or summarize it.

## Step 1: Gather Requirements

Use AskUserQuestion to collect info. Do NOT skip.

### Question Set 1 — Scope:

1. **Refactor Target** — "What would you like to refactor?"
   - Options:
     - Specific file/class (I'll tell you which)
     - Specific feature area (e.g., dashboard, search, listings)
     - Full codebase scan for code smells
     - Legacy code modernization (update old patterns)
   - multiSelect: false

2. **Refactor Goal** — "What is the main goal of this refactor?"
   - Options:
     - Reduce code duplication (DRY)
     - Improve readability & maintainability
     - Modernize patterns (old code → new standards)
     - Extract reusable components
     - Simplify complex logic
   - multiSelect: true

3. **Risk Tolerance** — "How much risk are you willing to take?"
   - Options:
     - Safe only — no behavior change, just structure (Recommended)
     - Moderate — small behavior improvements ok
     - Aggressive — willing to break backward compatibility for better code

### Question Set 2 — Constraints:

4. **Must Preserve** — "What must be preserved during the refactor?"
   - Options:
     - All public APIs (function signatures, hooks, filters)
     - Template override compatibility (theme overrides must still work)
     - Database structure (no schema changes)
     - All of the above
   - multiSelect: true

## Step 2: Launch Analysis Agents (Parallel)

Launch **4 agents in parallel**:

### Agent 1: Code Smell Detector
Scan the target area for:
- **Duplication** — find copy-pasted code blocks (similar logic in multiple places)
  - Search for similar function bodies across files
  - Find repeated query patterns, similar template blocks
- **Long methods** — functions over 50 lines that should be broken down
- **God classes** — classes with too many responsibilities (over 500 lines)
- **Deep nesting** — code with 4+ levels of if/foreach/while nesting
- **Magic numbers/strings** — hardcoded values that should be constants
- **Dead code** — unused functions, unreachable conditions, commented-out code
- **Parameter bloat** — functions with 5+ parameters

For each smell: file path, line range, smell type, severity (Critical/Major/Minor).

### Agent 2: Pattern Analyzer
Understand existing patterns to ensure refactoring stays consistent:
- **Naming conventions** — how are classes, methods, variables, hooks named?
- **File organization** — how is code split across files and directories?
- **Design patterns** — what patterns are already in use? (Repository, Factory, Observer, etc.)
- **Dependency injection** — how do classes receive dependencies?
- **Error handling** — how are errors handled? (exceptions, wp_die, error codes)
- **Hook patterns** — how are WordPress actions/filters structured?

This informs how refactored code should look to stay consistent.

### Agent 3: Dependency Mapper
Map what depends on what in the target area:
- **Class dependencies** — which classes instantiate or reference which
- **Function call graph** — who calls what
- **Hook connections** — which actions/filters connect code paths
- **Template dependencies** — which templates include/require which
- **External consumers** — are there extensions, themes, or third-party code that depends on this?
  - Check for `do_action` and `apply_filters` that external code might hook into
  - Check for template files that themes might override

This identifies what's SAFE to change vs what will break things.

### Agent 4: Improvement Opportunity Scanner
Look for modernization opportunities:
- **PHP modernization** — null coalescing (`??`), type hints, return types, match expressions
- **WordPress modern APIs** — newer functions that replace deprecated ones
- **Code extraction** — repeated patterns that could become helper functions or traits
- **Configuration extraction** — hardcoded values that should be filterable or in settings
- **Hook additions** — places where adding filters/actions would make code more extensible
- **Early returns** — nested ifs that could be flattened with guard clauses

## Step 3: Refactoring Plan

Present a structured plan before making any changes:

```
## Refactoring Plan

### Scope
[What files/areas will be changed]

### Code Smells Found
| # | Type | File | Lines | Severity | Safe to Fix |
|---|------|------|-------|----------|-------------|

### Proposed Changes (priority order)
| # | Change | File | Risk | Impact |
|---|--------|------|------|--------|
| 1 | Extract helper function for X | file.php | Low | Removes 3 duplications |
| 2 | ... | | | |

### Dependency Safety Check
- Public APIs affected: [none / list]
- Hooks affected: [none / list]
- Template overrides affected: [none / list]
- Database changes: [none / list]

### Will NOT Change (too risky)
- [List of things intentionally left alone with reason]
```

## Step 4: Get Approval & Execute

Ask: "Does the plan look good? Should I start?"
- "Yes — apply all changes"
- "Apply safe changes only"
- "Let me pick which ones"
- "Modify the plan first"

When implementing:
- Make ONE logical change at a time
- After each change, verify no syntax errors
- Keep a mental before/after of what improved
- If a change is risky, ask before applying

## Step 5: Verification

After refactoring:
- Run `php -l` on changed PHP files
- Check for any broken references (grep for renamed functions/classes)
- Verify no hooks were accidentally removed
- Present before/after metrics:
  - Lines of code (before → after)
  - Number of duplications removed
  - Complexity reduction

## Important Rules

- NEVER change function signatures that are part of the public API without user approval
- NEVER remove WordPress hooks (do_action/apply_filters) — extensions may depend on them
- NEVER change template file names — themes may override them
- ALWAYS refactor in small, verifiable steps — not one giant change
- ALWAYS preserve backward compatibility unless explicitly told otherwise
- If you find bugs during refactoring, report them but don't fix unless asked (separate concern)
- Refactoring should NOT change behavior — if it does, that's a separate task
- When in doubt about safety, ask the user
