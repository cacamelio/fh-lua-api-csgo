# Namespace: `common`

The `common` namespace exposes map information, system and Unix time, and microphone recording state.

---

## Methods

### `common.get_map_name`
Returns the full relative file path of the current level (e.g. `"maps/de_dust2.bsp"`).

```lua
common.get_map_name() -> string
```

---

### `common.get_map_shortname`
Returns the short friendly name of the current map (e.g. `"de_dust2"`, `"de_mirage"`).

```lua
common.get_map_shortname() -> string
```

---

### `common.get_local_time`
Returns a `systime_t` object representing the current local Windows system clock time.

```lua
common.get_local_time() -> systime_t
```

#### Fields of `systime_t`
- `time.year`: Year (e.g. 2026)
- `time.month`: Month ($1$ to $12$)
- `time.day`: Day of month ($1$ to $31$)
- `time.day_of_week`: Day of week ($0$ = Sunday, $1$ = Monday, etc.)
- `time.hour`: Hour ($0$ to $23$)
- `time.minute`: Minute ($0$ to $59$)
- `time.second`: Second ($0$ to $59$)
- `time.millisecond`: Millisecond ($0$ to $999$)

#### Example
```lua
local t = common.get_local_time()
local clock_str = string.format("%02d:%02d:%02d", t.hour, t.minute, t.second)
```

---

### `common.get_unix_time`
Returns the current 32-bit unsigned Unix timestamp (seconds since January 1, 1970).

```lua
common.get_unix_time() -> integer
```

---

### `common.is_recording_voice`
Returns whether the game engine is currently capturing microphone audio from the player.

```lua
common.is_recording_voice() -> boolean
```
