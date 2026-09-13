# Namespace: `network`

The `network` namespace provides both synchronous and non-blocking asynchronous HTTP/HTTPS GET and POST networking capabilities.

---

## Methods

### `network.get`
Performs an HTTP GET request to a URL.

```lua
-- Asynchronous (non-blocking)
network.get(url: string, headers?: table, callback?: function) -> void

-- Synchronous (blocking)
network.get(url: string, headers?: table) -> string
```

#### Parameters
- `url`: Destination web URL (e.g. `"https://api.github.com/..."`).
- `headers` *(optional)*: Table of key-value header strings (e.g. `{ ["User-Agent"] = "MyScript/1.0" }`).
- `callback` *(optional)*: When supplied, the request runs in a background thread and calls `callback(response_string)` on the game thread when finished! If omitted, the function blocks the current frame until the response is received.

#### Asynchronous Example
```lua
network.get("https://raw.githubusercontent.com/example/presets/main/cfg.json", nil, function(data)
    print("Received preset data! Length: " .. #data)
    local preset = json.parse(data)
    -- Process preset...
end)
```

---

### `network.post`
Performs an HTTP POST request to a URL with a request body payload.

```lua
-- Asynchronous (non-blocking)
network.post(url: string, body?: string, headers?: table, callback?: function) -> void

-- Synchronous (blocking)
network.post(url: string, body?: string, headers?: table) -> string
```

#### Parameters
- `url`: Target web address.
- `body` *(optional)*: Raw string or serialized JSON body.
- `headers` *(optional)*: Table of key-value header strings (e.g. `{ ["Content-Type"] = "application/json" }`).
- `callback` *(optional)*: Asynchronous completion callback receiving `(response_string)`.

#### Discord Webhook Example
```lua
local function send_discord_log(message)
    local webhook_url = "https://discord.com/api/webhooks/YOUR_WEBHOOK_URL"
    local payload = json.stringify({
        content = message,
        username = "driphook Logger"
    })

    network.post(webhook_url, payload, { ["Content-Type"] = "application/json" }, function(res)
        print("Webhook sent successfully.")
    end)
end
```
