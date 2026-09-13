# User Types: `shot_t` & `lag_record_t`

This section documents `shot_t` (passed to ragebot shot callbacks) and `lag_record_t` (representing backtrack and lag compensation records).

---

## 1. `shot_t`

The shot descriptor passed to [`"aim_shot"`](../callbacks/aimbot.md) and [`"aim_ack"`](../callbacks/aimbot.md).

### Properties
- `shot.command_number`: Integer command number of the shot.
- `shot.client_shoot_pos`: `vector` eye origin when the shot was queued.
- `shot.target_pos`: `vector` 3D coordinate of the targeted hitbox center.
- `shot.client_angle`: `qangle` calculated by ragebot for the shot.
- `shot.wanted_damage`: Integer damage projected by AutoWall.
- `shot.wanted_damagegroup`: Integer targeted hitgroup ID.
- `shot.hitchance`: Hitchance percentage float ($0$ to $100$).
- `shot.backtrack`: Integer number of ticks backtracked.
- `shot.record`: [`lag_record_t`](#lag_record_t) of the target.
- `shot.shoot_pos`: `vector` verified shooting origin acknowledged by server.
- `shot.end_pos`: `vector` final ray termination point on hit or wall impact.
- `shot.angle`: `qangle` validated shot angle.
- `shot.impacts`: Array table of `vector` impact coordinates.
- `shot.damage`: Integer damage actually dealt ($0$ if missed).
- `shot.damagegroup`: Integer hitgroup where the shot landed.
- `shot.hit_point`: `vector` coordinate of the bullet impact on the enemy body.
- `shot.acked`: Boolean indicating whether server acknowledged the shot.
- `shot.miss_reason`: String explaining why the shot missed (e.g. `"spread"`, `"occlusion"`, `"unregistered"`). Empty string if hit.

---

## 2. `lag_record_t`

Represents a captured backtrack simulation record for an enemy player.

### Properties
- `record.player`: [`player_t`](entity-and-player.md) target player pointer.
- `record.origin`: `vector` world coordinate origin of the player at record time.
- `record.velocity`: `vector` velocity at record time.
- `record.mins`: `vector` collision bounding box minimums.
- `record.maxs`: `vector` collision bounding box maximums.
- `record.abs_angles`: `qangle` absolute model rendering angles.
- `record.view_angles`: `qangle` target player eye angles.
- `record.simulation_time`: Float simulation time timestamp.
- `record.duck_amount`: Float crouch amount ($0.0$ to $1.0$).
- `record.duck_speed`: Float crouch transition speed.
- `record.choked_ticks`: Number of ticks choked by target prior to this record.
- `record.shifting_tickbase`: Boolean property (get/set) indicating if player is exploiting tickbase.
- `record.breaking_lag_compensation`: Boolean property (get/set) indicating if player is breaking lag compensation.
