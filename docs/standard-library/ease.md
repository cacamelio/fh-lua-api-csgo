# Standard Library: `ease`

```lua
local ease = require("ease")
```

The `ease` standard library contains 20+ mathematical easing functions. Every easing function accepts a normalized progress fraction $t \in [0.0, 1.0]$ and returns the eased progress factor $e \in [0.0, 1.0]$.

---

## Available Easing Functions

| Easing Category | Functions |
| :--- | :--- |
| **Linear** | `ease.linear(t)` |
| **Quadratic ($t^2$)** | `ease.in_quad(t)`, `ease.out_quad(t)`, `ease.in_out_quad(t)` |
| **Cubic ($t^3$)** | `ease.in_cubic(t)`, `ease.out_cubic(t)`, `ease.in_out_cubic(t)` |
| **Quartic ($t^4$)** | `ease.in_quart(t)`, `ease.out_quart(t)`, `ease.in_out_quart(t)` |
| **Quintic ($t^5$)** | `ease.in_quint(t)`, `ease.out_quint(t)`, `ease.in_out_quint(t)` |
| **Sinusoidal** | `ease.in_sine(t)`, `ease.out_sine(t)`, `ease.in_out_sine(t)` |
| **Exponential** | `ease.in_expo(t)`, `ease.out_expo(t)`, `ease.in_out_expo(t)` |
| **Circular** | `ease.in_circ(t)`, `ease.out_circ(t)`, `ease.in_out_circ(t)` |
| **Back (Overshoot)** | `ease.in_back(t)`, `ease.out_back(t)`, `ease.in_out_back(t)` |
| **Elastic** | `ease.in_elastic(t)`, `ease.out_elastic(t)`, `ease.in_out_elastic(t)` |
| **Bounce** | `ease.in_bounce(t)`, `ease.out_bounce(t)`, `ease.in_out_bounce(t)` |

---

## `ease.apply`

Interpolates between two numeric values using any easing function.

```lua
ease.apply(easing_fn: function, start_val: number, end_val: number, t: number) -> number
```

### Example
```lua
local progress = 0.5 -- Halfway through 1-second animation
local current_y = ease.apply(ease.out_bounce, 0, 300, progress)
```
