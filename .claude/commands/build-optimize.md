# Build System Specialist — Optimize & Perfect Your Build

Analyze, diagnose, and optimize any project's build system. Works with any tech stack — Webpack, Vite, Gulp, Rollup, esbuild, Parcel, WordPress, React, Vue, Next.js, or even a brand new project that needs a build system from scratch.

**First step is ALWAYS to detect the project type and build tools before doing anything else.**

## Step 0: Detect Project Type

Before asking questions, automatically scan the project:
- Check for `package.json`, `composer.json`, `Cargo.toml`, `go.mod`, `Makefile`, `Dockerfile`
- Check for build configs: `webpack.config.*`, `vite.config.*`, `rollup.config.*`, `gulpfile.*`, `tsconfig.json`, `.babelrc`
- Check for framework markers: `next.config.*`, `nuxt.config.*`, `angular.json`, `vue.config.*`
- If NO build system exists, note this — we may need to set one up from scratch

Use this detection to tailor all subsequent questions and analysis to the actual project.

## Step 1: Gather Requirements from User

Use AskUserQuestion to understand what the user needs. Do NOT skip.

### Question Set 0 — Project Context (ask if not obvious):

1. **Project Type** — "Ei project ta ki dhoroner?"
   - Options:
     - Existing project (already has build system)
     - New project (need to set up build from scratch)
     - Migrating build system (switching tools)
     - Not sure — analyze and tell me

### Question Set 1 — What's the Goal?

1. **Build Task** — "Build system e ki korte chao?"
   - Options:
     - Diagnose build issues (errors, slow build, broken output)
     - Optimize existing build (speed, bundle size, caching)
     - Add new build feature (new entry point, loader, plugin)
     - Full build audit (complete health check)
   - multiSelect: false

2. **Problem Area** — "Kon area te issue or improvement dorkar?"
   - Options:
     - JavaScript bundling (Webpack, Babel, minification)
     - CSS/SCSS compilation (Sass, PostCSS, RTL, minification)
     - Vue components build (vue-loader, admin panels)
     - WordPress Blocks build (@wordpress/scripts)
     - Production archive (.zip, .distignore)
     - Everything — full audit
   - multiSelect: true

3. **Urgency** — "Eita ki production blocking?"
   - Options:
     - Yes — build is broken/failing right now
     - No — optimization/improvement for future
     - Investigating — not sure yet what's wrong

### Question Set 2 — Context (if diagnosing issues):

Only ask these if user selected "Diagnose build issues":

4. **Error Details** — "Build error ta ki? Terminal output share koro."
   - Let the user paste the error message or describe the issue.

5. **When Started** — "Kokhon theke issue hocchhe?"
   - Options:
     - After a recent code change
     - After updating npm packages
     - After Node.js/npm version change
     - Always been slow/broken — just now investigating

### Question Set 2B — Context (if optimizing):

Only ask these if user selected "Optimize existing build":

4. **Current Pain Points** — "Akhon build e ki problem hocchhe?"
   - Options:
     - Build is too slow (takes too long)
     - Bundle size is too large
     - CSS not compiling correctly
     - RTL stylesheets not generating
     - Source maps not working
     - Hot reload/watch not detecting changes
   - multiSelect: true

5. **Target Metrics** — "Ki achieve korte chao?"
   - Options:
     - Faster dev build (watch mode)
     - Smaller production bundle
     - Better code splitting
     - All of the above

## Step 2: Launch Analysis Agents (Parallel)

Launch **5 agents in parallel**:

### Agent 1: Webpack Configuration Auditor
Read and analyze all webpack config files:
- `webpack.common.js` — shared loaders, rules, resolve config
- `webpack.dev.js` — dev mode settings, watch, source maps
- `webpack.prod.js` — production optimizations, plugins, archiving
- `webpack.dev.vue.js` — Vue-specific dev build
- `webpack-entry-list.js` — all entry points

Check for:
- **Redundant loaders** — same file type processed multiple times
- **Missing optimizations** — splitChunks not configured, no tree shaking
- **Outdated plugins** — deprecated webpack plugins
- **Loader order** — correct right-to-left processing chain
- **Resolve aliases** — missing or incorrect aliases
- **Entry point issues** — unnecessary entries, missing entries for new features
- **DevServer config** — HMR/watch mode efficiency

Report: findings with file path, line numbers, and recommendations.

### Agent 2: Bundle Size Analyzer
Analyze the output bundles:
- List all generated JS files in `assets/js/` with their sizes
- List all generated CSS files in `assets/css/` with their sizes
- Identify the **largest bundles** and what's inside them
- Check for **duplicate dependencies** across bundles (same library bundled multiple times)
- Check for **unused code** that's being bundled (dead code not tree-shaken)
- Check if **vendor splitting** would help (separate vendor.js for cached third-party code)
- Compare `.min.js` vs `.js` sizes to verify minification is working

