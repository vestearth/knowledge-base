# ADR-0001 Filesystem Source Of Truth

## Status

Approved / operating decision

## Context

AI Office Agency coordinates work through role prompts, YAML handoffs, task status, decisions, validation, and dashboard read models.

The project needs a source of truth that is easy to inspect, portable across target repos, friendly to local development, and simple enough to operate before the framework has a larger multi-user control plane.

The current project already stores runtime task state under `runs/<task-id>/` and exposes dashboard monitoring by reading those files.

## Decision

Use filesystem artifacts as the primary source of truth for AI Office Agency runtime state.

This includes:

- task scope and context
- `status.yaml`
- agent outputs
- reviewer outputs
- `decision.yaml`
- validation evidence
- dashboard read models derived from `runs/`

Do not introduce a database as the default runtime state layer yet.

## Consequences

### Benefits

- Operators can inspect task state directly.
- Runtime state stays simple and local-first.
- Framework portability remains easier.
- Dashboard can stay read-only while the workflow stabilizes.
- Task artifacts can double as debugging and handoff evidence.

### Tradeoffs

- File schema discipline becomes important.
- Concurrent writes need explicit protection.
- Dashboard read models can become subtle when artifacts disagree.
- Analytics may need stronger caching or indexing later if filesystem scans become expensive.
- Historical task folders can accumulate noise if not summarized into reusable knowledge.

## Follow-Ups

- Keep dashboard writes out of scope until read models are stable.
- Treat status anomalies as artifact/read-model questions before editing data.
- Promote repeated task lessons into reusable notes or skills.
- Revisit persistence only when a concrete use case requires multi-user control, query performance, audit indexing, or remote access.

## Source Files

- `/Users/earth/Documents/GitHub/ai-dev-office/README.md`
- `/Users/earth/Documents/GitHub/ai-dev-office/dashboard/README.md`
- `/Users/earth/Documents/GitHub/ai-dev-office/workflows/hybrid-default.yaml`
- `/Users/earth/Documents/GitHub/ai-dev-office/docs/config-profile-merge-contract.md`

## Related

- [[Filesystem Source Of Truth]]
- [[Read-Only Dashboard Boundary]]
- [[Evidence Required]]
- [[Verification Loop]]
- [[Current Decisions]]
