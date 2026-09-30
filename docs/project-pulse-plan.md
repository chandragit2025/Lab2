# Project Pulse Dashboard Implementation Plan

## Goal

Build a small, static Project Pulse dashboard that helps contributors understand project names, owners, status, recent activity, priority or risk, and short summaries. Follow `.github/project-pulse-brief.md` and the repository's Step 3 expectations: a polished, accessible card-based UI, data loaded from JSON, and a VS Code launch configuration that opens the dashboard rather than a directory listing. No framework or build tooling is needed.

## Responsibilities and file assignments

| Owner | Assignment |
|---|---|
| **Planner** | Research repository conventions and the Project Pulse brief; document the assignments, dependencies, ordering, and validation in this plan. No app files. |
| **Designer** | Define the information hierarchy, visual direction, responsive behavior, and accessibility guidance. Provide the design decisions to Coder; do not edit Coder-owned files. |
| **Coder** | Implement and integrate all four assigned files, following Designer's guidance and the fixed data/runtime requirements below. |
| **Orchestrator** | Coordinate the sequence, keep file ownership clear, check the integrated result, and report validation and any remaining issues. |

| File | Implementation assignment |
|---|---|
| `app/index.html` | **Coder.** Create the page with the exact title “Project Pulse”; link `styles.css`; load `project-data.json`; render visible project cards with project name, owner, status, recent activity, priority, and summary. Use semantic, accessible markup and the agreed class hooks, including `project-card`. Include a visible loading or error message if the JSON cannot be loaded. |
| `app/styles.css` | **Coder**, guided by **Designer**. Implement the visual direction, readable hierarchy and spacing, responsive card layout, visible status/priority treatments, and accessible contrast and focus states. Include `.dashboard` and `.project-card`, plus rounded cards (`border-radius`) and subtle depth (`box-shadow`). |
| `app/project-data.json` | **Coder.** Provide valid JSON with a top-level `projects` array and multiple representative project records. Each record must include `name`, `owner`, `status`, `recentActivity`, and `priority`; include a concise contributor-friendly `summary` as required by the brief. |
| `.vscode/launch.json` | **Coder.** Create strict JSON with no comments and a configuration named **Run Project Pulse Dashboard**. Serve from `${workspaceFolder}/app` using `python3 -m http.server 5500`; configure readiness handling to open `http://localhost:%s/index.html`, so VS Code opens the UI rather than the app directory listing. |

## Dependencies and work order

1. **Confirm the contract.** Orchestrator and Coder use the brief as the source of truth for required fields, title, launch name, command, working directory, and URL. Designer establishes visual and accessibility guidance and the HTML/CSS class hooks.
2. **Parallel work:** Designer can prepare the design guidance while Coder creates `app/project-data.json` and `.vscode/launch.json`. These tasks have separate ownership and rely on requirements already fixed in the brief.
3. **Sequence the UI implementation after design guidance and the data schema are settled.** Coder builds `app/index.html` and `app/styles.css` together so the markup, class hooks, and styling agree. The HTML renderer depends on the JSON schema; the CSS depends on the agreed markup hooks.
4. **Integrate and validate after all four files are in place.** Cross-file checks and the running preview must be performed on the integrated result, not inferred from individual files.

Do not have Designer and Coder edit the same file. If the design requires changing markup hooks or data fields, agree on the change first, then have Coder update the related files together.

## Validation and acceptance

- **Assigned files:** Confirm `app/index.html`, `app/styles.css`, `app/project-data.json`, and `.vscode/launch.json` exist at the specified paths.
- **JSON:** Run `python3 -m json.tool app/project-data.json` and `python3 -m json.tool .vscode/launch.json`; both must parse successfully. Confirm every project record has all required fields and the top-level `projects` key exists.
- **Wiring and required content:** Confirm the HTML title is exactly “Project Pulse”, it references `styles.css` and `project-data.json`, and the rendered card markup uses `project-card` and exposes status, recent activity, and priority. Confirm the stylesheet contains `.dashboard`, `.project-card`, `border-radius`, and `box-shadow`. Confirm the launch file contains the required configuration name and `index.html` target.
- **Repository check:** The Step 2 workflow (`.github/workflows/2-step.yml`) checks that the plan exists and includes Project Pulse, Designer, Coder, the four file assignments, dependencies, parallel work, and validation. The Step 3 workflow (`.github/workflows/3-step.yml`) checks the app files, required content and style hooks, JSON validity, and launch configuration. These checks complement—but do not replace—the runtime preview.
- **Behavior:** In VS Code, select **Run Project Pulse Dashboard** and start it. Confirm the browser opens `http://localhost:5500/index.html` and displays the dashboard, not a directory listing; project cards must show the values from the JSON file. Stop the server after checking.
- **Experience:** Review the rendered page at desktop and narrow viewport widths; confirm content remains readable and cards reflow without horizontal overflow. Check semantic structure, visible keyboard focus, and sufficient contrast. Confirm a failed data request produces an understandable visible error instead of an empty or success-shaped dashboard.

## Completion criteria

The four assigned files exist and are connected; the dashboard renders the JSON-backed project information in a responsive, polished UI; both JSON files parse; and the **Run Project Pulse Dashboard** configuration opens `index.html` successfully.
