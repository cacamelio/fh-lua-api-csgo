# Example: Cloud Preset Downloader

This script demonstrates using non-blocking asynchronous HTTP requests with `network.get()`, JSON parsing with `json.parse()`, and config restoration with `config.restore()` to download and load remote presets on the fly.

---

## Code (`cloud_loader.lua`)

```lua
local box = ui.groupbox("Scripts", "Cloud Config Downloader")
local url_input = box:input("Config URL")
local download_btn = box:button("Download & Apply")
local status_label = box:label("Status: Idle")

local is_downloading = false

download_btn:set_callback(function()
    if is_downloading then
        return
    end

    local url = url_input:get()
    if url == "" then
        print_raw("[Cloud] Please enter a valid URL!\n", color(255, 100, 100))
        return
    end

    is_downloading = true
    print("[Cloud] Fetching remote configuration from: " .. url)

    -- Non-blocking HTTP GET
    network.get(url, { ["User-Agent"] = "driphook-CloudClient/1.0" }, function(response_body)
        is_downloading = false

        if not response_body or response_body == "" then
            print_raw("[Cloud] Failed to download preset or empty response.\n", color(255, 80, 80))
            return
        end

        print("[Cloud] Download complete (" .. #response_body .. " bytes). Validating JSON...")

        local ok, parsed = pcall(function()
            return json.parse(response_body)
        end)

        if not ok or not parsed then
            print_raw("[Cloud] Invalid JSON response!\n", color(255, 80, 80))
            return
        end

        -- Apply to cheat config
        local applied = config.restore(response_body)
        if applied then
            print_raw("[Cloud] Remote configuration successfully loaded and applied!\n", color(100, 255, 100))
            chat_print(" [driphook] Cloud preset loaded successfully!")
        else
            print_raw("[Cloud] Error applying configuration.\n", color(255, 100, 100))
        end
    end)
end)
```
