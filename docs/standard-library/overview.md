# Standard Library Overview

The cheat embeds six compiled-in pure Lua standard libraries into `package.preload`. They require no external files and are immediately available to any script via `require(...)`.

---

## Module Index

| Module | Purpose |
| :--- | :--- |
| [`require("vector")`](vector.md) | Vector math extensions (angles between vectors, reflection, projection, normalization, distance2d). |
| [`require("color")`](color.md) | Color conversion (HSV, hex string parsing, float conversion, rainbow cycles, lightening/darkening). |
| [`require("timer")`](timer.md) | Delayed execution (`after`), repeated intervals (`every`), stopwatch, and framerate-independent damping. |
| [`require("ease")`](ease.md) | Mathematical easing functions (Linear, Quad, Cubic, Quart, Quint, Sine, Expo, Circ, Back, Elastic, Bounce). |
| [`require("animate")`](animate.md) | Keyframe and tweening animation engine with pause, resume, looping, and completion callbacks. |
| [`require("class")`](class.md) | Object-oriented programming (OOP) class creation, inheritance (`super`), and method mixins. |

---

## Updating Timers and Animations

> [!IMPORTANT]
> Both `timer` and `animate` rely on a tick/frame pump to advance their internal timestamps and trigger callbacks.
> You must call `timer.update()` and `animate.update()` every frame inside your `"render"` or `"createmove"` callback:
>
> ```lua
> local timer   = require("timer")
> local animate = require("animate")
>
> client.add_callback("render", function()
>     timer.update()
>     animate.update()
>     -- drawing code...
> end)
> ```
