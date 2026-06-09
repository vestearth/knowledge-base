# AI Office Agency - Reusable Skills

This note captures repeatable work patterns before they become formal skills.

## [[Verification Loop]]

Pattern:
1. Inspect the source of truth.
2. Make the smallest scoped change.
3. Run the relevant verification.
4. Report evidence before claiming completion.

Useful for:
- dashboard work
- YAML contract changes
- portability docs
- role prompt updates

Sources:
- `/Users/earth/Documents/GitHub/ai-dev-office/README.md`
- `/Users/earth/Documents/GitHub/ai-dev-office/tests/integration/`

## [[Search First]]

Pattern:
1. Search the repo for the term, contract, route, or task id.
2. Open the source files before answering.
3. Separate source-backed facts from inference.

Useful for:
- status questions
- architecture questions
- task continuation
- rules and prompt boundary reviews

## [[Evidence Required]]

Pattern:
1. Treat outputs, tests, schemas, and docs as evidence surfaces.
2. Cite the exact artifact that supports a claim.
3. Avoid status labels that are not visible in source files.

Useful for:
- "done/partial/not yet" answers
- dashboard anomaly reviews
- task closure checks

## [[Change Impact Analysis]]

Pattern:
1. Identify the layer being changed: framework rule, role prompt, schema, workflow, dashboard, profile, or template.
2. Check adjacent contract surfaces.
3. Update tests or docs that define the contract.

Example:
- Adding a top-level portable doc may require README, SKILL, install manifest, and contract-foundation test alignment.

Sources:
- `/Users/earth/Documents/GitHub/ai-dev-office/README.md`
- `/Users/earth/Documents/GitHub/ai-dev-office/AGENTS.md`

## PM Handoff Contract

Pattern:
1. Use PM output shape instead of freeform summaries.
2. Include task, scope, acceptance criteria, plan, assignment, artifacts, next action, context sources, and blockers.
3. Keep it executable by downstream roles.

Source:
- `/Users/earth/Documents/GitHub/ai-dev-office/agents/pm.md`

## [[Portable Core And Profile Overlay|Portability Boundary Check]]

Pattern:
1. Ask whether the change belongs in portable framework core, target project policy, profile, template, or local config.
2. Keep machine-specific values and runtime history out of portable files.
3. Verify contract assumptions after changing portable docs/config.

Sources:
- `/Users/earth/Documents/GitHub/ai-dev-office/AGENTS.md`
- `/Users/earth/Documents/GitHub/ai-dev-office/docs/config-profile-merge-contract.md`

## Candidate Formal Skills

- Source-backed status audit
- AI Dev Office task continuation
- Dashboard analytics stabilization
- Portable framework contract review
- Obsidian project-note extraction
