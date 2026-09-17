# Project Pulse implementation plan

## Summary

Mona's Project Pulse dashboard will be a lightweight static frontend that helps contributors scan active projects, ownership, status, recent activity, and priority. It will load project records from JSON, render a polished and responsive card-based interface, handle loading and data errors clearly, and run in a Codespace without a build step.

The Orchestrator will use this plan to assign non-overlapping file scopes to the Designer and Coder and sequence integration work where files or data contracts depend on one another.

## Implementation phases

### 1. Confirm scope and contracts

**Owner:** Orchestrator

Before implementation, confirm:

- The required outputs are `app/index.html`, `app/styles.css`, `app/project-data.json`, and `.vscode/launch.json`.
- The dashboard is a static application served over HTTP.
- `app/project-data.json` has a top-level `projects` array.
- Every project has `name`, `owner`, `status`, `recentActivity`, and `priority`.
- Shared HTML and CSS hooks include `.dashboard` and `.project-card`.
- The Codespace launch configuration opens `index.html`, not a directory listing.

### 2. Define the UI system

**Owner:** Designer  
**File assignment:** `app/styles.css`  
**Advisory scope:** Structure and accessibility guidance for `app/index.html`

The Designer will:

- Define the information hierarchy for the heading, dashboard summary, project grid, project names, owners, status badges, priority treatments, and recent activity.
- Create a polished visual system with readable typography, spacing, contrast, rounded cards, shadows, and responsive behavior.
- Define deterministic CSS hooks for the dashboard, cards, statuses, priorities, loading state, error state, and empty state.
- Ensure status and priority remain understandable without relying on color alone.
- Review the integrated markup for semantic structure, accessibility, and responsive behavior.

The Designer owns `app/styles.css`. Any requested markup changes must be handed back to the Coder sequentially to avoid overlapping edits to `app/index.html`.

### 3. Create the project data

**Owner:** Coder  
**File assignment:** `app/project-data.json`

The Coder will:

- Create strict, valid JSON with a top-level `projects` array.
- Add multiple realistic project records demonstrating different statuses and priorities.
- Include `name`, `owner`, `status`, `recentActivity`, and `priority` in every record.
- Avoid comments, trailing commas, and non-JSON syntax.

This phase establishes the data contract consumed by `app/index.html`.

### 4. Implement dashboard structure and behavior

**Owner:** Coder  
**File assignment:** `app/index.html`  
**Dependencies:** Phase 1 contracts, Phase 2 CSS hooks, and Phase 3 data shape

The Coder will:

- Create semantic page structure with a visible `Project Pulse` title and a `.dashboard` container.
- Reference `styles.css` and fetch `project-data.json`.
- Render a `.project-card` for every project.
- Display each project's name, owner, status, recent activity, and priority.
- Provide explicit loading, fetch/parsing error, invalid-data, and empty states.
- Handle missing project fields with readable fallback text instead of breaking all rendering.
- Render unknown status or priority values using a default visual treatment.
- Keep behavior deterministic and dependency-free.

### 5. Complete visual styling

**Owner:** Designer  
**File assignment:** `app/styles.css`  
**Dependency:** Stable markup and class hooks from Phase 4

The Designer will:

- Style `.dashboard` and `.project-card` as a responsive card grid.
- Apply clear typography, spacing, contrast, `border-radius`, and `box-shadow`.
- Style status badges and priority indicators with text and visual differentiation.
- Style loading, error, invalid-data, and empty states.
- Ensure the first view looks like a finished dashboard rather than a bare HTML page.
- Check narrow and wide viewport behavior.

### 6. Configure Codespace launch behavior

**Owner:** Coder  
**File assignment:** `.vscode/launch.json`

The Coder will:

- Create strict JSON with no comments.
- Add a configuration named `Run Project Pulse Dashboard`.
- Run `python3 -m http.server 5500`.
- Set `cwd` to `${workspaceFolder}/app`.
- Use `serverReadyAction` to open `http://localhost:%s/index.html`.
- Ensure the learner sees the dashboard instead of a directory listing.

### 7. Integrate and validate

**Owner:** Orchestrator  
**Files reviewed:** All four assigned files

The Orchestrator will:

- Confirm the JSON contract, markup, CSS hooks, and launch configuration agree.
- Confirm each specialist stayed within the assigned file scope.
- Route any HTML changes identified by the Designer back to the Coder before final CSS review.
- Run the validation expectations below and resolve integration failures with the relevant specialist.

## File assignments

