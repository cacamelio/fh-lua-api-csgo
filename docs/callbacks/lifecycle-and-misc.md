# Lifecycle & Other Hooks

This section covers script lifecycle, level transitions, console input interception, voice packets, and engine frame stages.

---

## 1. `"level_init"`

Triggered when the client connects to a new level or map.

### Signature
```lua
client.add_callback("level_init", function()
    print("Entered map: " .. common.get_map_name())
end)
```

---

## 2. `"unload"`

Triggered when your script is being unloaded (either by user clicking Unload, cheat unloading, or right before a script reloads).

### Signature
```lua
client.add_callback("unload", function()
    -- Free custom resources, save data to file, etc.
    print("Script cleanup executed.")
end)
```

> [!NOTE]
> Registered callbacks, timers, and UI widgets are cleaned up automatically by the C++ engine. Use this callback for custom cleanup such as resetting ConVars or saving custom file data.

---

## 3. `"frame_stage"`

Triggered on every engine `FrameStageNotify` call.

### Signature
```lua
client.add_callback("frame_stage", function(stage)
    -- stage is an integer matching EClientFrameStage
end)
```

### Stage Constants (`EClientFrameStage`)
| Value | Constant Name | Description |
| :---: | :--- | :--- |
| `-1` | `FRAME_UNDEFINED` | Undefined frame stage. |
| `0` | `FRAME_START` | Frame starting. |
| `1` | `FRAME_NET_UPDATE_START` | Network packets received, processing begins. |
| `2` | `FRAME_NET_UPDATE_POSTDATAUPDATE_START` | Entity data updated, before client animations. |
| `3` | `FRAME_NET_UPDATE_POSTDATAUPDATE_END` | Entity data update finished. |
| `4` | `FRAME_NET_UPDATE_END` | Network update complete. |
| `5` | `FRAME_RENDER_START` | World render setup beginning. |
| `6` | `FRAME_RENDER_END` | World render finished. |

### Example
```lua
client.add_callback("frame_stage", function(stage)
    if stage == 5 then -- FRAME_RENDER_START
        -- Inspect or modify viewmodels or local angles before rendering
    end
end)
```

---

## 4. `"console_input"`

Triggered when the user enters a command into the internal cheat console.

### Signature
```lua
client.add_callback("console_input", function(cmd_string)
    if cmd_string == "myscript_reset" then
        print("Reset command received!")
    end
end)
```

---

## 5. `"voice_message"`

Triggered whenever an incoming `CSVCMsg_VoiceData` voice packet is received from the server network stream.

### Signature
```lua
client.add_callback("voice_message", function(voice_data)
    -- voice_data is a recv_voice_data_t object
    local client_player = voice_data.client
    local payload = voice_data:get_voice_data()
end)
```

See [NetChannel & Voice Types](../types/netchannel-and-voice.md) for full field descriptions of `voice_data`.
