# Namespace: `client`

The `client` namespace provides functions to register callbacks and manage the execution lifecycle of the currently running script.

---

## Methods

### `client.add_callback`
Hooks an engine or cheat event.

```lua
client.add_callback(event_name: string, callback_func: function) -> void
```
- See the [Callbacks Section](../callbacks/overview.md) for full descriptions of all 15 supported events.

---

### `client.unload_script`
Programmatically requests the cheat engine to unload the currently running script.

```lua
client.unload_script() -> void
```

#### Example
```lua
if not utils.pattern_scan("client.dll", "55 8B EC") then
    error("Signatures outdated, unloading script.")
    client.unload_script()
end
```

---

### `client.reload_script`
Unloads the current script, saves its settings, and immediately loads it again. Very useful for self-reloading during development or hot-patching.

```lua
client.reload_script() -> void
```

#### Example
```lua
-- Press F5 to hot-reload script
client.add_callback("render", function()
    if utils.is_key_pressed(0x74) then -- VK_F5
        print("Hot reloading script...")
        client.reload_script()
    end
end)
```
