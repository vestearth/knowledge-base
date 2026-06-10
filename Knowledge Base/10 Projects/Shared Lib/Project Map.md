# Shared Lib - Project Map

## Purpose

`shared-lib` is the contract and shared-behavior center for the Games Labs workspace.

When a behavior must be consistent across services, this repo is usually the first place to inspect before changing consumers. That is especially true for protobuf contracts, shared errors, event schemas, metadata/auth helpers, and middleware behavior.

## Source Files

- `/Users/earth/Documents/GitHub/shared-lib/README.md`
- `/Users/earth/Documents/GitHub/shared-lib/proto/`
- `/Users/earth/Documents/GitHub/shared-lib/events/player_activity.go`
- `/Users/earth/Documents/GitHub/shared-lib/pkg/`
- `/Users/earth/Documents/GitHub/AGENTS.md`

## Main Areas

- Protobuf contracts under `proto/*pb/*.proto`
- Generated gateway and gRPC artifacts under the same proto folders
- Shared events, especially Daily Activities event schema
- Common auth, metadata, logging, health, response, and error helpers
- Shared operational conventions that consumer services must not duplicate locally

## Proto Surface

Current proto packages visible in the repo:

- `authpb`
- `basepb`
- `gamepb`
- `logpb`
- `missionspb`
- `orderpb`
- `paymentpb`
- `providerpb`
- `userpb`
- `walletpb`

Rule of thumb:
- If a contract is cross-service and stable enough to share, it probably belongs here.
- If a consumer needs a new proto field/type that other services will depend on, publish in `shared-lib` first, then bump consumers.

## Daily Activities Contract Role

`shared-lib` is the canonical owner of the `player.activity.v1` event contract.

Important fixed points:

- Routing key: `player.activity.v1`
- Schema version: `v1`
- Reverse events must use `reverse_of_event_id`
- Reverse semantics must not be encoded by sending negative amounts
- `occurred_at` is settlement/finalization time
- Missions evaluates event day in `Asia/Bangkok`

This means producers should align to the shared schema instead of inventing service-local payload shapes.

## Shared Logic That Should Not Drift

- metadata/user extraction
- structured logging behavior
- standardized HTTP/gRPC error behavior
- response helpers
- correlation/header constants
- event field names and compatibility policy

Workspace rule:
- `handler message` must live only in `shared-lib`
- consumer services should not keep local duplicate versions of shared contract logic

## Typical Change Workflow

1. Change `.proto` or shared package in `shared-lib`.
2. Regenerate artifacts.
3. Publish/bump `shared-lib`.
4. Update downstream services and gateway.
5. Verify consumers against the new shared contract.

## Good Uses For This Note

- deciding whether a type belongs in a service or in `shared-lib`
- finding the owning proto package
- checking whether a field/event name is canonical or service-local
- reviewing whether a downstream service is duplicating shared behavior

## Related

- [[API Gateway - Project Map]]
- [[Games Labs Missions - Project Map]]
- [[Turnover Settlement Flow]]

