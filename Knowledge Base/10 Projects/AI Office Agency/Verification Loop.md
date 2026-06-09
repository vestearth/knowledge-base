# Verification Loop

## Reusable Pattern

Use this when a task could produce a false sense of completion.

1. Identify the source of truth.
2. Make or review the smallest scoped change.
3. Run the command that proves the claim.
4. Read the output and exit code.
5. Report the evidence before saying the work is complete.

## Why It Exists

AI Office Agency depends on YAML handoffs, status files, role outputs, dashboard read models, and integration tests. A task can look complete from one artifact while another artifact still contradicts it.

## Use When

- Closing an AI Office task.
- Answering whether a dashboard issue is fixed.
- Updating framework rules, prompts, schemas, or workflows.
- Reviewing a task with generated handoff artifacts.

## Evidence Sources

- `/Users/earth/Documents/GitHub/ai-dev-office/README.md`
- `/Users/earth/Documents/GitHub/ai-dev-office/tests/integration/`
- `/Users/earth/Documents/GitHub/ai-dev-office/validate-yaml.rb`
- `/Users/earth/Documents/GitHub/ai-dev-office/runs/<task-id>/`

## Related

- [[Evidence Required]]
- [[Search First]]
- [[Change Impact Analysis]]

