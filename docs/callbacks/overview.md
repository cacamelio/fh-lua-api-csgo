# Callbacks Overview

Callbacks are event listeners registered through `client.add_callback`. They allow your script to execute code in response to specific game loop stages, user commands, rendering passes, network packets, or cheat operations.

---

## Registering a Callback

```lua
client.add_callback(event_name: string, callback_func: function) -> void
```

### Parameters
- `event_name`: Case-sensitive name of the event to hook.
- `callback_func`: The function executed when the event triggers.

---

## Complete Callback Reference Table

| Callback Name | Arguments Passed to Callback | Return Value | Frequency / Context |
| :--- | :--- | :--- | :--- |
| [`"render"`](render.md) | *None* | *None* | Every frame in Direct3D DrawHook. |
| [`"createmove"`](createmove.md) | `cmd` ([`user_cmd_t`](../types/user-cmd.md)) | *None* | Every tick in client `CreateMove`. |
| [`"antiaim"`](antiaim.md) | `ctx` ([`antiaim_context_t`](../types/user-cmd.md)) | *None* | Before Anti-Aim angle calculations. |
| [`"aim_shot"`](aimbot.md) | `shot` ([`shot_t`](../types/shot-data.md)) | *None* | Fired when ragebot shoots a bullet. |
| [`"aim_ack"`](aimbot.md) | `shot` ([`shot_t`](../types/shot-data.md)) | *None* | Fired when shot registers or misses on server. |
| [`"game_events"`](game-events.md) | `event` ([`game_event_t`](../types/shot-data.md)) | *None* | Fired on subscribed engine game events. |
| [`"pre_anim_update"`](animations.md) | `local_player` ([`player_t`](../types/entity-and-player.md)) | *None* | Fired before local player animation update. |
| [`"post_anim_update"`](animations.md) | `local_player` ([`player_t`](../types/entity-and-player.md)) | *None* | Fired after local player animation update. |
| [`"local_alpha"`](animations.md) | `local_player`, `current_alpha` | `number` (optional) | Fired when calculating local model transparency. Return float to override. |
| [`"draw_chams"`](chams.md) | `entity_type` (`integer`) | `integer` (optional) | Fired before rendering model chams. Return `0` (skip) or `1` (cancel). |
| [`"frame_stage"`](lifecycle-and-misc.md) | `stage` (`integer`) | *None* | Called on `FrameStageNotify` (see [EClientFrameStage](../constants-and-enums/frame-stages.md)). |
| [`"level_init"`](lifecycle-and-misc.md) | *None* | *None* | Called when entering a new map level. |
| [`"unload"`](lifecycle-and-misc.md) | *None* | *None* | Called right before the script is unloaded. |
| [`"voice_message"`](lifecycle-and-misc.md) | `data` ([`recv_voice_data_t`](../types/netchannel-and-voice.md)) | *None* | Fired when incoming voice network data is received. |
| [`"console_input"`](lifecycle-and-misc.md) | `command` (`string`) | *None* | Fired when a command is entered into cheat console. |

---

## Automatic Cleanup

When your script unloads (either manually via the menu or by reloading), all callbacks registered by that script are **automatically unregistered** by the cheat engine. You do not need to manually unhook events in your `"unload"` callback.
