# Read-Only Dashboard Boundary

## Decision

The AI Office Agency dashboard observes task state and analytics but does not control tasks.

## Current Views

- Monitor: browse runs, inspect task details, review timeline, and tail logs.
- Analytics: workflow health, failure clusters, trends, long-running work, and agent activity.
- Reports: lightweight snapshot summary.

## Why This Boundary Helps

- Keeps dashboard risk low.
- Avoids coupling UI actions to orchestration before the read model is stable.
- Makes filesystem-based inspection the first-class behavior.
- Lets analytics mature before write/control workflows are introduced.

## Known Limitations

- No task control actions.
- No auth or persistence layer.
- Analytics recomputes from filesystem data.
- Some panels fetch separate endpoints.
- Reports is not yet a full markdown report generator.

## Source Files

- `/Users/earth/Documents/GitHub/ai-dev-office/dashboard/README.md`
- `/Users/earth/Documents/GitHub/ai-dev-office/dashboard/server/`
- `/Users/earth/Documents/GitHub/ai-dev-office/dashboard/client/`
- `/Users/earth/Documents/GitHub/ai-dev-office/dashboard/shared/`

## Related

- [[Filesystem Source Of Truth]]
- [[Evidence Required]]
- [[Architecture]]

