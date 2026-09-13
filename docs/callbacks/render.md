# Render Callback (`"render"`)

The `"render"` callback executes every frame during Direct3D presentation. This is the designated hook for all on-screen graphics, overlays, ESP indicators, custom menus, visual alerts, and world-space drawings.

---

## Signature

```lua
client.add_callback("render", function()
    -- Drawing code here
end)
```

- **Arguments**: None.
- **Return Value**: None.

---

## Best Practices

> [!TIP]
> - Do not perform expensive calculations, disk I/O, or heavy loops inside `"render"`. Keep this hook optimized for drawing commands via the [`render.*`](../namespaces/render.md) namespace.
> - Call `require("timer").update()` and `require("animate").update()` inside `"render"` if you use the standard animation or timing libraries.
> - Check `globals.is_in_game` before accessing entity coordinates or projecting world-to-screen to avoid unnecessary calculations while on main menu.

---

## Example: Drawing an On-Screen Watermark and Crosshair

```lua
local font = render.load_font("Tahoma", 12, "b")

client.add_callback("render", function()
    local screen = render.screen_size()
    local cx = screen.x / 2
    local cy = screen.y / 2

    -- Draw subtle center crosshair dot
    render.rect(vector(cx - 1, cy - 1, 0), vector(cx + 2, cy + 2, 0), color(255, 255, 255, 220), 0)

    -- Draw watermark
    local watermark_text = "driphook.lua | " .. common.get_map_shortname()
    local sz = render.measure_text(font, watermark_text)
    local x = screen.x - sz.x - 15
    local y = 15

    -- Background with rounded corners
    render.rect(vector(x - 6, y - 4, 0), vector(x + sz.x + 6, y + sz.y + 4, 0), color(15, 15, 15, 220), 3)
    -- Top accent line
    render.line(vector(x - 6, y - 4, 0), vector(x + sz.x + 6, y - 4, 0), color(0, 180, 255, 255))
    -- Text with shadow
    render.text(font, vector(x, y, 0), color(255, 255, 255, 255), "d", watermark_text)
end)
```

---

## World-to-Screen in Render

To draw indicators or text over 3D world positions (e.g., player heads, dropped weapons, C4 bomb):

```lua
client.add_callback("render", function()
    if not globals.is_in_game then return end

    local local_player = entity.get_local_player()
    if not local_player or not local_player:is_alive() then return end

    for _, player in ipairs(entity.get_players(true)) do -- enemies only
        local head_pos = player:get_hitbox_position(0) -- Head hitbox
        local screen_pos = head_pos:to_screen() -- or render.world_to_screen(head_pos)

        -- to_screen() returns a vector with x and y coordinates
        if screen_pos.x > 0 and screen_pos.y > 0 then
            render.text(1, screen_pos, color(255, 50, 50, 255), "c", player:get_name())
        end
    end
end)
```
