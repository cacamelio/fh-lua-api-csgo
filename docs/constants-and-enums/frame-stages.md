# Enum Reference: `EClientFrameStage`

The `EClientFrameStage` integer enumeration is passed to the [`"frame_stage"`](../callbacks/lifecycle-and-misc.md) callback on every execution of `IBaseClientDLL::FrameStageNotify`.

---

## Enumeration Table

| Value | Identifier | Stage Description |
| :---: | :--- | :--- |
| `-1` | `FRAME_UNDEFINED` | Frame stage is uninitialized or outside game loop. |
| `0` | `FRAME_START` | Initial phase of engine frame simulation. |
| `1` | `FRAME_NET_UPDATE_START` | Network packets received from server; parsing begins. |
| `2` | `FRAME_NET_UPDATE_POSTDATAUPDATE_START` | Entity network updates applied; before animation processing. |
| `3` | `FRAME_NET_UPDATE_POSTDATAUPDATE_END` | Post-data update phase complete; animations and bones updated. |
| `4` | `FRAME_NET_UPDATE_END` | Full network packet update sequence complete. |
| `5` | `FRAME_RENDER_START` | Scene view rendering begins (viewmodel, player models, third-person). |
| `6` | `FRAME_RENDER_END` | Scene rendering finished; HUD and post-processing. |

---

## Example Usage

```lua
client.add_callback("frame_stage", function(stage)
    if stage == 2 then -- FRAME_NET_UPDATE_POSTDATAUPDATE_START
        -- Inspect fresh entity network positions right before client animations
    elseif stage == 5 then -- FRAME_RENDER_START
        -- Modify view angles or viewmodel before world rendering
    end
end)
```
