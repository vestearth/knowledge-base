# AI Office Agency - Architecture

## System Shape

AI Office Agency is a file-backed orchestration framework. Its core loop is role prompts plus YAML artifacts plus a driver that advances task state.

```text
Framework rules/config
  -> role prompts
  -> task artifacts in runs/<task-id>/
  -> validator and decision reconcile
  -> dashboard read models
```

## Runtime Pipeline

Primary path:

```text
pending
  -> pm
  -> assigned or assigned_parallel
  -> dev/dev-2
  -> in_review
  -> reviewer
  -> done
```

Escalation paths:

```text
changes_requested -> debugger -> reviewer or dev/dev-2
infra_failure -> devops -> reviewer or dev/dev-2
unclear/stuck/conflict -> free-roam -> dev/dev-2, pm, dynamic, or aborted
```

Source:
- `/Users/earth/Documents/GitHub/ai-dev-office/workflows/hybrid-default.yaml`

## Main Components

### Framework Contract

- `AGENTS.md`: portable framework rules
- `README.md`: framework overview and documentation index
- `office.config.example.yaml`: generic configuration surface
- `templates/install-manifest.yaml`: install boundary for target projects

### Roles

- `agents/pm.md`: task slicing and acceptance criteria
- `agents/dev.md`: implementation lane
- `agents/dev-2.md`: alternate/cross-service lane
- `agents/reviewer.md`: review verdict and release risk
- `agents/debugger.md`: root cause analysis and fix routing
- `agents/devops.md`: infra and CI/CD route
- `agents/free-roam.md`: ambiguity and stuck-pipeline handling

### Runtime State

- `runs/<task-id>/task.md`
- `runs/<task-id>/status.yaml`
- `runs/<task-id>/<agent>-output.yaml`
- `runs/<task-id>/decision.yaml`
- `runs/<task-id>/meta.yaml`

### Contracts And Validation

- `schemas/`: expected YAML shapes
- `validate-yaml.rb`: runtime validation
- integration tests: contract, decision reconcile, output contract, concurrent status writes, state-machine consistency

Sources:
- `/Users/earth/Documents/GitHub/ai-dev-office/README.md`
- `/Users/earth/Documents/GitHub/ai-dev-office/schemas/`

### Profiles And Portability

Profiles layer project-specific behavior over the generic framework. Load order gives later sources more authority, while protected fields preserve framework contract stability.

Source:
- `/Users/earth/Documents/GitHub/ai-dev-office/docs/config-profile-merge-contract.md`

### Dashboard

Dashboard is a read-only React/Express app over the filesystem:

- `Monitor`: task browsing, timeline, log tail
- `Analytics`: health, failure clusters, trends, long-running work, agent activity
- `Reports`: summary snapshot

Source:
- `/Users/earth/Documents/GitHub/ai-dev-office/dashboard/README.md`

## Architecture Notes To Watch

- If runtime state becomes too large for direct filesystem scans, analytics may need a stronger read model.
- If profiles start overriding protected fields, the profile is probably becoming a forked framework.
- If role prompts duplicate framework rules, the rule boundary needs cleanup.

