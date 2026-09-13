# Introduction

Welcome to the official **Lua API Documentation** for the CS:GO internal cheat framework. This comprehensive guide provides detailed information, function specifications, data structures, and practical examples for writing Lua scripts.

The scripting engine is powered by **LuaJIT 2.1** and exposed through modern C++ bindings using **Sol2**, giving you high performance, native interaction with game memory, mathematical utilities, direct rendering primitives, networked HTTP requests, and full UI customization.

---

## Key Features

- **Blazing Fast**: Embedded LuaJIT 2.1 with Just-In-Time compilation.
- **Deep Engine Integration**: Access to `CreateMove`, `FrameStageNotify`, Netvars, Engine Traces, AutoWall, Panorama, ConVars, and Materials.
- **Rich Rendering Pipeline**: Direct3D-based primitives, anti-aliased font management, gradients, weapon icons, 3D circles, and world-to-screen projection.
- **Exploit & Anti-Aim Manipulation**: Direct control over defensive double-tap ticks, tickbase shifting, fake lag, yaw offsets, and desync angles.
- **Built-in Standard Library**: Preloaded modular OOP (`class`), animation manager (`animate`), physics & vector math (`vector`), color utilities (`color`), high-resolution timers (`timer`), and 20+ easing curves (`ease`).
- **Asynchronous Networking**: Non-blocking HTTP GET and POST requests running on dedicated worker threads.
- **Custom UI Toolkit**: Dynamically register custom tabs, groupboxes, checkboxes, sliders, color pickers, keybinds, and combo boxes that seamlessly integrate into the main cheat interface and save state automatically.

---

## Directory Structure at a Glance

When using scripts, the cheat creates and manages the following folder hierarchy in your working directory:

```
driphook/
└── scripts/
    ├── *.lua            # Your script files
    ├── cfg/
    │   └── *.cfg        # Auto-saved and auto-loaded JSON UI configurations
    └── lib/
        ├── *.lua        # Reusable user modules
        └── */init.lua   # Package sub-folders
```

---

## Quick Navigation

| Section | Description |
| :--- | :--- |
| [**Getting Started**](getting-started/introduction.md) | Learn how scripts execute, the environment sandbox, and file loading. |
| [**Global Functions**](globals/functions.md) | Standard utilities like `print`, `chat_print`, `to_ticks`, and `math.angle_diff`. |
| [**Callbacks**](callbacks/overview.md) | Hooking into game frames, drawing, ragebot events, animations, and input. |
| [**Namespaces**](namespaces/client.md) | Core namespaces including `render`, `entity`, `ui`, `rage`, `utils`, `network`, and more. |
| [**User Types**](types/vector.md) | Classes like `vector`, `qangle`, `color`, `player_t`, `user_cmd_t`, and `shot_t`. |
| [**Standard Library**](standard-library/overview.md) | Preloaded libraries including `vector`, `color`, `timer`, `ease`, `animate`, and `class`. |
| [**Constants & Enums**](constants-and-enums/round-end-reasons.md) | Complete reference for `ERoundEndReason`, `ClassOfEntity`, and `EClientFrameStage`. |
| [**Practical Examples**](examples/watermark-and-indicators.md) | Ready-to-use scripts for watermarks, custom anti-aim, and miss loggers. |
