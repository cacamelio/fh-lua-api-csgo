# Example: Shot & Miss Logger

This script intercepts ragebot shots using `"aim_shot"` and `"aim_ack"`, formatting clean developer console logs and on-screen kill/hit event feeds.

---

## Code (`shot_logger.lua`)

```lua
local hitgroups = {
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

local log_box = ui.groupbox("Scripts", "Shot Logger")
local console_log_cb = log_box:checkbox("Log to Console", true)
local onscreen_log_cb = log_box:checkbox("On-Screen Feed", true)

local on_screen_events = {}

local function add_screen_log(text, col)
    table.insert(on_screen_events, 1, {
        text = text,
        color = col,
        time = globals.realtime + 5.0 -- Show for 5 seconds
    })
    if #on_screen_events > 6 then
        table.remove(on_screen_events)
    end
end

-- 1. Fired Shot Handler
client.add_callback("aim_shot", function(shot)
    if not console_log_cb:get() then return end

    local enemy_name = "target"
    if shot.record and shot.record.player then
        enemy_name = shot.record.player:get_name()
    end

    local hg = hitgroups[shot.wanted_damagegroup] or "body"
    local msg = string.format("[Aimbot] Fired at %s -> hitbox: %s | wanted dmg: %d | hitchance: %.1f%% | backtrack: %d ticks\n",
        enemy_name, hg, shot.wanted_damage, shot.hitchance, shot.backtrack)
    
    print_raw(msg, color(180, 180, 255))
end)

-- 2. Shot Acknowledgment Handler (Hit or Miss)
client.add_callback("aim_ack", function(shot)
    local enemy_name = "target"
    if shot.record and shot.record.player then
        enemy_name = shot.record.player:get_name()
    end

    if shot.damage > 0 then
        -- HIT
        local hg = hitgroups[shot.damagegroup] or "body"
        local msg = string.format("[Aimbot] Hit %s in %s for %d damage (wanted: %d)\n",
            enemy_name, hg, shot.damage, shot.wanted_damage)

        if console_log_cb:get() then
            print_raw(msg, color(100, 255, 100))
        end
        if onscreen_log_cb:get() then
            add_screen_log(string.format("Hit %s in %s for %d dmg", enemy_name, hg, shot.damage), color(100, 255, 100))
        end
    else
        -- MISS
        local reason = shot.miss_reason ~= "" and shot.miss_reason or "unknown"
        local msg = string.format("[Aimbot] Missed %s due to %s\n", enemy_name, reason)

        if console_log_cb:get() then
            print_raw(msg, color(255, 90, 90))
        end
        if onscreen_log_cb:get() then
            add_screen_log(string.format("Missed %s (%s)", enemy_name, reason), color(255, 90, 90))
        end
    end
end)

-- 3. Render On-Screen Feed
client.add_callback("render", function()
    if not onscreen_log_cb:get() then return end

    local y = 200
    for i = #on_screen_events, 1, -1 do
        local ev = on_screen_events[i]
        if globals.realtime > ev.time then
            table.remove(on_screen_events, i)
        else
            render.text(1, vector(25, y, 0), ev.color, "od", ev.text)
            y = y + 18
        end
    end
end)
```
