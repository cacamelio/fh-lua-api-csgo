# Aimbot & Shot Callbacks (`"aim_shot"`, `"aim_ack"`)

The cheat provides two dedicated ragebot callbacks for tracking and logging shots:
1. **`"aim_shot"`**: Dispatched immediately when the ragebot fires a bullet.
2. **`"aim_ack"`**: Dispatched when the server acknowledges the bullet, confirming either a registered hit or a miss with the exact reason.

---

## The `"aim_shot"` Callback

Fired at the exact moment a ragebot shot is queued.

### Signature
```lua
client.add_callback("aim_shot", function(shot)
    -- shot is a shot_t object
end)
```

---

## The `"aim_ack"` Callback

Fired when the shot is acknowledged by server bullet impact and player hurt events (or timed out / unregistered).

### Signature
```lua
client.add_callback("aim_ack", function(shot)
    -- shot is a shot_t object
end)
```

---

## The `shot_t` Object Structure

| Field | Type | Context | Description |
| :--- | :--- | :--- | :--- |
| `shot.command_number` | `integer` | Shot & Ack | Command number of the firing tick. |
| `shot.client_shoot_pos` | [`vector`](../types/vector.md) | Shot & Ack | Origin of player eye position at shot time. |
| `shot.target_pos` | [`vector`](../types/vector.md) | Shot & Ack | Targeted hitbox center in 3D space. |
| `shot.client_angle` | [`qangle`](../types/qangle.md) | Shot & Ack | View angles calculated for the shot. |
| `shot.wanted_damage` | `integer` | Shot & Ack | Projected damage calculated by AutoWall. |
| `shot.wanted_damagegroup`| `integer` | Shot & Ack | Hitgroup targeted (e.g., `1` for Head). |
| `shot.hitchance` | `number` | Shot & Ack | Calculated hitchance percentage ($0$ to $100$). |
| `shot.backtrack` | `integer` | Shot & Ack | Backtracked tick count ($0$ for current tick). |
| `shot.record` | [`lag_record_t`](../types/shot-data.md) | Shot & Ack | Target's animation and lag compensation record. |
| `shot.shoot_pos` | [`vector`](../types/vector.md) | Ack | Server validated shooting position. |
| `shot.end_pos` | [`vector`](../types/vector.md) | Ack | Bullet ray endpoint on hit or wall impact. |
| `shot.damage` | `integer` | Ack | Actual damage dealt ($0$ if missed). |
| `shot.damagegroup` | `integer` | Ack | Hitgroup where the enemy was actually hit. |
| `shot.hit_point` | [`vector`](../types/vector.md) | Ack | Actual 3D coordinate where bullet impacted player. |
| `shot.acked` | `boolean` | Ack | `true` if server acknowledged the shot. |
| `shot.miss_reason` | `string` | Ack | Detailed miss reason (e.g. `"spread"`, `"occlusion"`, `"unregistered"`). Empty if hit. |

---

## Example: Building a Detailed Hit & Miss Logger

```lua
local hitgroup_names = {
    [0] = "generic",
    [1] = "head",
    [2] = "chest",
    [3] = "stomach",
    [4] = "left arm",
    [5] = "right arm",
    [6] = "left leg",
    [7] = "right leg",
    [8] = "neck",
    [10] = "gear"
}

-- 1. Log fired shot
client.add_callback("aim_shot", function(shot)
    local target_name = "unknown"
    if shot.record and shot.record.player then
        target_name = shot.record.player:get_name()
    end

    local hg = hitgroup_names[shot.wanted_damagegroup] or "body"
    print_raw(string.format("[Shot] Fired at %s (%s) for %d dmg (hc: %.1f%%, bt: %d ticks)\n",
        target_name, hg, shot.wanted_damage, shot.hitchance, shot.backtrack), color(200, 200, 255))
end)

-- 2. Log shot result (Hit or Miss)
client.add_callback("aim_ack", function(shot)
    local target_name = "unknown"
    if shot.record and shot.record.player then
        target_name = shot.record.player:get_name()
    end

    if shot.damage > 0 then
        local hit_hg = hitgroup_names[shot.damagegroup] or "body"
        print_raw(string.format("[Hit] %s in %s for %d dmg (wanted: %d)\n",
            target_name, hit_hg, shot.damage, shot.wanted_damage), color(100, 255, 100))
    else
        local reason = shot.miss_reason ~= "" and shot.miss_reason or "unknown"
        print_raw(string.format("[Miss] Missed %s due to %s\n",
            target_name, reason), color(255, 80, 80))
    end
end)
```
