# Project Pulse implementation plan

## Summary

Build Mona's Project Pulse as a small, static, contributor-friendly dashboard. The
first view must clearly identify **Project Pulse** and let a contributor scan active
projects, owners, status, recent activity, and priority or risk without reading a
directory listing. The implementation has no application framework or package
dependency: it uses HTML, CSS, JSON, and the Python standard-library HTTP server
already available in the Codespace.

The deliverables are:

- `app/index.html` — accessible dashboard structure and rendering logic.
- `app/styles.css` — polished responsive visual system and layout.
- `app/project-data.json` — the source data with a top-level `projects` array.
- `.vscode/launch.json` — a strict JSON launch configuration named **Run Project
  Pulse Dashboard**.

## Responsibilities and file assignments

### Orchestrator

- Coordinate the phases below and enforce the file ownership boundaries.
- Give Designer the visual and accessibility brief before implementation.
- Give Coder the approved design direction and this plan before coding.
- Keep the data contract consistent across JSON keys, HTML rendering, CSS hooks,
  and launch configuration.
- Integrate and review all outputs; do not allow unrelated files to be changed.

### Planner

- This document establishes the implementation order, dependencies, edge cases,
  parallel work decisions, and validation expectations.
- Resolve assumptions before implementation: use a static browser app, local JSON,
  no external assets or runtime dependencies, and Python HTTP server port `5500`.

### Designer

- Own the design direction for `app/styles.css` and provide markup guidance for
  `app/index.html`; do not change `app/project-data.json` or
  `.vscode/launch.json`.
- Define a clear information hierarchy: page title and short purpose statement,
  summary context, then a responsive grid of project cards.
- Specify accessible status and priority treatments, including sufficient color
  contrast, text labels in addition to color, visible focus states, readable
  line lengths, and responsive behavior for narrow screens.
- Require deterministic hooks including `.dashboard` and `.project-card`.
- Favor a polished but lightweight interface using rounded cards, restrained
  shadows, clear spacing, typography, status badges, and explicit priority/risk
  labels rather than decorative complexity.
- Review the implemented page visually and recommend corrections without
  expanding the scope beyond the assigned files.

### Coder

- Own implementation of `app/index.html`, `app/project-data.json`, and
  `.vscode/launch.json`, incorporating Designer's guidance and preserving the
  existing repository conventions.
- Build semantic HTML with a `Project Pulse` title, a main dashboard landmark,
  a project-card container, and accessible headings/labels.
- Load `project-data.json` from the browser and render one visible
  `project-card` per project, showing `name`, `owner`, `status`,
  `recentActivity`, and `priority`; surface a clear user-facing error if data
  cannot be loaded instead of silently rendering an empty success state.
- Keep `app/styles.css` as the sole stylesheet and use the Designer's responsive
  and accessibility rules.
- Create `.vscode/launch.json` as strict JSON with no comments. Add the exact
  launch name **Run Project Pulse Dashboard**, run `python3 -m http.server 5500`,
  set the working directory to `${workspaceFolder}/app`, and configure
  `serverReadyAction` to open `http://localhost:%s/index.html`.
- Do not add a build system, dependencies, framework, or unrelated files.

## Ordered implementation phases

### Phase 1: Confirm the contract and design direction

**Owner:** Orchestrator with Planner and Designer  
**Files:** planning only; Designer may prepare guidance for `app/index.html` and
`app/styles.css`.

1. Confirm the required data fields and the top-level `projects` array.
2. Confirm that the app is static and must be served over HTTP so browser JSON
   loading works reliably.
3. Designer defines the card anatomy, visual tokens, responsive breakpoints,
   status/priority semantics, and accessibility requirements.
4. Orchestrator passes the resulting guidance and this plan to Coder.

### Phase 2: Create the content contract

**Owner:** Coder  
**Assignment:** `app/project-data.json`

1. Add a valid JSON object with a top-level `projects` array.
2. Include several representative active team projects so the grid demonstrates
   the intended experience.
3. Give every item non-empty `name`, `owner`, `status`, `recentActivity`, and
   `priority` values.
4. Use a small, consistent vocabulary for status and priority so badges can be
   styled predictably; keep the values human-readable.

### Phase 3: Build the semantic dashboard

**Owner:** Coder, using Designer guidance  
**Assignment:** `app/index.html`

1. Add the exact visible title `Project Pulse`, metadata, and a concise
   contributor-friendly introduction.
2. Link `styles.css` and reference `project-data.json`.
3. Add semantic landmarks and a project grid with deterministic `.dashboard` and
   `.project-card` hooks (the `.dashboard` hook must also be present in CSS).
4. Render cards from the JSON rather than duplicating project content in markup.
5. Expose status, recent activity, and priority in text as well as styling.
6. Handle loading and malformed/unavailable data states accessibly, with a useful
   message and no uncaught silent failure.

### Phase 4: Implement the visual system

**Owner:** Designer, then Coder for integration  
**Assignment:** `app/styles.css`; integration touchpoints in
`app/index.html` are coordinated with Coder.

1. Style the dashboard shell, header, grid, and cards with clear spacing,
   `border-radius`, `box-shadow`, readable typography, and a cohesive palette.
2. Make status badges and priority/risk indicators visually distinct while
   retaining text labels and adequate contrast.
3. Add hover/focus behavior without making information hover-only.
4. Make the layout responsive: cards should reflow cleanly on mobile, tablet,
   and desktop widths without horizontal scrolling.