Run: `ls -lhS assets/js/*.js assets/js/*.min.js assets/css/*.css assets/css/*.min.css` for size overview.

Report: size table, largest offenders, optimization suggestions.

### Agent 3: Dependency & Compatibility Checker
Analyze `package.json` and `blocks/package.json`:
- Check for **outdated packages** that might cause issues
- Check for **version conflicts** between main and blocks package.json
- Check **Node.js compatibility** — does the project specify an engine?
- Check **Babel config** — is `@wordpress/babel-preset-default` up to date?
- Check for **security vulnerabilities** in dependencies
- Check **peer dependency** warnings
- Check if `@babel/runtime` version matches what's expected
- Verify **PostCSS + Autoprefixer** browser targets are correct

Run: `npm ls --depth=0 2>&1 | head -50` and check for warnings.

Report: dependency issues, version conflicts, update recommendations.

### Agent 4: SCSS/CSS Pipeline Analyzer
Analyze the SCSS compilation chain:
- Read `postcss.config.js` — what plugins are configured
- Check SCSS file organization in `assets/src/scss/`
- Verify the **loader chain**: sass-loader → postcss-loader → resolve-url-loader → MiniCssExtractPlugin
- Check for:
  - **Unused SCSS imports** — files imported but not contributing styles
  - **Large SCSS files** that could be split
  - **Duplicate styles** across different entry points
  - **RTL generation** — is webpack-rtl-plugin working correctly?
  - **CSS variable consistency** — are variables defined and used properly?
  - **Source map quality** — do dev source maps point to correct SCSS lines?

Report: pipeline status, issues, optimization suggestions.

### Agent 5: Production Build & Distribution Analyzer
Analyze the production build pipeline:
- Read `webpack.prod.js` — FileManagerPlugin copy/archive config
- Read `.distignore` — what's excluded from distribution
- Check for:
  - **Source files leaking into production** — are `assets/src/`, node_modules properly excluded?
  - **Missing files in build** — new templates/assets not included in copy config
  - **Archive integrity** — does the .zip contain everything needed?
  - **Translation catalog** — is `pot.js` generating `.pot` file correctly?
  - **Unnecessary files** — test files, configs, docs included in production
  - **File permissions** — any files with wrong permissions in the archive
  - **Build reproducibility** — will the same source always produce the same output?

Report: distribution checklist, missing/extra files, recommendations.

## Step 3: Consolidated Report

After all agents complete, present:

```
## Build System Report

### Overall Health: HEALTHY / NEEDS ATTENTION / CRITICAL

### Build Pipeline Status
| Pipeline | Status | Issues |
|----------|--------|--------|
| JS Bundling (Webpack) | ✓/✗ | ... |
| SCSS Compilation | ✓/✗ | ... |
| Vue Build | ✓/✗ | ... |
| Blocks Build | ✓/✗ | ... |
| RTL Generation | ✓/✗ | ... |
| Production Archive | ✓/✗ | ... |
| i18n (.pot) | ✓/✗ | ... |

### Bundle Size Summary
| File | Current Size | Status |
|------|-------------|--------|
| (largest bundles) | | |

### Critical Issues (must fix)
1. ...

### Optimization Opportunities
1. ...

### Recommended Actions (priority order)
1. [ ] ...
2. [ ] ...
```

## Step 4: Fix or Optimize

After presenting the report, ask the user:

"Report dekhle — ki korte chao?"

Options:
- "Fix critical issues now"
- "Apply all optimizations"
- "Fix specific issues (I'll pick)"
- "Just save the report — I'll handle later"

If fixing, create a detailed plan with TodoWrite and implement step by step. After each change:
- Run the relevant build command to verify (`npm run dev` or `npm run prod`)
- Compare before/after metrics (build time, bundle size)
- Report the improvement

## Step 5: Verify

After all changes, run a full verification:
- `npm run prod` — ensure production build succeeds
- Compare bundle sizes before/after
- Check no files are missing from the build output
- Verify RTL stylesheets are generated
- Verify source maps are correct in dev mode

Present final before/after comparison.

## Important Rules

- NEVER delete or modify build output files directly — always change source config
- ALWAYS back up webpack configs before modifying (suggest git commit first)
- ALWAYS run the build after changes to verify nothing broke
- ALWAYS check both dev AND prod builds after configuration changes
- NEVER update major versions of webpack/babel/sass without user approval — these can break everything
- If a change is risky, explain the risk and ask before proceeding
- When adding new entry points, check webpack-entry-list.js first
- When modifying loaders, maintain the correct processing order (right to left in webpack)
- Consider the blocks build is SEPARATE — changes to main webpack don't affect blocks