| File | Primary owner | Responsibility |
| --- | --- | --- |
| `app/index.html` | Coder | Semantic dashboard markup, JSON loading, project-card rendering, accessible content, and loading/error/empty behavior. |
| `app/styles.css` | Designer | Visual system, `.dashboard` and `.project-card` hooks, status and priority treatments, state styling, accessibility, and responsive layout. |
| `app/project-data.json` | Coder | Strict JSON data contract and representative project records with all required fields. |
| `.vscode/launch.json` | Coder | Deterministic Codespace launch configuration serving `app/` and opening `index.html`. |

## Dependencies and sequencing

1. The Orchestrator must confirm the file scopes, required fields, and shared CSS hooks before implementation.
2. `app/project-data.json` defines the contract used by `app/index.html`; the contract must be stable before final rendering logic is validated.
3. The Designer must define the core class hooks before HTML/CSS integration.
4. `app/index.html` must expose the agreed hooks before the Designer completes `app/styles.css`.
5. The Designer's final accessibility and hierarchy review follows the Coder's markup implementation.
6. If that review requires HTML changes, the Coder makes them before the Designer performs the final CSS pass.
7. Full integration validation requires all four files.

## Parallel work decisions

The Orchestrator may run these tasks in parallel:

- **Designer UI direction and Coder JSON data:** The work uses separate files and only needs the agreed fields and hooks.
- **Designer CSS implementation and Coder launch configuration:** `app/styles.css` and `.vscode/launch.json` do not overlap.
- **Coder JSON data and launch configuration:** The launch behavior does not depend on project content.

These tasks must run sequentially:

- Planning and contract confirmation before implementation.
- Data-contract agreement before final HTML data handling.
- Core CSS-hook agreement before final HTML/CSS integration.
- Coder markup implementation before the Designer's final review.
- Any edits requiring both agents to influence the same file.
- Orchestrator integration review after all assigned files exist.

Parallel tasks must always have explicit, non-overlapping file ownership. The Orchestrator should not allow Designer and Coder to modify `app/index.html` concurrently.

## Edge cases, risks, and assumptions

The implementation must account for:

- A failed fetch or invalid JSON by showing a clear error instead of a blank page.
- A missing or non-array `projects` value by showing an invalid-data state.
- An empty `projects` array by showing a friendly empty state.
- Missing fields within one record by using safe fallback text without preventing other cards from rendering.
- Unknown status or priority values by retaining readable text and applying default styling.
- Browser restrictions when opening the HTML through `file://`; normal use must go through the local HTTP server.
- Port `5500` already being in use.
- Mismatched HTML and CSS class names.
- Insufficient contrast or color-only communication.
- Strict JSON requirements for both JSON files.

Assumptions:

- No external frameworks, packages, or build tooling are required.
- JavaScript for fetching and rendering data may live in `app/index.html`.
- Python 3 is available in the Codespace.
- The learner controls staging, commits, and pushes.

## Validation expectations

### Structure and integration

- All four assigned files exist.
- `app/index.html` visibly includes `Project Pulse`.
- `app/index.html` references `styles.css` and `project-data.json`.
- Rendered projects use the `project-card` class.
- `app/styles.css` includes `.dashboard`, `.project-card`, `border-radius`, and `box-shadow`.

### Data and behavior

- `python3 -m json.tool app/project-data.json` succeeds.
- The top-level `projects` value is an array.
- Every sample record contains `name`, `owner`, `status`, `recentActivity`, and `priority`.
- Project cards render from the JSON data when served over HTTP.
- Loading, fetch/parsing error, invalid-data, and empty states are observable.
- A record with a missing field does not stop other projects from rendering.

### UI, accessibility, and responsiveness

- The initial view clearly reads as a polished Project Pulse dashboard.
- Cards, statuses, priorities, owners, and recent activity are easy to scan.
- Status and priority are conveyed with text, not color alone.
- Text and controls have readable contrast and sizing.
- Semantic landmarks and heading order are understandable to assistive technology.
- The layout works at narrow mobile and wide desktop viewport sizes.

### Codespace launch

- `python3 -m json.tool .vscode/launch.json` succeeds.
- The configuration name is exactly `Run Project Pulse Dashboard`.
- The command is `python3 -m http.server 5500`.
- The working directory is `${workspaceFolder}/app`.
- `serverReadyAction` opens `http://localhost:%s/index.html`.
- Starting the configuration opens the dashboard rather than a directory listing.
- The local server is stopped after validation.

## Open questions

No unresolved questions. This plan assumes the repository brief and custom agent definitions are authoritative and that the dashboard remains a static HTML, CSS, and JSON application.
