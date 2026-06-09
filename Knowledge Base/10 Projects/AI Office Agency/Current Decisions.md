# AI Office Agency - Current Decisions

This note captures decisions already visible in the project. Promote an item into a dedicated ADR only when it starts affecting multiple files, projects, or future choices.

## Approved Or Operating Decisions

### Filesystem Is The Source Of Truth

Runtime state lives in `runs/<task-id>/` and dashboard analytics read from those files. The dashboard currently has no database or persistence layer.

Sources:
- `/Users/earth/Documents/GitHub/ai-dev-office/README.md`
- `/Users/earth/Documents/GitHub/ai-dev-office/dashboard/README.md`

### Portable Framework Core, Project-Specific Profiles

The framework core stays generic. Project behavior goes through profiles, local config, target project `AGENTS.md`, or templates.

Sources:
- `/Users/earth/Documents/GitHub/ai-dev-office/AGENTS.md`
- `/Users/earth/Documents/GitHub/ai-dev-office/docs/config-profile-merge-contract.md`
- `/Users/earth/Documents/GitHub/ai-dev-office/profiles/README.md`

### Config Merge Uses Later Sources As Stronger Overrides

Load order is `office.config.yaml`, selected profile, local config, environment variables, then CLI flags. Protected framework fields should not be changed by normal profiles.

Source:
- `/Users/earth/Documents/GitHub/ai-dev-office/docs/config-profile-merge-contract.md`

### Codex-First Runtime

Automated roles default to Codex. Cursor runners are fallbacks for implementation lanes. Claude and Gemini are documented as manual advisory lanes, not automated runners.

Source:
- `/Users/earth/Documents/GitHub/ai-dev-office/model-routing-codex-first.md`

### Reviewer And Debugger Gates Stay In The Workflow

Fallback runners and manual advisory lanes do not replace reviewer/debugger gates. Outputs remain draft until normalized into AI Dev Office artifacts and validated.

Source:
- `/Users/earth/Documents/GitHub/ai-dev-office/model-routing-codex-first.md`

### Dashboard Is Read-Only

Dashboard views inspect tasks, logs, metrics, and reports. They do not control tasks.

Source:
- `/Users/earth/Documents/GitHub/ai-dev-office/dashboard/README.md`

### Parallel Work Is Selective

The workflow supports PM-split parallel lanes with `dev` and `dev-2`, but the main pipeline remains PM -> Dev/Dev-2 -> Reviewer -> Done.

Source:
- `/Users/earth/Documents/GitHub/ai-dev-office/workflows/hybrid-default.yaml`

## Candidates For ADR

- Filesystem source of truth vs database-backed state
- Codex-first with manual advisory lanes
- Portable core plus profile overlays
- Read-only dashboard as first dashboard boundary
- Selective parallel execution instead of default parallelism

