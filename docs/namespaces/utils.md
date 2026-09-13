# Namespace: `utils`

The `utils` namespace contains engine ray tracing, AutoWall bullet penetration, signature pattern scanning, interface factory lookup, key state queries, and clantag animation tools.

---

## Memory & Engine

### `utils.create_interface`
Retrieves a pointer address to an exported engine or client interface.

```lua
utils.create_interface(module_name: string, interface_name: string) -> integer
```

#### Example
```lua
local vgui_surface = utils.create_interface("vguimatsurface.dll", "VGUI_Surface031")
```

---

### `utils.pattern_scan`
Performs an IDA-style byte pattern scan within a loaded module.

```lua
utils.pattern_scan(module_name: string, pattern: string) -> integer
```

#### Example
```lua
local address = utils.pattern_scan("client.dll", "55 8B EC 8B 45 08 8A 48 08")
```

---

### `utils.console_exec`
Executes an engine console command through `EngineClient->ExecuteClientCmd`.

```lua
utils.console_exec(command: string) -> void
```

#### Example
```lua
utils.console_exec("clear")
utils.console_exec("say Hello from Lua!")
```

---

## Ray Tracing & AutoWall

### `utils.trace_line`
Performs a line trace from a start vector to an end vector against the world and entity collision models.

```lua
utils.trace_line(start_pos: vector, end_pos: vector, mask: integer, filter: entity_t | function) -> trace_t
```

#### Parameters
- `mask`: Trace mask bitfield (e.g. `0x4600400B` for `MASK_SHOT`).
- `filter`: Can be an `entity_t` to ignore, or a **custom Lua filter callback** `function(entity) -> boolean`!

#### Example
```lua
local me = entity.get_local_player()
local trace = utils.trace_line(me:get_eye_position(), me:get_eye_position() + vector(0, 0, 100), 0x4600400B, me)
print("Fraction: " .. trace.fraction)
```

---

### `utils.trace_hull`
Traces an axis-aligned bounding box (AABB) along a ray.

```lua
utils.trace_hull(start_pos: vector, end_pos: vector, mins: vector, maxs: vector, mask: integer, filter: entity_t | function) -> trace_t
```

---

### `utils.trace_bullet`
Invokes the cheat's internal **AutoWall** engine to simulate bullet penetration through walls, computing exact damage to a target player.

```lua
utils.trace_bullet(attacker: player_t, start_pos: vector, end_pos: vector, target?: player_t) -> fire_bullet_t
```

#### Returns
- [`fire_bullet_t`](../types/trace.md) containing:
  - `damage`: Calculated damage (float). If bullet cannot penetrate, damage is `-1.0`.
  - `trace`: Enter `trace_t` of the wall collision.

#### Example
```lua
local me = entity.get_local_player()
local enemy = rage.get_current_threat()
if me and enemy then
    local wallbang = utils.trace_bullet(me, me:get_eye_position(), enemy:get_hitbox_position(0), enemy)
    print("Projected Wallbang Damage: " .. wallbang.damage)
end
```

---

## Utilities & Clantag

### `utils.set_clantag`
Sets your player clan tag in game.

```lua
utils.set_clantag(tag: string) -> void
```

---

### `utils.is_key_pressed`
Queries the asynchronous state of a virtual key code (`GetAsyncKeyState`).

```lua
utils.is_key_pressed(vkey: integer) -> boolean
```

---

### `utils.get_mouse_position`
Returns the mouse cursor position relative to the game window as a `vector`.

```lua
utils.get_mouse_position() -> vector
```

---

### `utils.get_net_channel`
Returns the active [`net_channel_info_t`](../types/netchannel-and-voice.md) object for ping, packet loss, and rate statistics.

```lua
utils.get_net_channel() -> net_channel_info_t | nil
```

---

### Random Number Generators
- `utils.random_seed(seed: integer) -> void`
- `utils.random_int(min: integer, max: integer) -> integer`
- `utils.random_float(min: number, max: number) -> number`

---

### `utils.send_voice_message`
Sends a custom voice network packet to other players on the server.

```lua
utils.send_voice_message(voice_data: voice_data) -> void
```
