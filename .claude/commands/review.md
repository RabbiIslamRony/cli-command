# PR Review Agent

Run a comprehensive PR review on the current branch by launching 3 parallel agents.

## Step 0: Load Project Context

`CLAUDE.md` at the project root is automatically loaded by Claude Code. Use it to understand the project's tech stack, conventions, structure, and workflow throughout this command. Do NOT print or summarize it.

## Instructions

1. First, detect the current branch and the base branch (trunk). Run `git log trunk..HEAD --oneline` and `git diff trunk...HEAD --stat` to understand the scope.

2. Identify the **source files** changed (exclude build/compiled files like `assets/css/`, `assets/js/*.min.js`, `assets/js/admin-settings-manager.js`, `assets/js/admin-multi-directory-builder.js`, `assets/js/formgent-integration.js`, `blocks/build/`, `node_modules/`, `package-lock.json`). Focus on:
   - `assets/src/` (SCSS, JS source)
   - `includes/` (PHP classes, models, helpers)
   - `templates/` (PHP templates)
   - `config.php`, `directorist-base.php`

3. Launch **3 agents in parallel**:

### Agent 1: Code Quality
Review changed source files for:
- Code duplication
- Unused variables or dead code
- Poor naming conventions
- Missing sanitization/escaping in PHP (WordPress context)
- Inconsistent coding patterns
- Unnecessary complexity

Report findings with file paths and line numbers.

### Agent 2: Security
Review changed PHP files for WordPress security issues:
- Nonce verification (`check_ajax_referer` / `wp_verify_nonce`)
- Capability checks (`current_user_can`)
- SQL injection (`$wpdb->prepare` for user input)
- XSS (output escaping: `esc_html()`, `esc_attr()`, `esc_url()`, `wp_kses()`)
- CSRF protection (nonces on forms/AJAX)
- Direct file access (`ABSPATH` check)
- Unvalidated redirects (`wp_safe_redirect`)
- Privilege escalation risks

Rate each finding: Critical / High / Medium / Low.

### Agent 3: UX
Review changed template/SCSS/JS files for:
- Accessibility (ARIA attributes, keyboard nav, color contrast, screen reader support)
- Responsive design (mobile breakpoints, overflow handling)
- Internationalization (strings wrapped in `__()` / `esc_html__()`)
- Edge cases (empty states, long content, error handling)
- UI consistency with existing patterns

4. After all 3 agents complete, present a **consolidated report** with:
   - Summary table of all findings by severity
   - Positive findings (things done well)
   - Recommended priority fixes
