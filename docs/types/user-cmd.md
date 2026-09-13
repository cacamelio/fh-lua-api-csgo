# User Types: `user_cmd_t` & `antiaim_context_t`

This section documents `user_cmd_t` passed to `"createmove"` and `antiaim_context_t` passed to `"antiaim"`.

---

## 1. `user_cmd_t`

The user command passed to the `"createmove"` callback:

```lua
client.add_callback("createmove", function(cmd)
    -- cmd is user_cmd_t
end)
```

### Fields & Properties
| Field | Type | Access | Description |
| :--- | :--- | :--- | :--- |
| `cmd.command_number` | `integer` | Read-only | Sequence number of the command. |
| `cmd.tickcount` | `integer` | Read/Write | Tickcount timestamp for command simulation. |
| `cmd.move` | [`vector`](vector.md) | Read/Write | Vector where `x` is `forwardmove`, `y` is `sidemove`, `z` is `upmove`. |
| `cmd.viewangles` | [`qangle`](qangle.md) | Read/Write | Player aim orientation (`pitch`, `yaw`, `roll`). |
| `cmd.buttons` | `integer` | Read/Write | Bitmask of input buttons (`IN_ATTACK`, `IN_JUMP`, etc.). |
| `cmd.random_seed` | `integer` | Read/Write | Random seed used for bullet spread RNG. |
| `cmd.allow_defensive` | `boolean` | Read/Write | Whether the defensive exploit is permitted this tick. |
| `cmd.override_defensive`| `boolean` or `nil` | Read/Write | Manually forces defensive exploit activation on/off. |

### Methods
- `cmd:set_move(yaw: number, speed?: number) -> void`: Computes and sets `cmd.move.x` and `cmd.move.y` so that the player moves towards the world angle `yaw` at `speed` (if `speed` is omitted, preserves current move length).

---

## 2. `antiaim_context_t`

The context object passed to the `"antiaim"` callback:

```lua
client.add_callback("antiaim", function(ctx)
    -- ctx is antiaim_context_t
end)
```

### Methods
- `ctx:pitch(pitch_angle: number) -> void`: Sets third-person pitch angle (e.g. `89.0`).
- `ctx:yaw(yaw_angle: number) -> void`: Sets third-person base yaw angle (e.g. `180.0`).
- `ctx:yaw_offset(offset: number) -> void`: Adds angular offset to base yaw.
- `ctx:fakelag(choke_amount: integer) -> void`: Sets number of ticks to choke ($1$ to $16$).
- `ctx:desync(enabled: boolean) -> void`: Enables or disables desynchronization fake angle.
- `ctx:desync_angle(angle: number) -> void`: Sets max desync delta angle (e.g. `58.0`).
- `ctx:desync_side(side: integer) -> void`: Sets desync side: `-1` for Left, `1` for Right.
