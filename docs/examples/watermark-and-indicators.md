# Example: Watermark & Keybind Indicators

This complete script demonstrates building a modern, clean on-screen watermark and an active keybind list overlay using `render`, `ui`, and `timer`.

---

## Code (`indicators.lua`)

```lua
local timer = require("timer")

-- Setup custom UI elements in the Scripts tab
local box = ui.groupbox("Scripts", "Overlay Settings")
local watermark_cb = box:checkbox("Show Watermark", true)
local keybinds_cb = box:checkbox("Show Keybinds", true)
local accent_color = box:color_picker("Accent Color", color(0, 180, 255, 255))

local font_main = render.load_font("Tahoma", 12, "b")
local font_bold = render.load_font("Tahoma", 13, "b")

client.add_callback("render", function()
    timer.update()

    local accent = accent_color:get()
    local screen = render.screen_size()

    -- 1. WATERMARK
    if watermark_cb:get() then
        local fps = math.floor(1 / globals.frametime)
        local ping = 0
        local net = utils.get_net_channel()
        if net then
            ping = math.floor(net:get_latency(0) * 1000)
        end

        local time_str = string.format("%02d:%02d:%02d", 
            common.get_local_time().hour, 
            common.get_local_time().minute, 
            common.get_local_time().second)

        local watermark_text = string.format("driphook | %s | %d fps | %d ms | %s", 
            common.get_map_shortname() ~= "" and common.get_map_shortname() or "menu", 
            fps, ping, time_str)

        local sz = render.measure_text(font_main, watermark_text)
        local wx = screen.x - sz.x - 20
        local wy = 15

        -- Background box
        render.rect(vector(wx - 8, wy - 5, 0), vector(wx + sz.x + 8, wy + sz.y + 5, 0), color(16, 16, 20, 220), 4)
        -- Gradient accent bar on top
        render.gradient(vector(wx - 8, wy - 5, 0), vector(wx + sz.x + 8, wy - 3, 0), accent, color(255, 255, 255, 255), accent, color(255, 255, 255, 255))
        -- Text
        render.text(font_main, vector(wx, wy, 0), color(255, 255, 255, 255), "d", watermark_text)
    end

    -- 2. KEYBINDS OVERLAY
    if keybinds_cb:get() then
        local active_binds = ui.get_binds()
        
        local kx = 20
        local ky = 300
        local kw = 160
        local header_h = 24
        local row_h = 18
        local total_h = header_h + (#active_binds * row_h) + 4

        -- Header container
        render.rect(vector(kx, ky, 0), vector(kx + kw, ky + total_h, 0), color(16, 16, 20, 200), 4)
        render.line(vector(kx, ky + header_h, 0), vector(kx + kw, ky + header_h, 0), color(40, 40, 45, 255))
        render.text(font_bold, vector(kx + (kw / 2), ky + 4, 0), accent, "c", "Active Binds")

        -- Rows
        local cur_y = ky + header_h + 4
        for _, bind in ipairs(active_binds) do
            local mode_str = bind.mode == 1 and "[hold]" or "[toggle]"
            render.text(font_main, vector(kx + 8, cur_y, 0), color(240, 240, 240, 255), "", bind.name)
            render.text(font_main, vector(kx + kw - 8, cur_y, 0), color(160, 160, 160, 255), "c", mode_str)
            cur_y = cur_y + row_h
        end
    end
end)
```
