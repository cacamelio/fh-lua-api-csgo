# Environment & Architecture

This cheat features an embedded **LuaJIT 2.1** scripting runtime interfaced with the C++ internal engine using **Sol2 v3**.

---

## Technical Specifications

| Property | Details |
| :--- | :--- |
| **Lua Engine** | LuaJIT 2.1.0-beta3 |
| **Binding Library** | Sol2 (Safe function calls and type bindings enabled) |
| **Sandboxing** | Each script runs in an isolated `sol::environment` inheriting from the global table. |
| **Standard Libraries** | `base`, `string`, `math`, `table`, `debug`, `package`, `jit`, `ffi`, `bit32`, `os` |
| **Preloaded Modules** | `vector`, `color`, `timer`, `ease`, `animate`, `class` (embedded in binary) |
| **Error Handling** | Traceback formatted and displayed directly to the internal developer console with call stacks. |

---

## Script Isolation & Environments

When a script is loaded:
1. A fresh `sol::environment` is instantiated with the cheat's global environment as its parent fallback.
2. Global definitions inside your script do not overwrite globals of another script.
3. Callbacks and UI elements registered by your script are tracked by an internal script ID.
4. When a script is unloaded:
   - Its `"unload"` callback is invoked.
   - All registered callbacks in `client.add_callback` are automatically removed.
   - All UI widgets created by the script in tabs or groupboxes are cleanly removed from the menu tree.
   - All UI values are saved to disk in `driphook/scripts/cfg/<name>.cfg`.
   - The script environment is scheduled for safe delayed deallocation (5 seconds cooldown to ensure in-flight references finish cleanly).

> [!NOTE]
> Because scripts run in isolated environments, global variables declared without `local` will stay confined to your script's environment. To share code or states between scripts, create shared Lua modules in `driphook/scripts/lib/` or write to persistent JSON files using the `files` and `json` namespaces.

---

## Threading & Safety

- **Game Thread Hooking**: The majority of callbacks (`render`, `createmove`, `antiaim`, `frame_stage`) execute synchronously on the main game thread or render thread.
- **Asynchronous HTTP**: HTTP requests made with `network.get()` or `network.post()` that provide a callback function are dispatched to a background worker thread. When the request finishes, the response is buffered into a thread-safe queue and dispatched back to the Lua callback on the game thread.
- **Error Protection**: Sol2 operates with `SOL_SAFE_FUNCTION_CALLS` enabled. Lua exceptions, runtime syntax errors, or nil indexing errors are intercepted by `LuaErrorHandler`, which logs a formatted red traceback to the cheat console rather than crashing the game.
