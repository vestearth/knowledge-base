# Change Impact Analysis

## Reusable Pattern

Before changing AI Office Agency, identify which layer owns the behavior and which neighboring contracts must move with it.

## Layers

- Framework rules: `AGENTS.md`
- User docs: `README.md`, `docs/`
- Role behavior: `agents/*.md`
- Runtime flow: `workflows/*.yaml`
- Data contract: `schemas/*.yaml`
- Validation: `validate-yaml.rb`
- Target install boundary: `templates/install-manifest.yaml`
- Profiles: `profiles/*.yaml`
- Dashboard read model: `dashboard/server`, `dashboard/client`, `dashboard/shared`

## Checklist

1. Name the layer being changed.
2. Check whether the behavior is portable core, profile overlay, target-project policy, or local config.
3. Update adjacent docs/tests/contracts that define the same behavior.
4. Run the smallest verification command that proves the layer still works.

## Example

Adding a top-level portable doc may also require:

- README index update
- SKILL entrypoint update
- install manifest update
- contract-foundation integration test update

## Related

- [[Portable Core And Profile Overlay]]
- [[Verification Loop]]
- [[Evidence Required]]

