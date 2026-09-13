# Game Events Callback (`"game_events"`)

The `"game_events"` callback is triggered whenever a Source engine `IGameEvent` is broadcasted by the server. Common events include `player_hurt`, `player_death`, `bullet_impact`, `round_start`, `round_end`, `bomb_planted`, and `item_purchase`.

---

## Signature

```lua
client.add_callback("game_events", function(event)
    -- event is an IGameEvent wrapper object
end)
```

- **Arguments**: `event` ([`game_event_t`](../types/shot-data.md))
- **Return Value**: None.

---

## The `game_event_t` Methods

The `event` object provides both explicit type-getter functions and convenient metamethod index access:

| Method / Syntax | Return Type | Description |
| :--- | :--- | :--- |
| `event:get_name()` | `string` | Name of the event (e.g. `"player_hurt"`). |
| `event:get_int(key)` | `integer` | Retrieves an integer value for key. |
| `event:get_float(key)` | `number` | Retrieves a float value for key. |
| `event:get_bool(key)` | `boolean` | Retrieves a boolean value for key. |
| `event:get_string(key)` | `string` | Retrieves a string value for key. |
| `event[key]` or `event.key`| `any` | **Dynamic index accessor**: Automatically inspects KeyValues data type (`string`, `int`, `float`, `color`) and returns the appropriate Lua value! |

---

## Example: Kill Sounds & Kill Counter

```lua
local kills = 0

client.add_callback("game_events", function(event)
    local event_name = event:get_name()

    if event_name == "player_death" then
        local local_player = entity.get_local_player()
        if not local_player then return end

        local local_userid = local_player:get_player_info().userid
        local attacker = event.attacker
        local victim = event.userid
        local headshot = event.headshot

        -- Did we kill this player?
        if attacker == local_userid and victim ~= local_userid then
            kills = kills + 1
            local victim_player = entity.get(victim, true) -- true = is_userid
            local victim_name = victim_player and victim_player:get_name() or "enemy"

            print_raw(string.format("[Kill #%d] Eliminated %s%s\n",
                kills, victim_name, headshot and " with Headshot!" or ""), color(255, 215, 0))

            -- Play a hit sound via console command or panorama
            utils.console_exec("play buttons/bell1.wav")
        end

    elseif event_name == "round_start" then
        -- Reset round-specific states
        print("Round started!")
    end
end)
```

---

## Example: Bullet Impact Visualizer

```lua
client.add_callback("game_events", function(event)
    if event:get_name() == "bullet_impact" then
        local local_player = entity.get_local_player()
        if not local_player then return end

        local local_userid = local_player:get_player_info().userid
        if event.userid == local_userid then
            local impact_pos = vector(event.x, event.y, event.z)
            print(string.format("Local impact at: %.1f, %.1f, %.1f", impact_pos.x, impact_pos.y, impact_pos.z))
        end
    end
end)
```
