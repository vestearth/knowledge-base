# Games Labs Game - Project Map

## Purpose

`Games-Labs-Game` owns catalog, launch eligibility, and the canonical gameplay round lifecycle used by Daily Activities producers.

It is the system boundary where provider-facing game ids must become canonical platform game UUIDs, and where gameplay settlement becomes reusable event data for Missions.

## Source Files

- `/Users/earth/Documents/GitHub/Games-Labs-Game/README.md`
- `/Users/earth/Documents/GitHub/Games-Labs-Game/internal/`
- `/Users/earth/Documents/GitHub/Games-Labs-Game/k3s/service.yaml`
- `/Users/earth/Documents/GitHub/shared-lib/proto/gamepb/game.proto`
- `/Users/earth/Documents/GitHub/shared-lib/events/player_activity.go`

## Main Areas

- game catalog and category listing
- launch flow and playable-level checks
- Golden Pass-aware unlock behavior
- `SettleRound` / reverse settlement lifecycle
- `player.activity.v1` production for round and gameplay turnover

## Mental Model

```text
Catalog / Launch request
  -> Game checks user/pass/runtime dependencies
  -> returns playable metadata or launch result

Provider settlement callback
  -> Provider calls Game SettleRound
  -> Game stores canonical round lifecycle
  -> Game publishes round / turnover events
```

## High-Pressure Behaviors

### Golden Pass

- depends on `MISSIONS_API_URL`
- checks active pass ownership live
- uses cached config briefly
- affects playability metadata and launch enforcement

### Settlement

- `round_lifecycles.settled_amount` is the authoritative gameplay turnover field
- first insert wins on duplicate settlement attempts
- `turnover.settled` fires only when settled amount is positive
- provider game ids must resolve to platform game UUIDs before event production

## Runtime Dependencies

- `USER_API_URL` for authenticated playability evaluation
- `MISSIONS_API_URL` for Golden Pass checks
- `RABBITMQ_URL` for Daily Activities event publishing
- external Provider access through gRPC NodePort `30553`

## Common Questions This Repo Answers

- why does a game appear visible but not playable?
- is Golden Pass changing only metadata or final launch enforcement too?
- did gameplay turnover really become canonical `turnover.settled`?
- are we using provider game ids where platform UUIDs are required?

## Good Uses For This Note

- debugging launch eligibility
- tracing gameplay turnover into Missions
- checking where game UUID normalization happens
- separating discovery/list behavior from hard launch enforcement

## Related

- [[Turnover Settlement Flow]]
- [[Games Labs Missions - Project Map]]
- [[Shared Lib - Project Map]]

