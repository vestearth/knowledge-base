# Verification Loop

## Intent

Prevent the system from treating a claim as true before it has fresh evidence.

This is not only a testing habit. It is an operating pattern for AI-assisted work where task state, implementation, review, dashboard read models, and generated artifacts can drift apart.

## How To Use

Use this when you are about to say something is fixed, complete, approved, valid, or understood.

Do not use it as a ritual for tiny personal scratch notes that make no external claim.

After using it, your answer should include both the claim and the evidence that supports it.

## Reusable Pattern

Use this when a task could produce a false sense of completion.

1. Identify the source of truth.
2. Make or review the smallest scoped change.
3. Run the command that proves the claim.
4. Read the output and exit code.
5. Report the evidence before saying the work is complete.

## Skill-Quality Procedure

### 1. Name The Claim

Write the claim in plain language before verifying it.

Examples:
- "The dashboard build passes."
- "TASK-069 is closed."
- "The new framework doc is covered by the install contract."
- "The weird dashboard status is a read-model issue, not corrupted task state."

### 2. Identify The Proof

Choose the evidence that would actually prove the claim.

Examples:
- Build claim -> build command output.
- YAML validity claim -> `ruby validate-yaml.rb <task-id>`.
- Runtime status claim -> task artifact set in `runs/<task-id>/`.
- Framework contract claim -> README, SKILL entrypoint, install manifest, and integration test.
- Dashboard behavior claim -> server/client source plus focused test/build output.

### 3. Run Or Inspect Fresh Evidence

Do not rely on previous memory if the claim is current-state sensitive. Re-run the command or reopen the file.

### 4. Compare Evidence Against The Claim

If evidence is incomplete, say `partial`. If the evidence contradicts the claim, report the contradiction. Do not soften it into "probably".

### 5. Report In Evidence Form

Use this shape:

```text
Claim: ...
Evidence: ...
Result: done | partial | not yet | blocked
Remaining risk: ...
```

## Why It Exists

AI Office Agency depends on YAML handoffs, status files, role outputs, dashboard read models, and integration tests. A task can look complete from one artifact while another artifact still contradicts it.

## Use When

- Closing an AI Office task.
- Answering whether a dashboard issue is fixed.
- Updating framework rules, prompts, schemas, or workflows.
- Reviewing a task with generated handoff artifacts.

## Anti-Patterns

- Saying "done" because one agent output says success.
- Treating a passing build as proof that product behavior is correct.
- Treating a dashboard status as task truth without checking source artifacts.
- Editing a read model before checking whether the underlying artifacts are valid.
- Creating an ADR from memory without linking to the files that forced the decision.

## AI Office Examples

### Dashboard Analytics

Useful verification evidence:
- server tests
- server build
- client build
- analytics fixtures
- dashboard README/API contract

Related:
- [[Read-Only Dashboard Boundary]]
- [[Filesystem Source Of Truth]]

### Task Closure

Useful verification evidence:
- `status.yaml`
- `decision.yaml`
- `reviewer-output.yaml`
- `verification-evidence.md`
- `ruby validate-yaml.rb <task-id>`

Related:
- [[Evidence Required]]

### Framework Contract Change

Useful verification evidence:
- `README.md`
- `SKILL.md`
- `templates/install-manifest.yaml`
- `tests/integration/contract-foundation.sh`

Related:
- [[Change Impact Analysis]]

## Prompt Snippet

```text
Before answering, verify the claim from source.

1. State the claim being checked.
2. Identify the source-of-truth files or commands.
3. Inspect or run them.
4. Answer as done, partial, not yet, or blocked.
5. Include the exact evidence path.
```

## Evidence Sources

- `/Users/earth/Documents/GitHub/ai-dev-office/README.md`
- `/Users/earth/Documents/GitHub/ai-dev-office/tests/integration/`
- `/Users/earth/Documents/GitHub/ai-dev-office/validate-yaml.rb`
- `/Users/earth/Documents/GitHub/ai-dev-office/runs/<task-id>/`

## Related

- [[Evidence Required]]
- [[Search First]]
- [[Change Impact Analysis]]
- [[Filesystem Source Of Truth]]