5. Include visible keyboard focus, reduced-motion-safe transitions if motion is
   used, and sensible defaults for long project names or activity text.

### Phase 5: Make the dashboard runnable

**Owner:** Coder  
**Assignment:** `.vscode/launch.json`

1. Create valid strict JSON with no comments.
2. Add the exact configuration name `Run Project Pulse Dashboard`.
3. Serve `${workspaceFolder}/app` using `python3 -m http.server 5500`.
4. Use `serverReadyAction` to open `http://localhost:%s/index.html`, ensuring
   the browser opens the UI rather than the server directory root.
5. Keep this file independent of external extensions or npm packages.

### Phase 6: Integrate and review

**Owner:** Orchestrator, with Designer and Coder

1. Review the four assigned deliverables against the data contract and design
   guidance.
2. Check that HTML class names and data fields match the CSS and JSON exactly.
3. Check that the launch working directory, command, and URL target the same app.
4. Resolve integration defects in the owning file only, then perform the full
   validation pass below.

## Dependencies

- `app/project-data.json` is the content contract. The HTML renderer depends on
  its top-level `projects` array and five required properties per project.
- `app/index.html` depends on both `app/styles.css` and
  `app/project-data.json`; its selectors and rendered fields must match those
  files.
- `app/styles.css` depends on the semantic class hooks agreed by Designer and
  implemented by Coder, especially `.dashboard` and `.project-card`.
- `.vscode/launch.json` depends on the final app location and the fixed port
  `5500`, but does not need to wait for implementation details beyond
  `app/index.html` existing.
- The browser preview depends on serving through HTTP. Opening the HTML directly
  from the filesystem is not an equivalent validation because fetch behavior can
  be restricted.

## Parallel work decisions

### Work that can run in parallel

- Designer can develop the layout, accessibility, and visual specification while
  Coder prepares the JSON content contract, because they own different files and
  the data schema is already fixed by this plan.
- After the contract is agreed, Designer can refine CSS guidance while Coder
  writes the launch configuration; `app/styles.css` and `.vscode/launch.json`
  have no direct file dependency.
- Static checks for JSON syntax, required strings, and launch-file structure can
  be prepared independently of visual review.

### Work that must be sequential

- The Orchestrator must establish the schema and design handoff before Coder
  finalizes HTML, so the renderer and card styling do not diverge.
- JSON must be available before exercising the browser's data-loading path.
- HTML structure and CSS hooks must be aligned before visual review is complete.
- The launch configuration must be reviewed after the app path and target are
  known, and the integrated preview must run only after all four deliverables
  exist.
- Final validation and handoff are sequential after all parallel work is merged.

## Edge cases and implementation risks

- Invalid JSON or a missing `projects` array must produce a visible, accessible
  error state rather than a blank dashboard.
- A project with missing or empty fields must not create unlabeled badges or
  misleading content; either validate data and report the problem or render a
  clear fallback label.
- Long names and recent-activity summaries must wrap without breaking the card
  grid or causing horizontal scrolling.
- Status and priority must remain understandable in grayscale, by keyboard, and
  with assistive technology; color alone is insufficient.
- The page must work at narrow viewport widths and with browser text zoom.
- Fetching local JSON requires the HTTP server; do not treat a `file://` preview
  as proof that the app works.
- Port `5500` may already be occupied. Report that as an environment issue and
  use the configured port consistently rather than silently changing the launch
  contract.
- Avoid remote fonts, images, analytics, and third-party JavaScript so the
  dashboard remains deterministic and usable offline in the Codespace.

## Validation expectations

### Static validation

- Confirm all four assigned files exist:
  `app/index.html`, `app/styles.css`, `app/project-data.json`, and
  `.vscode/launch.json`.
- Parse both JSON files with a JSON parser; verify `project-data.json` has a
  top-level `projects` array and that every project has `name`, `owner`, `status`,
  `recentActivity`, and `priority`.
- Confirm `index.html` contains the exact `Project Pulse` title, references
  `styles.css` and `project-data.json`, contains `.project-card` markup, and
  exposes status, recent activity, and priority values.
- Confirm `styles.css` contains `.dashboard`, `.project-card`,
  `border-radius`, and `box-shadow`, plus responsive rules and visible focus
  treatment.
- Parse `.vscode/launch.json` as strict JSON and confirm it contains
  `Run Project Pulse Dashboard`, `python3 -m http.server 5500`, the app working
  directory, `serverReadyAction`, and `http://localhost:%s/index.html`.

### Runtime and UX validation

- Start the **Run Project Pulse Dashboard** configuration and confirm the
  browser opens `index.html`, not a directory listing.
- Confirm the page displays multiple project cards populated from JSON, with
  each required field visible and correctly labeled.
- Test a failed or malformed data load and confirm the user sees an informative
  error state.
- Resize through desktop, tablet, and mobile widths; check for no horizontal
  scrolling, readable spacing, and intact card hierarchy.
- Navigate with keyboard only and confirm logical focus order, visible focus,
  usable controls (if any), and no information available only on hover.
- Inspect contrast and text semantics, including headings, landmarks, badge
  labels, and status/priority meaning without color.
- Stop the preview server after the check and record any environment limitation
  rather than weakening the implementation contract.

## Handoff criteria

The Orchestrator may hand off Project Pulse when the four assigned files are
present, the data contract and launch configuration pass static checks, the
browser preview opens `index.html` through **Run Project Pulse Dashboard**, and
the responsive/accessibility review confirms that contributors can scan project
ownership, status, activity, and priority quickly.
