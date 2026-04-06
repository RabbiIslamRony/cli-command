# Feature Planner — Smart Implementation Planning

Before writing any code, deeply analyze the project and create the best possible implementation plan. This command gathers requirements from the user, studies the codebase architecture, and delivers a detailed action plan with improvement suggestions.

## Step 1: Gather Feature Requirements

Use AskUserQuestion to collect info from the user. Do NOT skip any question.

### Question Set 1 — Feature Basics:

1. **Feature Type** — "Ki dhoroner kaj korte chao?"
   - Options: New feature (notun kichu add), Improve existing feature, Bug fix, Refactor/Cleanup
   - multiSelect: false

2. **Feature Description** — "Feature ta ki? Ektu details bolo."
   - This is critical — if the user's answer is vague, ask follow-up questions to clarify scope, expected behavior, and edge cases.

3. **Priority** — "Ei feature er priority ki?"
   - Options: Critical (blocking release), High (must have this sprint), Medium (nice to have), Low (backlog/future)

### Question Set 2 — Scope & Constraints:

4. **Affected Areas** — "Ei feature kon kon area te impact korbe?"
   - Options: Frontend only (templates, SCSS, JS), Backend only (PHP classes, AJAX, DB), Full stack (frontend + backend), Admin panel only
   - multiSelect: true

5. **Has Reference?** — "Kono reference design, PR, issue, or example ache?"
   - Options: Figma/design file, GitHub issue link, Reference from another plugin/site, No reference — plan from scratch

6. **Breaking Changes** — "Ei feature ki existing functionality break korte pare?"
   - Options: No breaking changes expected, Might affect existing templates, Might affect existing data/DB, Not sure — need analysis

## Step 2: Launch Analysis Agents (Parallel)

Launch **4 agents in parallel** to deeply study the codebase:

### Agent 1: Project Architecture Scanner
Scan the entire project to understand:
- **Directory structure** — map out `includes/`, `templates/`, `assets/src/`, `blocks/`
- **Design patterns used** — MVC? Repository? Service classes? How models work?
- **Class hierarchy** — key base classes, inheritance chains
- **Hook system** — what WordPress hooks (actions/filters) are available for extension
- **Data flow** — how data moves from DB → PHP model → template → frontend

Focus on areas the user mentioned in "Affected Areas". Output a brief architecture summary.

### Agent 2: Related Code Finder
Based on the user's feature description, find:
- **Existing similar features** — code that does something similar to what the user wants
- **Files that will need changes** — list every file that likely needs modification
- **Reusable components** — existing PHP classes, JS modules, SCSS components that can be reused
- **Database schema** — relevant custom post types, meta fields, options, taxonomies
- **AJAX endpoints** — existing handlers that are related
- **Template structure** — relevant template files and their hierarchy

For each found item, include: file path, line numbers, brief description of what it does.

### Agent 3: Dependency & Impact Analyzer
Analyze what the feature will touch and what might break:
- **Direct dependencies** — files/functions that the feature will directly modify
- **Reverse dependencies** — other code that depends on what we're changing (use grep to find all references)
- **Template overrides** — check if templates can be overridden by themes (if so, changes need backward compatibility)
- **Filter/Action hooks** — existing hooks that other plugins/extensions might use
- **Database migrations** — will we need new meta fields, options, or schema changes?
- **Cache invalidation** — does anything need cache clearing after changes?

### Agent 4: Best Practices & Standards Checker
Review the project to understand coding conventions:
- **PHP style** — namespace usage, class structure, method naming (camelCase vs snake_case)
- **JS style** — IIFE vs modules, jQuery vs vanilla, event delegation patterns
- **SCSS style** — BEM naming, variable usage, file organization
- **Template style** — escaping patterns, i18n usage, component inclusion
- **Testing patterns** — are there tests? What framework? What's the coverage approach?
- **Git workflow** — branch naming, commit message style, PR conventions

## Step 3: Create Implementation Plan

After all agents complete, synthesize findings into a structured plan.

### Plan Format:

```
## Feature Plan: [Feature Name]

### Understanding
[1-2 sentences confirming what the feature does, in your own words. Ask the user to verify.]

### Architecture Decision
[Based on the codebase analysis, explain the BEST approach to implement this feature. Explain WHY this approach is best, considering the existing patterns.]

### Files to Create
| # | File Path | Purpose |
|---|-----------|---------|

### Files to Modify
| # | File Path | What Changes | Lines Affected |
|---|-----------|-------------|----------------|

### Implementation Steps (Priority Order)
1. [ ] Step 1 — description (file path)
2. [ ] Step 2 — description (file path)
...

### Database/Data Changes
- New meta fields: ...
- New options: ...
- Migration needed: yes/no

### Hooks to Add
- Filters: ... (so extensions can modify behavior)
- Actions: ... (so extensions can add functionality)

### Edge Cases to Handle
1. ...
2. ...

### Risks & Mitigation
| Risk | Impact | Mitigation |
|------|--------|-----------|

### Estimated Complexity
- PHP: X files, ~Y lines
- JS: X files, ~Y lines  
- SCSS: X files, ~Y lines
- Templates: X files
```

## Step 4: Suggest Improvements

After presenting the plan, ALWAYS check for improvement opportunities. Ask the user:

"Ei feature er sathe related kisu improvement er scope dekhtechi. Eishob ki korte chao?"

Present improvements as a checklist using AskUserQuestion with multiSelect: true:

Potential improvements to look for:
- **Performance** — can we add caching? Lazy loading? Reduce DB queries?
- **Accessibility** — ARIA attributes, keyboard navigation, screen reader support
- **Responsive** — mobile/tablet handling
- **Internationalization** — are all strings translatable?
- **Security hardening** — additional escaping, capability checks, rate limiting
- **UX polish** — loading states, error messages, empty states, animations
- **Developer experience** — add hooks/filters for extensibility
- **Backward compatibility** — deprecation notices instead of hard removal

Only suggest improvements that are genuinely useful — don't suggest unnecessary things.

## Step 5: Get Approval

After presenting the plan and improvement suggestions, ask:

"Plan ta ki thik ache? Kisu change korte chaile bolo. Ready hole implementation shuru korbo."

Options:
- "Looks good — start implementation"
- "I want to modify the plan"
- "Add the suggested improvements too"
- "Let me think — save the plan for later"

If the user wants modifications, update the plan and re-present. Do NOT start coding until the user explicitly approves.

## Important Rules

- NEVER start coding without user approval on the plan
- ALWAYS show the plan first, even if the feature seems simple
- ALWAYS check for reusable existing code — don't reinvent the wheel
- ALWAYS consider backward compatibility for template changes
- ALWAYS suggest adding WordPress hooks (filters/actions) for extensibility
- ALWAYS consider the impact on existing extensions/themes
- If the feature is too large, suggest breaking it into smaller PRs
- If you find existing bugs in related code, mention them but don't fix unless asked
