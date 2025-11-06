# Platformer Physics: Momentum and Friction

This is the final component of platformer movement. We use momentum (velocity on the X-axis, or o.vx) and friction to create realistic sliding movement and gradual stopping.

## Momentum & Friction Physics (X-Axis)

Horizontal movement is controlled by gradually accelerating the player and applying friction to slow them down when no buttons are pressed.

| Variable/Concept | Code Role | Explanation | 
 | ----- | ----- | ----- | 
| **`o.vx`** | Horizontal Velocity | Tracks the current left/right speed and direction. | 
| **Acceleration** | `o.vx += accel` | Player input adds a small force (`accel`) to the velocity, gradually speeding the player up. | 
| **Friction** | `o.vx *= friction` | Reduces the horizontal speed every frame, causing the player to slow down and glide to a stop when input is released. | 
| **Wall Hit** | `if collide(o) then ... o.vx=0` | If the horizontal move causes a collision, `o.vx` is reset to `0`, stopping all momentum. |

---

## Final move(o) Function (Full Physics)

This function combines all the logic from Steps 1, 2, and 3, and adds the final horizontal movement and collision checks. This is the complete platformer movement code.

Note: You must have the collide(o) function defined elsewhere in your code for this to work.

```
function move(o)
    local lx=o.x -- Save the last safe X position
    local ly=o.y -- Save the last safe Y position
    
    -- PLATFORMER CONSTANTS (Tune these values to change the feel of the game)
    local gravity = 0.2     
    local terminal_vel = 3  
    local accel = 0.5       
    local friction = 0.85   
    local max_h_vel = 3     
    local jump_height = -4  -- Added: Initial speed when jumping up

    -- VERTICAL MOVEMENT (Gravity and Jump)
    
    -- 1. APPLY JUMP INPUT
    if (btnp(4)) then
        if is_on_ground(o) then
            o.vy = jump_height
        end
    end
    
    -- 2. APPLY GRAVITY & Y-VELOCITY
    o.vy = o.vy + gravity
    
    -- Clamp max falling speed
    if o.vy > terminal_vel then o.vy = terminal_vel end 
    
    -- Apply speculative move on Y-axis
    o.y = o.y + o.vy
    
    -- 3. COLLISION CHECK (Y AXIS)
    if collide(o) then
        o.y=ly    -- Collision! Move back to safe position
        o.vy=0    -- Stop vertical velocity (the player has landed or hit a ceiling)
    end
    
    
    -- HORIZONTAL MOVEMENT (Momentum and Friction)
    
    -- 4. APPLY HORIZONTAL INPUT & FRICTION (MOMENTUM)
    if (btn(1)) o.vx += accel -- Right: increase velocity
    if (btn(0)) o.vx -= accel -- Left: decrease velocity
    
    o.vx *= friction -- Apply friction: slow down velocity every frame
    
    -- Clamp max speed using PICO-8's sgn() to maintain the correct direction
    if abs(o.vx) > max_h_vel then
        o.vx = max_h_vel * sgn(o.vx)
    end
    
    -- Apply speculative move on X-axis
    o.x = o.x + o.vx
    
    -- 5. COLLISION CHECK (X AXIS)
    if collide(o) then
        o.x=lx    -- Collision! Move back to safe position
        o.vx=0    -- Stop horizontal velocity (the player has hit a wall)
    end
end
```
