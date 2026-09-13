# User Types: `trace_t` & `fire_bullet_t`

This section documents `trace_t` (`CGameTrace`) and `fire_bullet_t` (`FireBulletData_t`).

---

## 1. `trace_t`

The result object returned by `utils.trace_line` and `utils.trace_hull`.

### Fields
- `trace.startpos`: `vector` ray starting point.
- `trace.endpos`: `vector` final ray endpoint after collision or termination.
- `trace.fraction`: Normalized fraction ($0.0$ to $1.0$) of ray distance traveled before hit ($1.0$ = no collision).
- `trace.allsolid`: Boolean indicating if the trace started and remained inside a solid surface.
- `trace.startsolid`: Boolean indicating if the trace started inside a solid brush.
- `trace.hit_entity`: Entity pointer hit by the ray (or `nil` if hit world geometry).
- `trace.hitgroup`: Hitgroup integer index hit by the ray.

#### Example
```lua
local me = entity.get_local_player()
local eye = me:get_eye_position()
local forward = me:get_eye_angles():angles() * 8192.0

local tr = utils.trace_line(eye, eye + forward, 0x4600400B, me)
if tr.fraction < 1.0 then
    print("Ray hit wall at distance: " .. (eye:dist(tr.endpos)))
end
```

---

## 2. `fire_bullet_t`

The AutoWall calculation result returned by `utils.trace_bullet`.

### Fields
- `data.damage`: Float damage projected to hit the target. If the bullet cannot penetrate or deal damage, this value is `-1.0`.
- `data.trace`: The initial enter [`trace_t`](#1-trace_t) structure.
