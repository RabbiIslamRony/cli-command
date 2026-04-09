# Design to Code — Pixel Perfect Implementation

## Autonomous Execution Rules

- Do NOT ask permission before reading, editing, creating, or deleting files — just do it.
- Do NOT ask "should I proceed?" or "can I modify this?" — act autonomously.
- ONLY use AskUserQuestion when gathering requirements, QA info, or resolving genuine ambiguity about what the user wants.
- Once the user approves a plan, execute ALL steps without per-step confirmation.
- Sub-agents inherit these rules — they must also act without asking file-level permissions.

Convert a reference design (Figma, screenshot, image URL) into pixel-perfect code by gathering requirements from the user and launching specialized agents.

## Step 0: Load Project Context

`CLAUDE.md` at the project root is automatically loaded by Claude Code. Use it to understand the project's tech stack, conventions, structure, and workflow throughout this command. Do NOT print or summarize it.

## Step 1: Gather Information from User

Before starting any work, ask the user the following questions using the AskUserQuestion tool. Do NOT skip this step.

### Question Set 1:
Ask these together:

1. **Design Source** — "How will you provide the reference design?"
   - Options: Figma URL, Screenshot/Image file path, Image URL, Already provided above
   - If not already provided, ask the user to share it.

2. **Implementation Target** — "Where should this design be implemented?"
   - Options: New template file, Existing template (user specifies path), SCSS component only, Full page (HTML + SCSS + JS)

3. **Tech Stack** — "Which technologies should be used?"
   - Options: SCSS + PHP template (WordPress), SCSS + React/Vue component, HTML + SCSS only, Match existing pattern (auto-detect)
   - multiSelect: false

### Question Set 2:
After getting design source, ask:

4. **Responsive Requirements** — "Which breakpoints need to be supported?"
   - Options: Desktop only, Desktop + Tablet (768px), All breakpoints (mobile, tablet, desktop) (Recommended), Custom breakpoints
   - multiSelect: false

5. **Design Tokens** — "Should we use existing CSS variables from the project?"
   - Options: Yes, use project CSS variables (--directorist-color-*, etc) (Recommended), No, use exact hex values from design, Mix — variables where available, exact values otherwise

6. **Interaction/JS** — "Does the design have interactive elements?"
   - Options: No interaction needed, Simple interactions (toggle, dropdown, hover), Complex interactions (animations, AJAX, dynamic content), Not sure — analyze the design

## Step 2: Analyze the Design

If the user provides a **Figma URL**, use the Figma MCP tools:
- Call `get_design_context` with the fileKey and nodeId extracted from the URL
- Call `get_screenshot` to get a visual reference
- Call `get_metadata` for design tokens, spacing, typography

If the user provides a **screenshot/image**, use:
- Read the image file to visually analyze it
- Extract layout structure, colors, typography, spacing, component hierarchy

If the user provides an **image URL**, use:
- WebFetch or the image analysis MCP tools to analyze the design

## Step 3: Launch Parallel Agents

Launch these agents **in parallel**:

### Agent 1: Design Token Extraction
Analyze the reference design and extract:
- **Colors**: All colors used (text, backgrounds, borders, shadows)
- **Typography**: Font sizes, weights, line heights, letter spacing
- **Spacing**: Margins, paddings, gaps (use consistent scale like 4px/8px grid)
- **Border radius**: All rounded corners
- **Shadows**: Box shadows, text shadows
- **Layout**: Flex/Grid structure, alignment, dimensions

Map extracted tokens to existing project CSS variables where possible.
Output a structured token list.

### Agent 2: Component Structure Analysis
Analyze the reference design to determine:
- **HTML structure**: Semantic element hierarchy (header, nav, section, article, etc.)
- **CSS class naming**: Follow existing project BEM conventions (`.directorist-*`)
- **Existing components**: Search the codebase for reusable components that match parts of the design
- **New components needed**: List any new components that need to be created
- **Accessibility requirements**: ARIA roles, labels, keyboard interaction patterns

Search the existing codebase (`assets/src/scss/`, `templates/`) to find similar patterns to reuse.

### Agent 3: Responsive Layout Planning
Based on the design and the user's breakpoint requirements:
- **Desktop layout**: Primary layout structure
- **Tablet layout**: What changes at tablet breakpoint
- **Mobile layout**: What stacks, hides, or reflows on mobile
- **Critical measurements**: Min/max widths, fluid vs fixed sizing
- **Overflow handling**: How content behaves when it exceeds bounds

## Step 4: Implementation

After all analysis agents complete, synthesize their findings and implement:

1. **Write SCSS first** — Create/update SCSS file(s) with:
   - All design tokens mapped to CSS variables or exact values
   - BEM class structure matching the project convention
   - Responsive breakpoints with mobile-first or desktop-first approach (match project convention)
   - Proper use of existing mixins/variables from the project

2. **Write HTML/PHP template** — Create the markup with:
   - Semantic HTML5 elements
   - Proper accessibility attributes (ARIA, roles, labels)
   - WordPress template conventions (esc_html, esc_attr, __, etc.)
   - Proper class names matching the SCSS

3. **Write JS if needed** — For interactive elements:
   - Vanilla JS following project patterns (IIFE, event delegation)
   - Keyboard accessibility support
   - Progressive enhancement approach

## Step 5: Pixel Comparison

After implementation, ask the user to check the result against the reference design. Provide a checklist:

- [ ] Layout matches reference (structure, alignment)
- [ ] Colors match reference (text, backgrounds, borders)
- [ ] Typography matches (font size, weight, line height)
- [ ] Spacing matches (margins, paddings, gaps)
- [ ] Border radius matches
- [ ] Shadows match
- [ ] Responsive behavior works correctly
- [ ] Interactive elements work as expected
- [ ] Accessibility is preserved (keyboard nav, screen reader)

Ask the user to screenshot the implementation so you can compare with the original design visually using the image analysis tools.

## Important Rules

- NEVER guess measurements — extract exact values from the design
- ALWAYS check existing codebase for reusable components before creating new ones
- ALWAYS follow the project's naming convention (BEM: `.directorist-block__element--modifier`)
- ALWAYS use CSS variables where the project already defines them
- ALWAYS include responsive handling unless explicitly told "desktop only"
- ALWAYS add proper accessibility attributes
- ALWAYS escape output in PHP templates (WordPress coding standards)
- When in doubt, ask the user rather than assuming
