# AI Office Agency - Project Map

## Purpose

AI Office Agency is a portable multi-agent orchestration framework for turning requests into task artifacts, agent handoffs, implementation, review, debugging, and operational follow-up.

This note is the starting map for the Obsidian vault. Keep it practical: link to real project files first, then split into ADRs, skills, prompts, or system graph notes only when a repeated pattern appears.

## Source Files

- Framework index: `/Users/earth/Documents/GitHub/ai-dev-office/README.md`
- Framework rules: `/Users/earth/Documents/GitHub/ai-dev-office/AGENTS.md`
- Skill entrypoint: `/Users/earth/Documents/GitHub/ai-dev-office/SKILL.md`
- Workflow: `/Users/earth/Documents/GitHub/ai-dev-office/workflows/hybrid-default.yaml`
- Active config: `/Users/earth/Documents/GitHub/ai-dev-office/office.config.yaml`
- Example config: `/Users/earth/Documents/GitHub/ai-dev-office/office.config.example.yaml`

## Main Areas

- Agents: `pm`, `dev`, `dev-2`, `reviewer`, `debugger`, `devops`, `free-roam`
- Workflows: PM-driven task creation, implementation lanes, review gate, debugger/devops/free-roam escalation
- Runtime state: `runs/<task-id>/status.yaml`, task docs, role outputs, reviewer decisions, meta events
- Contracts: YAML schemas, validation, decision reconcile, status transitions
- Portability: profiles, templates, install manifest, local override boundaries
- Dashboard: read-only monitoring and analytics over `runs/`
- Prompting: role prompts, routing rules, manual advisory lanes

## Current Mental Model

```text
User Request
  -> PM task shaping
  -> Dev or Dev-2 implementation
  -> Reviewer gate
  -> Done

Exceptions:
  reviewer rejects -> debugger
  infra failure -> devops
  unclear/stuck/conflict -> free-roam
```

## Related Notes

- [[Architecture]]
- [[Current Decisions]]
- [[Open Questions]]
- [[Lessons Learned]]
- [[Prompt Library]]
- [[Reusable Skills]]

