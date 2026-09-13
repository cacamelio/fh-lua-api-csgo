# CreateMove Callback (`"createmove"`)

The `"createmove"` callback is called every tick during the game engine's `CreateMove` pipeline. It gives your script direct access to the user command (`user_cmd_t`), allowing you to modify view angles, movement vectors, button bitmasks, and defensive exploit flags before the command is sent to the game server.

---

## Signature

```lua
client.add_callback("createmove", function(cmd)
    -- cmd is a user_cmd_t object
end)
```

- **Arguments**: `cmd` ([`user_cmd_t`](../types/user-cmd.md))
- **Return Value**: None. Changes are made directly by mutating `cmd` fields or methods.

---

## The `user_cmd_t` Object

| Field / Method | Type | Description |
| :--- | :--- | :--- |
| `cmd.command_number` | `integer` | Incremental command sequence number. |
| `cmd.tickcount` | `integer` | Tick count for this command. |
| `cmd.move` | [`vector`](../types/vector.md) | Movement vector where `move.x` is `forwardmove`, `move.y` is `sidemove`, `move.z` is `upmove`. |
| `cmd.viewangles` | [`qangle`](../types/qangle.md) | Aim view angles (`pitch`, `yaw`, `roll`). |
| `cmd.buttons` | `integer` | Bitmask of active inputs (`IN_ATTACK`, `IN_JUMP`, `IN_DUCK`, etc.). |
| `cmd.random_seed` | `integer` | Engine random seed for weapon spread. |
| `cmd.allow_defensive` | `boolean` | Allows or disallows defensive tickbase shifting for this tick. |
| `cmd.override_defensive` | `boolean` or `nil` | If set to a boolean, forces defensive exploit execution on/off for this tick. |
| `cmd:set_move(yaw, [speed])` | `function` | Calculates forward and side move values to move towards a specific world yaw angle. |

---

## Common Button Flags

```lua
local IN_ATTACK    = bit32.lshift(1, 0)
local IN_JUMP      = bit32.lshift(1, 1)
local IN_DUCK      = bit32.lshift(1, 2)
local IN_FORWARD   = bit32.lshift(1, 3)
local IN_BACK      = bit32.lshift(1, 4)
local IN_USE       = bit32.lshift(1, 5)
local IN_MOVELEFT  = bit32.lshift(1, 9)
local IN_MOVERIGHT = bit32.lshift(1, 10)
local IN_ATTACK2   = bit32.lshift(1, 11)
local IN_RELOAD    = bit32.lshift(1, 13)
local IN_SPEED     = bit32.lshift(1, 17) -- Shift walk
local IN_BULLRUSH  = bit32.lshift(1, 22)
```

---

## Examples

### 1. Directional Movement Helper (`cmd:set_move`)
Moves your player toward an absolute world yaw angle regardless of where you are looking:

```lua
client.add_callback("createmove", function(cmd)
    local local_player = entity.get_local_player()
    if not local_player or not local_player:is_alive() then return end

    -- Move directly North (yaw 0) at max running speed (450.0)
    if utils.is_key_pressed(0x45) then -- 'E' key
        cmd:set_move(0.0, 450.0)
    end
end)
```

### 2. Auto-Crouch on Shoot
```lua
client.add_callback("createmove", function(cmd)
    if bit32.band(cmd.buttons, 1) ~= 0 then -- IN_ATTACK
        cmd.buttons = bit32.bor(cmd.buttons, 4) -- Add IN_DUCK
    end
end)
```

### 3. Defensive Exploit Control
```lua
client.add_callback("createmove", function(cmd)
    -- Force defensive double-tap tickbase warp when pressing hotkey
    if utils.is_key_pressed(0x06) then -- Mouse 5
        cmd.override_defensive = true
    end
end)
```
