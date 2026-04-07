# Performance Specialist — Speed & Optimization Audit

Deep performance analysis for any project. Identifies slow queries, heavy bundles, render bottlenecks, memory leaks, and delivers actionable optimizations with before/after metrics.

## Step 0: Load Project Context

`CLAUDE.md` at the project root is automatically loaded by Claude Code. Use it to understand the project's tech stack, conventions, structure, and workflow throughout this command. Do NOT print or summarize it.

## Step 1: Gather Requirements

Use AskUserQuestion to collect info. Do NOT skip.

### Question Set 1 — What's Slow?

1. **Performance Area** — "Which area feels slow?"
   - Options:
     - Page load (frontend slow)
     - Admin panel (backend slow)
     - Specific AJAX action (button click e wait)
     - Database queries (general slowness)
   - multiSelect: true

2. **Specific Page/Action** — "Which page or action is slow? Please share the URL or action name."
   - Let the user describe the exact page, URL, or user action that's slow.

3. **Scale** — "Roughly how much data is involved?"
   - Options:
     - Small (< 100 listings/posts)
     - Medium (100-1000)
     - Large (1000-10000)
     - Very large (10000+)

### Question Set 2 — Metrics:

4. **Current Speed** — "How slow is it currently? Do you have any measurements?"
   - Options:
     - Very slow (5+ seconds)
     - Slow (2-5 seconds)
     - Slightly slow (1-2 seconds)
     - Not sure — just feels slow
     - Have specific metrics (user will share)

5. **Target** — "What is your target speed?"
   - Options:
     - As fast as possible
     - Under 1 second
     - Under 2 seconds
     - Just noticeably faster than now

## Step 2: Launch Analysis Agents (Parallel)

Launch **5 agents in parallel**:

### Agent 1: PHP/Backend Performance Analyzer
Analyze PHP code for performance issues:
- **Database queries** — find all `$wpdb->` calls, `WP_Query`, `get_posts`, `get_option`, `get_post_meta` in the affected area
  - Are queries using indexes? Check for `meta_query` without indexes
  - N+1 query problems — queries inside loops
  - Missing `no_found_rows => true` on queries that don't need pagination
  - Unnecessary `SELECT *` when only specific fields needed
  - `get_option()` calls on non-autoloaded options
- **Caching** — check for missing caching opportunities
  - Transients for expensive operations
  - Object cache usage (`wp_cache_get/set`)
  - Static variable caching for repeated function calls
- **PHP bottlenecks**
  - Heavy computation in template files (should be in model)
  - Unnecessary file includes / class autoloading
  - Large arrays/objects in memory
  - `file_get_contents` or remote HTTP calls without caching

Read the files related to the user's reported slow page/action. Report each issue with file path, line number, and estimated impact.

### Agent 2: Frontend Performance Analyzer
Analyze frontend assets for performance:
- **Bundle analysis** — check sizes of loaded JS/CSS files
  - Which files are loaded on the slow page
  - Are files loaded unnecessarily (script/style enqueued everywhere but only needed on specific pages)
  - Deferred/async loading — are scripts render-blocking?
- **Image optimization** — check for unoptimized images, missing lazy loading
- **DOM complexity** — check templates for excessive nesting, unnecessary wrappers
- **CSS performance** — complex selectors, unused styles on the page
- **Third-party scripts** — external scripts that block rendering
- **Font loading** — font-display strategy, preloading

### Agent 3: Database Query Profiler
Deep analysis of database interactions:
- Map all database queries triggered by the affected page/action
- Find queries in model files (`includes/model/`), class files (`includes/classes/`)
- Check for:
  - **Slow queries** — JOINs across large tables, LIKE with leading wildcards, unindexed meta queries
  - **Redundant queries** — same data fetched multiple times
  - **Missing indexes** — meta_key queries without indexes
  - **Option table abuse** — autoloaded options that shouldn't be, large serialized data
  - **Post meta calls** — individual `get_post_meta()` vs batch loading
- Suggest query optimizations with before/after examples

### Agent 4: Caching Strategy Analyzer
Check current caching and suggest improvements:
- **Transient API usage** — what's cached, what should be cached
- **Object cache compatibility** — is code object-cache friendly?
- **Fragment caching** — template fragments that could be cached
- **Query result caching** — expensive queries that produce stable results
- **HTTP caching** — cache headers on AJAX responses
- **Static asset caching** — proper cache busting on CSS/JS files
- Check for cache invalidation issues (stale data after updates)

### Agent 5: AJAX & API Performance
Analyze AJAX handlers and REST API endpoints:
- **Response time** — trace what each AJAX handler does step by step
- **Payload size** — are AJAX responses returning more data than needed?
- **Request batching** — multiple AJAX calls that could be combined
- **Debouncing/throttling** — rapid-fire requests from user input (search, filters)
- **Error handling** — failed requests causing retry storms
- **Nonce refresh** — stale nonces causing repeated failures

## Step 3: Performance Report

```
## Performance Analysis Report

### Overall Score: FAST / ACCEPTABLE / SLOW / CRITICAL

### Top Bottlenecks (ranked by impact)
| # | Issue | Location | Impact | Fix Difficulty |
|---|-------|----------|--------|---------------|
| 1 | ... | file:line | High | Easy |
| 2 | ... | file:line | High | Medium |

### Database Queries
- Total queries on page: X
- Slow queries (>100ms): X
- Redundant queries: X
- Missing indexes: X

### Frontend Assets
- Total JS loaded: X KB
- Total CSS loaded: X KB
- Render-blocking scripts: X
- Unnecessary assets on this page: X

### Caching Status
- Transients used: X
- Cache hit rate: [if measurable]
- Missing cache opportunities: X

### Quick Wins (easy fixes, big impact)
1. [ ] ...
2. [ ] ...

### Medium Effort Optimizations
1. [ ] ...

### Long-term Improvements
1. [ ] ...

### Expected Improvement
- Estimated page load reduction: ~X seconds
- Estimated query reduction: ~X fewer queries
```

## Step 4: Optimize

Ask: "Which fixes should I apply?"
- "Quick wins only"
- "All optimizations"
- "Let me pick specific ones"
- "Save report — I'll handle later"

Implement chosen fixes, then verify with before/after comparison.

## Important Rules

- ALWAYS measure before changing — note current state for comparison
- NEVER optimize prematurely — fix the actual bottleneck, not what seems slow
- ALWAYS check if WordPress caching plugins might interfere
- NEVER add caching without proper invalidation logic
- Database index changes need careful consideration — they speed reads but slow writes
- ALWAYS test with realistic data amounts, not just empty/small datasets
- Frontend changes should be verified with browser DevTools Network tab
- Consider backwards compatibility when changing query patterns
