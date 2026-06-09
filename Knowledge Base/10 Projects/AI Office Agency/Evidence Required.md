# Evidence Required

## Reusable Pattern

Do not answer status from vibes, memory, or one artifact alone. Tie claims to files, test output, run artifacts, or source inspection.

## Claim Types

- Done: show the task artifact, decision, status, test/build output, or commit that proves it.
- Partial: name what is complete and what still lacks evidence.
- Not yet: point to the missing artifact, missing test, missing route, or open decision.
- Inference: label it as inference and say what would verify it.

## AI Office Agency Application

For task status, inspect the artifact set together:

- `runs/<task-id>/task.md`
- `runs/<task-id>/status.yaml`
- `runs/<task-id>/pm-output.yaml`
- `runs/<task-id>/dev-output.yaml` or `dev-2-output.yaml`
- `runs/<task-id>/reviewer-output.yaml`
- `runs/<task-id>/decision.yaml`
- `runs/<task-id>/verification-evidence.md`

## Why It Matters

Dashboard status, reviewer verdict, and task phase can describe different layers of reality. The answer should say which layer is being discussed.

## Related

- [[Verification Loop]]
- [[Search First]]
- [[Read-Only Dashboard Boundary]]

