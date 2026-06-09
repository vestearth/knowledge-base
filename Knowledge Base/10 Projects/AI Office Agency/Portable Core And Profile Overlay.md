# Portable Core And Profile Overlay

## Decision

AI Office Agency keeps the framework core generic and moves project-specific behavior into profiles, templates, target project rules, or local config.

## Rule Of Thumb

- Core framework: portable defaults and stable contracts.
- Profile: project-family behavior.
- Template: installable starter files for target repos.
- Target project `AGENTS.md`: product-specific policy.
- Local config: machine-specific values and secrets.

## Config Merge Order

Later sources win:

1. `office.config.yaml`
2. selected profile config
3. `office.config.local.yaml`
4. environment variables
5. CLI flags

## Protected Fields

Normal profiles should not override framework identity or core state contracts such as:

- `office.version`
- `state_model.source_of_truth`
- `handoff_contract.state_files`
- `agents[].id`
- `runner_selector.config_dir`

## Source Files

- `/Users/earth/Documents/GitHub/ai-dev-office/AGENTS.md`
- `/Users/earth/Documents/GitHub/ai-dev-office/docs/config-profile-merge-contract.md`
- `/Users/earth/Documents/GitHub/ai-dev-office/profiles/README.md`
- `/Users/earth/Documents/GitHub/ai-dev-office/templates/install-manifest.yaml`

## Related

- [[Change Impact Analysis]]
- [[Current Decisions]]
- [[Architecture]]

