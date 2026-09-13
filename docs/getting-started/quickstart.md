# Quickstart Tutorial

This step-by-step tutorial will guide you through creating, loading, and customizing your first Lua script.

---

## 1. Creating Your Script

1. Navigate to your CS:GO game folder: `<CSGO_FOLDER>/driphook/scripts/`.
2. Create a new text file named `hello_world.lua`.
3. Open `hello_world.lua` in your preferred code editor (such as VS Code).

---

## 2. Writing a Basic Script

Copy and paste the following snippet into `hello_world.lua`:

```lua
-- hello_world.lua
print("Hello World script initialized!")

-- 1. Create a custom UI groupbox on the "Scripts" tab
local my_box = ui.groupbox("Scripts", "Hello World Settings")

-- 2. Add interactive controls
local enabled_cb = my_box:checkbox("Enable Watermark", true)
local watermark_color = my_box:color_picker("Accent Color", color(0, 160, 255, 255))
local font = render.load_font("Tahoma", 13, "b")

-- 3. Hook the render callback to draw on screen
client.add_callback("render", function()
    -- Only render if our checkbox is checked
    if not enabled_cb:get() then
        return
    end

    local text_str = "driphook | Hello World | FPS: " .. math.floor(1 / globals.frametime)
    local col = watermark_color:get()

    -- Measure text size to draw a background box
    local text_size = render.measure_text(font, text_str)
    local screen = render.screen_size()

    local pos_x = screen.x - text_size.x - 20
    local pos_y = 15

    -- Draw background rectangle
    render.rect(vector(pos_x - 6, pos_y - 4, 0), vector(pos_x + text_size.x + 6, pos_y + text_size.y + 4, 0), color(20, 20, 20, 200), 4)

    -- Draw colored outline
    render.rect_outline(vector(pos_x - 6, pos_y - 4, 0), vector(pos_x + text_size.x + 6, pos_y + text_size.y + 4, 0), col, 4)

    -- Draw the text
    render.text(font, vector(pos_x, pos_y, 0), color(255, 255, 255, 255), "", text_str)
end)

-- 4. Listen for script unload to clean up or log
client.add_callback("unload", function()
    print("Hello World script unloaded successfully!")
end)
```

---

## 3. Loading the Script in the Cheat

1. Inject or launch the cheat in CS:GO.
2. Open the cheat menu (default key: `Insert`).
3. Navigate to the **Scripts / Config** tab.
4. Click **Refresh Scripts**. You will see `hello_world` listed in the script browser.
5. Select `hello_world` and click **Load**.
6. The script name will now show an asterisk (`* hello_world`), indicating it is running.
7. You will immediately see your custom groupbox **Hello World Settings** appear with the checkbox and color picker, and the watermark rendered in the top-right corner of the screen!

---

## 4. Reloading and Modifying

- If you make changes to your `.lua` file, simply click **Unload** and then **Load** again, or programmatically call:
  ```lua
  client.reload_script()
  ```
- Any UI settings you changed (e.g. customized color) will persist across unloads and reloads because they are saved to `driphook/scripts/cfg/hello_world.cfg`.
