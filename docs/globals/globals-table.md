# Globals Table (`globals.*`)

The `globals` table contains read-only variables providing real-time game simulation data, engine clock drift parameters, network choking, and round state.

---

## Variable Reference

| Property | Type | Description |
| :--- | :--- | :--- |
| `globals.curtime` | `number` | Current game simulation time in seconds (advances per tick during gameplay). |
| `globals.realtime` | `number` | Real-world continuous elapsed time since game launch in seconds. |
| `globals.frametime` | `number` | Time spent rendering the last frame in seconds ($1 / \text{frametime} \approx \text{FPS}$). |
| `globals.framecount` | `integer` | Total number of frames rendered since game launch. |
| `globals.tickcount` | `integer` | Current client tick counter. |
| `globals.tickinterval` | `number` | Seconds per simulation tick ($0.015625$ for 64-tick, $0.0078125$ for 128-tick). |
| `globals.max_players` | `integer` | Maximum players allowed on current server (`maxclients`). |
| `globals.is_connected` | `boolean` | `true` if connected to a server or local lobby. |
| `globals.is_in_game` | `boolean` | `true` if actively inside an active map round. |
| `globals.choked_commands` | `integer` | Number of choked packets queued for transmission by client state. |
| `globals.commandack` | `integer` | Sequence number of the most recent user command acknowledged by server. |
| `globals.commandack_prev` | `integer` | Sequence number of the previous command acknowledged. |
| `globals.last_outgoing_command` | `integer` | Most recent user command index sent to the server. |
| `globals.server_tick` | `integer` | Server tick tracked by engine clock drift manager (`m_nServerTick`). |
| `globals.client_tick` | `integer` | Client tick tracked by engine clock drift manager (`m_nClientTick`). |
| `globals.delta_tick` | `integer` | Delta tick index for network delta compression. |
| `globals.clock_offset` | `integer` | Current clock offset between client and server clock drift. |
| `globals.chat_opened` | `boolean` | `true` if the in-game text chat input is currently open. |
| `globals.last_round_end_reason`| `integer` | Reason code for why the last round ended (see [ERoundEndReason](../constants-and-enums/round-end-reasons.md)). |

---

## Example Usage

### 1. FPS and Tickrate Monitor
```lua
client.add_callback("render", function()
    local fps = math.floor(1 / globals.frametime)
    local tickrate = math.floor(1 / globals.tickinterval + 0.5)
    
    render.text(1, vector(20, 20, 0), color(255, 255, 255), "", 
        string.format("FPS: %d | Tickrate: %d | Choked: %d", fps, tickrate, globals.choked_commands))
end)
```

### 2. Checking Game State Before Logic
```lua
client.add_callback("createmove", function(cmd)
    -- Don't run movement logic if not connected or chat is open
    if not globals.is_in_game or globals.chat_opened then
        return
    end

    -- Process commands...
end)
```

### 3. Round End Reason Detection
```lua
client.add_callback("game_events", function(event)
    if event:get_name() == "round_end" then
        local reason = globals.last_round_end_reason
        print("Round ended with reason ID: " .. tostring(reason))
        if reason == 8 then
            print("CTs Won the round!")
        elseif reason == 9 then
            print("Terrorists Won the round!")
        end
    end
end)
```
