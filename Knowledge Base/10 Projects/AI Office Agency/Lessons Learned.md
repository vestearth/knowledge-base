# AI Office Agency - Lessons Learned

Use this note for field observations before they harden into ADRs, reusable skills, or project rules.

## Start From Work Artifacts, Not Taxonomy

This vault should grow from real AI Office Agency tasks. Folder structure and metadata should follow observed note pressure, not precede it.

Practice:
- Capture a real decision, prompt, failure, or workflow.
- Link it to the source artifact.
- Only extract a template after the pattern repeats.

## Fixture-First Stabilization Works For Analytics

Dashboard analytics become safer when behavior is pinned with fixtures and focused tests before expanding UI or API shape.

Related areas:
- `dashboard/server/src/services/analytics.fixtures.ts`
- `dashboard/server/src/services/analytics.test.ts`
- `/Users/earth/Documents/GitHub/ai-dev-office/dashboard/README.md`

## Read Model Semantics Are Not Data Corruption

A task can look strange in the dashboard because the read model interprets multiple artifacts together. Inspect `status.yaml`, `decision.yaml`, and `reviewer-output.yaml` before editing source task state.

Useful habit:
- Treat weird dashboard status as a read-model question first.
- Validate task artifacts before changing the model.

## Portability Needs Explicit Negative Space

The install contract is as much about what not to copy as what to copy. Runtime history, logs, generated handoff outputs, local config, secrets, and machine-specific files should stay out of target installs.

Sources:
- `/Users/earth/Documents/GitHub/ai-dev-office/AGENTS.md`
- `/Users/earth/Documents/GitHub/ai-dev-office/templates/install-manifest.yaml`

## Manual Advisory Lanes Need Human Boundary Language

Claude and Gemini can help as second-opinion lanes, but their outputs remain advisory until normalized, validated, and accepted through AI Dev Office artifacts.

Source:
- `/Users/earth/Documents/GitHub/ai-dev-office/model-routing-codex-first.md`

## Integration Tests Are Living Architecture

The integration test list in `README.md` documents important system guarantees: dependency guards, runner fallback, concurrent writes, validation failure bounds, decision reconcile, state-machine consistency, and observability.

Source:
- `/Users/earth/Documents/GitHub/ai-dev-office/README.md`

## Candidate Extractions

- ADR: filesystem-backed task state
- ADR: read-only dashboard boundary
- Skill: verification loop before completion
- Skill: evidence-required status answering
- Skill: source-first project map creation
- Rule: keep framework defaults generic and push project assumptions to profiles

