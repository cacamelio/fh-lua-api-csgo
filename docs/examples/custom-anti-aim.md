# Example: Custom Anti-Aim & Fake Lag

This script builds a complete custom Anti-Aim controller with manual directional switches (Left/Right/Back), jitter angle modulation, and adaptive fake lag choke limits.

---

## Code (`custom_aa.lua`)

```lua
local aa_box = ui.groupbox("Scripts", "Custom Anti-Aim")
local enabled_cb = aa_box:checkbox("Enable Lua AA", true)
local pitch_combo = aa_box:combo("Pitch", "Off", "Down", "Up")
local jitter_slider = aa_box:slider_int("Jitter Range", 0, 90, 30)
local desync_cb = aa_box:checkbox("Desync", true)
local fakelag_slider = aa_box:slider_int("Fake Lag Limit", 1, 16, 14)

local current_side = 0 -- -1 = Left, 0 = Back, 1 = Right
local jitter_flip = false

client.add_callback("antiaim", function(ctx)
    if not enabled_cb:get() then
        return
    end

    local local_player = entity.get_local_player()
    if not local_player or not local_player:is_alive() then
        return
    end

    -- 1. Manual Direction Keys (Z = Left, X = Back, C = Right)
    if utils.is_key_pressed(0x5A) then current_side = -1 end -- 'Z'
    if utils.is_key_pressed(0x58) then current_side = 0  end -- 'X'
    if utils.is_key_pressed(0x43) then current_side = 1  end -- 'C'

    -- 2. Pitch Override
    local pitch_mode = pitch_combo:get()
    if pitch_mode == 1 then
        ctx:pitch(89.0) -- Down
    elseif pitch_mode == 2 then
        ctx:pitch(-89.0) -- Up
    end

    -- 3. Base Yaw & Manual Direction
    ctx:yaw(180.0) -- Backwards

    local offset = 0.0
    if current_side == -1 then
        offset = -90.0
    elseif current_side == 1 then
        offset = 90.0
    end

    -- 4. Jitter Calculation
    jitter_flip = not jitter_flip
    local jitter_amount = jitter_slider:get()
    if jitter_amount > 0 then
        local delta = jitter_flip and (jitter_amount / 2) or -(jitter_amount / 2)
        offset = offset + delta
    end
    ctx:yaw_offset(offset)

    -- 5. Desync Fake Angle
    if desync_cb:get() then
        ctx:desync(true)
        ctx:desync_angle(58.0)
        -- Invert desync side relative to manual angle
        ctx:desync_side(current_side == -1 and 1 or -1)
    else
        ctx:desync(false)
    end

    -- 6. Fake Lag
    ctx:fakelag(fakelag_slider:get())
end)

-- Visual Direction Indicator on Screen
client.add_callback("render", function()
    if not enabled_cb:get() or not globals.is_in_game then return end

    local screen = render.screen_size()
    local cx = screen.x / 2
    local cy = screen.y / 2 + 40

    local left_col  = current_side == -1 and color(0, 200, 255) or color(100, 100, 100, 150)
    local back_col  = current_side == 0  and color(0, 200, 255) or color(100, 100, 100, 150)
    local right_col = current_side == 1  and color(0, 200, 255) or color(100, 100, 100, 150)

    render.text(1, vector(cx - 40, cy, 0), left_col, "c", "<")
    render.text(1, vector(cx, cy + 15, 0), back_col, "c", "V")
    render.text(1, vector(cx + 40, cy, 0), right_col, "c", ">")
end)
```
