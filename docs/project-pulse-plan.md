# Project Pulse dashboard implementation plan

## Goal

Build a lightweight static dashboard that lets contributors scan project owners, status, recent activity, priority or risk, and a short summary. Use the existing repository brief and conventions; keep the implementation in HTML, CSS, and JSON without adding a frontend framework or package setup. Serve the page over HTTP so it can load its JSON data.

## Responsibilities and file assignments

| File or work | Owner | Assignment |
|---|---|---|
| Design guidance (no implementation-file edits) | Designer | Recommend information hierarchy, visual direction, responsive behavior, accessibility, and stable CSS hooks such as `.dashboard` and `.project-card`. Make status, priority, and ownership easy to scan; ensure labels do not rely on color alone. |
| `app/project-data.json` | Coder | Add representative projects in a top-level `projects` array. Each project includes `name`, `owner`, `status`, `recentActivity`, and `priority`, plus a short `summary` to meet the brief. Use varied values for the visual states. |
| `app/styles.css` | Coder | Implement the responsive card layout and visual system, following Designer guidance. Keep text readable and status/priority treatments accessible; include the agreed CSS hooks. |
| `app/index.html` | Coder | Create the page titled **Project Pulse**, reference the stylesheet and JSON data, and render project cards with all supplied fields. Include clear loading, empty, and error states. Keep any necessary data-loading logic in this page because no separate JavaScript file is in scope. |
| `.vscode/launch.json` | Coder | Add the **Run Project Pulse Dashboard** launch configuration using `python3 -m http.server 5500`, working directory `${workspaceFolder}/app`, and a `serverReadyAction` that opens `http://localhost:%s/index.html`. Keep the file valid JSON with no comments. |

Coder is the sole writer for the four implementation files. Designer provides recommendations only, avoiding overlapping edits.

## Dependencies and sequence

1. **Agree on the contract.** Confirm the data shape, status/priority vocabulary, and CSS hooks. The JSON includes the five required fields and `summary` for the contributor-friendly overview.
2. **Parallel work.** While Designer prepares visual and accessibility guidance, Coder can create `app/project-data.json` and `.vscode/launch.json` using the agreed schema and launch requirements. These do not depend on visual styling.
3. **Implement styling.** Coder builds `app/styles.css` using Designer’s guidance and the agreed selectors.
4. **Integrate the page.** Coder completes `app/index.html` against the finalized data schema and CSS hooks. This depends on steps 1 and 3.
5. **Integrate and validate.** After all four files exist, Coder verifies the data, page, and launch configuration together; the Orchestrator checks the result against this plan and the repository’s Step 3 requirements.

The page must be loaded over HTTP, not `file://`, because browsers may block local JSON fetches. Keep each file’s ownership explicit; do not have multiple workers edit the same file concurrently.

## Edge cases and risks

- Show a clear message if fetching data fails, the data is missing or unusable, or `projects` is empty.
- Render project-provided text as text, not injected HTML. Long names, activity, and summaries should not break the layout.
- Keep unknown status or priority values readable and identifiable without relying only on color.
- Port `5500` may be occupied; report the conflict and stop the preview server after testing.
- Confirm that the available VS Code debugger supports the launch configuration and server startup behavior.
- Python’s standard-library HTTP server is suitable for local preview, not production hosting.

## Validation expectations

1. Parse both JSON files:
   - `python3 -m json.tool app/project-data.json`
   - `python3 -m json.tool .vscode/launch.json`
2. Confirm `projects` is an array and every project includes `name`, `owner`, `status`, `recentActivity`, and `priority`.
3. Review the HTML and CSS for required references and hooks, all visible project fields, responsive behavior, and accessible labeling.
4. Run **Run Project Pulse Dashboard** in VS Code. Confirm it serves from `app/` and opens `http://localhost:5500/index.html`, not a directory listing.
5. In a browser, verify the project cards and supplied data, empty/error states, a narrow viewport, and keyboard navigation. Stop the preview server afterward.
6. Confirm the plan names all four required files and records Designer/Coder responsibilities, dependencies, parallel/sequential decisions, and these validation expectations. The repository’s Step 2 checks planning references; Step 3 checks implementation files and JSON syntax.
