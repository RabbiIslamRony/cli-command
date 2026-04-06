# Final Check — Pre-deploy Validation

Run a comprehensive final check before deploying or merging. Ensures no errors, no broken code, and everything is production-ready.

## Step 1: Detect Scope

Run `git status` and `git diff trunk...HEAD --stat` to identify all changed files in the current branch.

Separate files into categories:
- **PHP files** (includes/, templates/, config.php, directorist-base.php)
- **SCSS files** (assets/src/scss/)
- **JS files** (assets/src/js/)
- **Template files** (templates/)
- **Build files** (assets/css/, assets/js/*.min.js) — note if build is outdated

## Step 2: Launch Parallel Agents

Launch **5 agents in parallel**:

### Agent 1: PHP Syntax & Error Check
For every changed PHP file:
- Run `php -l <file>` to check for syntax errors
- Check for common PHP errors:
  - Undefined variables or functions
  - Missing semicolons, brackets, parentheses
  - Incorrect function signatures
  - Deprecated function usage (PHP 8.x compatibility)
  - Missing `defined('ABSPATH')` guard in template files
- Check WordPress coding standards:
  - Proper escaping on all output (`esc_html`, `esc_attr`, `esc_url`, `wp_kses`)
  - Proper sanitization on all input (`sanitize_text_field`, `absint`, etc.)
  - Nonce verification on AJAX handlers and form submissions
  - `current_user_can()` capability checks before privileged actions

Report: file path, line number, error description, severity (Error/Warning).

### Agent 2: SCSS/CSS Validation
For every changed SCSS file:
- Check for syntax errors (unclosed brackets, missing semicolons)
- Check for:
  - Undefined SCSS variables
  - Invalid CSS property values
  - Duplicate selectors or properties
  - Missing responsive breakpoints for new components
  - Hardcoded colors that should use CSS variables (`--directorist-color-*`)
  - RTL compatibility (use `margin-inline-start` instead of `margin-left` where applicable)
  - z-index conflicts with existing styles

Report: file path, line number, issue description.

### Agent 3: JavaScript Validation
For every changed JS file:
- Check for syntax errors
- Check for:
  - Undefined variables or functions
  - Missing event cleanup (memory leaks)
  - Console.log/debug statements left in code
  - Strict equality (`===`) usage
  - Proper event delegation for dynamic content
  - Missing error handling on AJAX calls
  - Browser compatibility issues (no ES6+ in non-compiled files unless supported)

Report: file path, line number, issue description.

### Agent 4: Template Integrity Check
For every changed template file:
- Verify all PHP opening/closing tags are balanced
- Check that all translatable strings use `__()`, `esc_html__()`, `esc_html_e()`, `esc_attr__()`
- Check that all HTML output is properly escaped
- Verify HTML structure validity (unclosed tags, improper nesting)
- Check that all template files have the `defined('ABSPATH')` guard
- Verify `wp_kses_post()` or `wp_kses()` is used for rich HTML output
- Check for hardcoded text strings that should be translatable

Report: file path, line number, issue description.

### Agent 5: Build & Integration Check
- Check if source files (SCSS/JS in `assets/src/`) have corresponding compiled files in `assets/css/` and `assets/js/`
- Verify if the build is up to date by comparing timestamps or checking git status
- Look for any merge conflict markers (`<<<<<<<`, `=======`, `>>>>>>>`) in all changed files
- Check for any `TODO`, `FIXME`, `HACK`, `XXX` comments in changed files that might need attention
- Verify no debug/development code is left:
  - `error_log()` calls
  - `var_dump()` / `print_r()`
  - `console.log()` / `console.debug()`
  - `wp_die()` used for debugging
- Check that no sensitive data is exposed (API keys, passwords, tokens)

Report: file path, line number, issue description.

## Step 3: Consolidated Report

After all agents complete, present a single report:

### Format:

```
## Final Check Report — [branch name]

### Status: PASS / FAIL / WARNINGS

### Errors (must fix before merge)
| # | File | Line | Issue | Agent |
|---|------|------|-------|-------|

### Warnings (should fix)
| # | File | Line | Issue | Agent |
|---|------|------|-------|-------|

### Info (nice to fix)
| # | File | Line | Issue | Agent |
|---|------|------|-------|-------|

### Build Status
- [ ] SCSS compiled and up to date
- [ ] JS compiled and up to date
- [ ] No merge conflict markers
- [ ] No debug code remaining

### Summary
X errors, Y warnings, Z info items found across N files.
```

If there are **0 errors**, report: **"All clear! Ready to merge."**
If there are errors, report: **"Fix X errors before merging."** and list them in priority order.
