# Standard Library: `class`

```lua
local class = require("class")
```

The `class` standard library provides an intuitive, robust Object-Oriented Programming (OOP) class model with constructors, inheritance, superclass method resolution, and mixins.

---

## Defining a Class

Calling `class()` creates a new class prototype. Define an `:init(...)` method to serve as the constructor:

```lua
local UIComponent = class()

function UIComponent:init(x, y, w, h)
    self.x = x
    self.y = y
    self.w = w
    self.h = h
    self.visible = true
end

function UIComponent:draw()
    if not self.visible then return end
    render.rect(vector(self.x, self.y, 0), vector(self.x + self.w, self.y + self.h, 0), color(40, 40, 40))
end
```

### Instantiation
Instantiate objects by calling the class directly:

```lua
local my_box = UIComponent(50, 50, 200, 100)
my_box:draw()
```

---

## Inheritance

Pass a parent class to `class(ParentClass)` to derive a child class:

```lua
local Button = class(UIComponent)

function Button:init(x, y, w, h, text, on_click)
    -- Call superclass constructor
    self:super().init(self, x, y, w, h)
    self.text = text
    self.on_click = on_click
end

function Button:draw()
    -- Override draw method
    self:super().draw(self)
    render.text(1, vector(self.x + 10, self.y + 10, 0), color(255, 255, 255), "", self.text)
end
```

---

## Type Checking & Mixins

### `instance:is_a`
Checks if an instance is of a class or inherits from it.

```lua
local btn = Button(0, 0, 100, 30, "Click Me")
print(btn:is_a(Button))      -- true
print(btn:is_a(UIComponent)) -- true
```

---

### `class.mixin`
Copies methods from one or more tables into a target class prototype.

```lua
local DraggableMixin = {
    drag = function(self, dx, dy)
        self.x = self.x + dx
        self.y = self.y + dy
    end
}

class.mixin(UIComponent, DraggableMixin)
```
