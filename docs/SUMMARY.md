# Summary

* [Introduction](README.md)

## Getting Started
* [Environment & Architecture](getting-started/introduction.md)
* [File Structure & Modules](getting-started/file-structure.md)
* [Quickstart Tutorial](getting-started/quickstart.md)

## Globals
* [Global Functions](globals/functions.md)
* [Globals Table (`globals.*`)](globals/globals-table.md)

## Callbacks & Events
* [Callbacks Overview](callbacks/overview.md)
* [Render (`"render"`)](callbacks/render.md)
* [CreateMove (`"createmove"`)](callbacks/createmove.md)
* [Anti-Aim (`"antiaim"`)](callbacks/antiaim.md)
* [Aimbot & Shots (`"aim_shot"`, `"aim_ack"`)](callbacks/aimbot.md)
* [Game Events (`"game_events"`)](callbacks/game-events.md)
* [Animation Hooks (`"pre_anim_update"`, `"post_anim_update"`, `"local_alpha"`)](callbacks/animations.md)
* [Chams Override (`"draw_chams"`)](callbacks/chams.md)
* [Lifecycle & Other Hooks](callbacks/lifecycle-and-misc.md)

## Namespaces
* [client](namespaces/client.md)
* [entity](namespaces/entity.md)
* [ui](namespaces/ui.md)
* [render](namespaces/render.md)
* [rage](namespaces/rage.md)
* [utils](namespaces/utils.md)
* [network](namespaces/network.md)
* [files](namespaces/files.md)
* [json](namespaces/json.md)
* [cvar](namespaces/cvar.md)
* [plist](namespaces/plist.md)
* [materials](namespaces/materials.md)
* [panorama](namespaces/panorama.md)
* [common](namespaces/common.md)
* [config](namespaces/config.md)

## User Types
* [vector](types/vector.md)
* [qangle](types/qangle.md)
* [color](types/color.md)
* [entity_t, player_t & weapon_t](types/entity-and-player.md)
* [animstate_t & animlayer_t](types/animstate-and-layers.md)
* [user_cmd_t](types/user-cmd.md)
* [shot_t & lag_record_t](types/shot-data.md)
* [ui_element_t & groupbox_t](types/ui-widgets.md)
* [material_t & image_t](types/material.md)
* [trace_t & fire_bullet_t](types/trace.md)
* [net_channel_info_t & recv_voice_data_t](types/netchannel-and-voice.md)
* [ui_panel_t](types/panorama-panel.md)

## Standard Library (`require`)
* [Overview](standard-library/overview.md)
* [vector](standard-library/vector.md)
* [color](standard-library/color.md)
* [timer](standard-library/timer.md)
* [ease](standard-library/ease.md)
* [animate](standard-library/animate.md)
* [class](standard-library/class.md)

## Constants & Enums
* [Round End Reasons (`ERoundEndReason`)](constants-and-enums/round-end-reasons.md)
* [Entity Classes (`ClassOfEntity`)](constants-and-enums/entity-classes.md)
* [Client Frame Stages (`EClientFrameStage`)](constants-and-enums/frame-stages.md)
* [UI Widget Types](constants-and-enums/widget-types.md)

## Practical Examples
* [Watermark & Keybind Indicators](examples/watermark-and-indicators.md)
* [Custom Anti-Aim & Fake Lag](examples/custom-anti-aim.md)
* [Shot & Miss Logger](examples/shot-logger.md)
* [Cloud Preset Downloader](examples/cloud-config-loader.md)
