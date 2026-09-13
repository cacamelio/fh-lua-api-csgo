# Namespace: `rage`

The `rage` namespace exposes states and controls for the ragebot aimbot, tickbase manipulation (double-tap), defensive peek exploits, and anti-aim tracking.

---

## Methods

### `rage.get_current_threat`
Returns the enemy player currently targeted as the primary ragebot threat.

```lua
rage.get_current_threat() -> player_t | nil
```

---

### `rage.get_exploit_charge`
Returns the current exploit recharging percentage as a normalized float ($0.0$ to $1.0$). When $1.0$, double-tap or hide-shots is fully charged and ready to fire.

```lua
rage.get_exploit_charge() -> number
```

---

### `rage.is_defensive_active`
Returns whether defensive tickbase manipulation is currently active for the local player.

```lua
rage.is_defensive_active() -> boolean
```

---

### `rage.get_defensive_ticks`
Returns the number of ticks currently used by defensive exploits.

```lua
rage.get_defensive_ticks() -> integer
```

---

### `rage.is_shifting`
Returns whether the cheat is currently shifting tickbase (e.g. executing a double-tap burst or break lag compensation sequence).

```lua
rage.is_shifting() -> boolean
```

---

### `rage.force_teleport`
Forces the local tickbase to instantly teleport (break lag compensation) on the next command.

```lua
rage.force_teleport() -> void
```

---

### `rage.force_charge`
Forces immediate recharging of tickbase shift ticks without waiting for natural delay.

```lua
rage.force_charge() -> void
```

---

### `rage.override_tickbase_shift`
Overrides the number of ticks to shift for lag compensation / exploits.

```lua
rage.override_tickbase_shift(ticks: integer) -> void
```

---

### `rage.get_antiaim_yaw`
Returns the base anti-aim yaw angle currently calculated by the anti-aim system.

```lua
rage.get_antiaim_yaw() -> number
```

---

### `rage.is_peeking`
Returns whether the local player is currently peeking an enemy or target angle based on AutoPeek state.

```lua
rage.is_peeking() -> boolean
```

---

## Example: Double-Tap Ready Indicator

```lua
client.add_callback("render", function()
    if not globals.is_in_game then return end

    local charge = rage.get_exploit_charge()
    local is_ready = charge >= 1.0
    local is_def = rage.is_defensive_active()

    local text = string.format("DT: %s (%.0f%%)%s", 
        is_ready and "READY" or "CHARGING", 
        charge * 100,
        is_def and " [DEFENSIVE]" or "")

    local col = is_ready and color(100, 255, 100) or color(255, 160, 0)
    render.text(1, vector(50, 400, 0), col, "od", text)
end)
```
