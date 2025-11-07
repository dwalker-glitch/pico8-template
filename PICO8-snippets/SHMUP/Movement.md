# PICO-8 Space Shooter Movement and Boundary Check

Movement in a space shooter (sometimes called a vertical or horizontal scroller) is typically direct, constant, and focused on keeping the player inside the screen boundaries. There is no complex physics like gravity or momentum.

---

## The move(o) Function: Direct Control

This function takes player input and directly updates the player's position based on the o.speed value, followed by a check to ensure the player remains on screen.

### Direct Movement Explanation

Pressing an arrow button instantly moves the player by the o.speed amount every frame.

### Screen Boundary Check

The PICO-8 built-in function mid(min, val, max) is used to "clamp" the player's position, ensuring they cannot move off-screen.
| Type | Line | Code | Explanation | 
 | :--- | :--- | :--- | :--- | 
| **Direct Movement** | `2-5` | `if (btn(0)) o.x -= o.speed` | Checks for input and immediately modifies the `x` or `y` position by the `speed` value every frame. | 
| **Boundary Check** | `9` | `o.x = mid(0, o.x, 128 - o.w)` | Clamps the player's X position between: **0** (the left edge) and **128 - player width** (the right edge). | 
| **Boundary Check** | `12` | `o.y = mid(0, o.y, 128 - o.h)` | Clamps the player's Y position between: **0** (the top edge) and **128 - player height** (the bottom edge). |

---

### move(o) Code

```
function move(o)
    -- Apply direct movement based on speed
    if (btn(0)) o.x -= o.speed -- Left
    if (btn(1)) o.x += o.speed -- Right
    if (btn(2)) o.y -= o.speed -- Up
    if (btn(3)) o.y += o.speed -- Down
    
    -- Boundary check (Clamping the position)
    -- PICO-8 screen size is 128x128
    
    -- X-Axis Clamp: Keep player between 0 and 128 - width
    o.x = mid(0, o.x, 128 - o.w)
    
    -- Y-Axis Clamp: Keep player between 0 and 128 - height
    o.y = mid(0, o.y, 128 - o.h)
end
```

