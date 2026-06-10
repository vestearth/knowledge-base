# Turnover Settlement Flow

## Purpose

This note captures the gameplay turnover rail that matters for Daily Activities and related mission progress.

It exists to prevent a common mistake: treating all turnover-like records as the same thing.

## Source Files

- `/Users/earth/Documents/GitHub/Games-Labs-Provider/README.md`
- `/Users/earth/Documents/GitHub/Games-Labs-Game/README.md`
- `/Users/earth/Documents/GitHub/Games-Labs-Missions/README.md`
- `/Users/earth/Documents/GitHub/shared-lib/README.md`
- `/Users/earth/Documents/GitHub/ai-dev-office/docs/turnover-rollout-runbook.md`

## Main Flow

```text
Provider wallet settlement succeeds
  -> Provider calls Game SettleRound / ReverseRound
  -> Game stores round lifecycle and settled_amount
  -> Game publishes player.activity.v1
  -> Missions consumes event
  -> Missions updates progress
  -> Mobile / client reads overview or progress
```

## Ownership By Repo

- `Games-Labs-Provider`: detects provider callback success and triggers settlement hook
- `Games-Labs-Game`: canonical gameplay round lifecycle owner and event producer for game-scoped turnover
- `shared-lib`: owns `player.activity.v1` schema and compatibility rules
- `Games-Labs-Missions`: consumes the event and turns it into Daily Activities progress

## Critical Rules

- Game-scoped turnover for Missions must come from `Games-Labs-Game`, not directly from Provider.
- `turnover.settled` for gameplay is a different rail from exchange/order turnover.
- Reverse semantics must use reverse events, not negative numbers.
- `occurred_at` is settlement/finalization time and Missions evaluates it in `Asia/Bangkok`.

## Rails To Keep Separate

### Gameplay turnover rail

- source service: `games-labs-game`
- carries `game_id` and `game_type`
- used for game/game-type mission progress

### Order/exchange turnover rail

- source service: `games-labs-order`
- does not carry gameplay `game_id` / `game_type`
- should not be double-counted as gameplay turnover

## Provider Hook Notes

Current provider docs show settlement hooks for:

- `afb`
- `1up`
- `vp`
- `idg`
- `ggsoft`
- `sigma`

Common rule:
- settlement hook fires only after wallet operation succeeds
- if `GAME_API_URL` is not configured, local/dev may no-op but production-like env should fail fast

## Failure Checklist

When turnover progress does not move, check in this order:

1. Did Provider actually fire the settlement hook?
2. Was `GAME_API_URL` configured correctly?
3. Did Game persist `round_lifecycles.settled_amount`?
4. Did Game publish `player.activity.v1`?
5. Did Missions consume `turnover.settled`?
6. Is the client reading the correct overview/progress endpoint?

## Common Misreads

- wallet transaction exists -> not enough
- provider callback exists -> not enough
- order turnover exists -> not proof of gameplay turnover
- event emitted once -> not proof that client/UI is reading the right projection

## Related

- [[Games Labs Missions - Project Map]]
- [[Shared Lib - Project Map]]
- [[Backoffice API Contract Map]]

