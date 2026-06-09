# Filesystem Source Of Truth

## Decision

AI Office Agency treats filesystem artifacts as the primary runtime state. Task state, handoffs, decisions, and dashboard analytics are read from files rather than a database.

Formal decision note: [[ADR-0001 Filesystem Source Of Truth]]

## Current Shape

- Runtime task state: `runs/<task-id>/`
- Status: `status.yaml`
- Role outputs: `<agent>-output.yaml`
- Decisions: `decision.yaml`
- Validation: `validate-yaml.rb`
- Dashboard reads: `dashboard/server`

## Why This Is Useful

- Easy to inspect and debug.
- Git-friendly for framework docs and templates.
- Simple portability model.
- No database setup required for local operation.
- Dashboard can stay read-only and low-risk.

## Tradeoffs

- Read models can become subtle when multiple artifacts disagree.
- Analytics can become expensive if scans grow.
- Concurrency needs explicit protection.
- File naming and schema discipline matter more.

## Source Files

- `/Users/earth/Documents/GitHub/ai-dev-office/README.md`
- `/Users/earth/Documents/GitHub/ai-dev-office/dashboard/README.md`
- `/Users/earth/Documents/GitHub/ai-dev-office/workflows/hybrid-default.yaml`

## Related

- [[Read-Only Dashboard Boundary]]
- [[Evidence Required]]
- [[Current Decisions]]
- [[ADR-0001 Filesystem Source Of Truth]]
