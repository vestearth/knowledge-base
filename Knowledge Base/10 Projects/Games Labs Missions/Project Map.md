# Games Labs Missions - Project Map

## Purpose

`Games-Labs-Missions` is one of the highest-pressure product repos in this workspace.

It owns missions, check-in, streak/restore, mission boost, store-related reward flows, level progression hooks, and the Daily Activities consumer behavior that turns events into user-facing mission progress.

## Source Files

- `/Users/earth/Documents/GitHub/Games-Labs-Missions/README.md`
- `/Users/earth/Documents/GitHub/Games-Labs-Missions/internal/`
- `/Users/earth/Documents/GitHub/Games-Labs-Missions/docs/`
- `/Users/earth/Documents/GitHub/shared-lib/proto/missionspb/missions.proto`
- `/Users/earth/Documents/GitHub/docs/Games-Labs-APIs.postman_collection.json`
- `/Users/earth/Documents/GitHub/AGENTS.md`

## Main Areas

- client-facing missions/check-in/streak APIs
- admin APIs for missions config and Daily Activities config
- event-driven Daily Activities progress via `player.activity.v1`
- reward/store/pass integration
- level progression and turnover-linked features

## Mental Model

```text
User action or gameplay settlement
  -> producer emits event or calls API
  -> Missions updates progress/state
  -> client reads overview/calendar/progress
  -> client claims reward from Missions API
```

## Product Surfaces That Matter Most

- `GET /api/v1/quest/overview`
- `GET /api/v1/missions/check-in/calendar`
- streak restore quote/restore endpoints
- Daily Activities admin endpoints
- level turnover endpoints
- Mission Boost status/claim endpoints

## Daily Activities Role

Missions is the consumer side of the `player.activity.v1` contract.

Important current rules from the repo docs:

- Mobile must not compute mission progress itself
- backend events own Daily Activity progress
- `turnover.settled` for game-scoped turnover should come from `Games-Labs-Game`
- consumer day bucketing follows `Asia/Bangkok`
- local/dev can use an interval reset override, but staging/production stay on midnight Bangkok reset

## Runtime Dependencies

- `shared-lib` for `missionspb` and shared event contracts
- `RABBITMQ_URL` for Daily Activities event consumption
- Wallet integration for reward/economy flows
- api-gateway for public HTTP exposure

## Questions This Repo Often Answers

- Does the current API already support the field the client wants?
- Is a mission status computed from backend events or client inference?
- Is a check-in/streak behavior owned by config, runtime state, or event input?
- Is a Daily Activities problem in producer events, consumer logic, or UI interpretation?

## Current Pressure Areas

- Daily Activities contract strictness vs legacy CRUD assumptions
- check-in calendar correctness, including restore/restored behavior
- event-driven progress visibility in overview/progress APIs
- admin configuration shape for activity conditions and filters

## Good Uses For This Note

- starting a Missions contract check
- locating whether a behavior is mobile-facing, admin-facing, or event-driven
- separating “API exists” from “event pipeline actually feeds it”
- reminding future work that Missions is a product rules engine, not only a CRUD service

## Related

- [[Shared Lib - Project Map]]
- [[Turnover Settlement Flow]]
- [[Backoffice API Contract Map]]

