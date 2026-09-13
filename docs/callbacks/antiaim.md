# Anti-Aim Callback (`"antiaim"`)

The `"antiaim"` callback executes during the cheat's anti-aim processing stage in `CreateMove`. It gives you control over your local player's third-person angles, pitch, yaw offsets, fake lag limits, desync states, and desync rotation directions.

---

## Signature

```lua
client.add_callback("antiaim", function(ctx)
    -- ctx is an antiaim_context_t object
end)
```

- **Arguments**: `ctx` ([`antiaim_context_t`](../types/user-cmd.md))
- **Return Value**: None. Methods on `ctx` are called to apply angle overrides.

---

## The `antiaim_context_t` Context

When you call any override method on `ctx`, an internal bitmask flag is set for that property so the cheat knows to use your Lua value instead of the menu setting.

| Method | Argument Type | Description |
| :--- | :--- | :--- |
| `ctx:pitch(value)` | `number` | Overrides pitch angle (e.g., `89.0` for down, `-89.0` for up). |
| `ctx:yaw(value)` | `number` | Overrides base yaw angle directly (e.g. `180.0` for backwards). |
| `ctx:yaw_offset(value)` | `number` | Adds an angular offset relative to the base anti-aim yaw. |
| `ctx:fakelag(choked_ticks)` | `integer` | Sets the exact number of packets to choke for fake lag ($1$ to $16$). |
| `ctx:desync(enable)` | `boolean` | Enables or disables desynchronization / fake angles. |
| `ctx:desync_angle(value)` | `number` | Overrides the desync delta limit angle (e.g. `58.0`). |
| `ctx:desync_side(side)` | `integer` | Selects desync direction: `-1` for Left, `1` for Right, or `0` for none. |

---

## Examples

### 1. Manual Directional Anti-Aim (Left / Right / Back)
```lua
local side = 0 -- 0 = Back, -1 = Left, 1 = Right

client.add_callback("antiaim", function(ctx)
    -- Hotkeys to change direction (Z: Left, X: Back, C: Right)
    if utils.is_key_pressed(0x5A) then side = -1 end -- 'Z'
    if utils.is_key_pressed(0x58) then side = 0  end -- 'X'
    if utils.is_key_pressed(0x43) then side = 1  end -- 'C'

    -- Pitch Down
    ctx:pitch(89.0)

    -- Base yaw backwards (180 degrees)
    ctx:yaw(180.0)

    -- Apply manual angle offset
    if side == -1 then
        ctx:yaw_offset(-90.0) -- Facing Left
        ctx:desync_side(1)
    elseif side == 1 then
        ctx:yaw_offset(90.0)  -- Facing Right
        ctx:desync_side(-1)
    else
        ctx:yaw_offset(0.0)   -- Facing Back
        ctx:desync_side(1)
    end

    -- Enable desync with 58 degree max delta
    ctx:desync(true)
    ctx:desync_angle(58.0)
end)
```

### 2. Dynamic Jitter Anti-Aim & Adaptive Fake Lag
```lua
local jitter_state = false

client.add_callback("antiaim", function(ctx)
    jitter_state = not jitter_state
    
    local jitter_offset = jitter_state and 25.0 or -25.0
    ctx:yaw_offset(jitter_offset)

    -- Dynamic fake lag: 14 ticks in air, 6 ticks on ground
    local local_player = entity.get_local_player()
    if local_player then
        local flags = local_player:get_flags()
        local on_ground = bit32.band(flags, 1) ~= 0
        ctx:fakelag(on_ground and 6 or 14)
    end
end)
```
