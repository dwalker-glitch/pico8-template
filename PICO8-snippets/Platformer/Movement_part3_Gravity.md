# Platformer Physics: Gravity and Jump

This section introduces the vertical physics, using the o.vy variable we defined in Step 1. Gravity is simply an acceleration that happens every frame.

## Gravity Physics (Y-Axis)

Gravity is handled by continuously increasing the vertical velocity (o.vy) and checking for collision after the movement is applied.

| Variable/Concept | Code Role | Explanation | 
 | ----- | ----- | ----- | 
| **`o.vy`** | Vertical Velocity | Tracks current speed and direction (positive is down, negative is up for jump). | 
| **`gravity`** | `o.vy = o.vy + gravity` | Continuously accelerates the player downward, simulating gravity. | 
| **`terminal_vel`** | Speed Clamp | Sets a maximum falling speed to prevent infinite acceleration. | 
| **Landing** | `if collide(o) then ... o.vy=0` | If the downward move causes a collision, `o.vy` is reset to `0`, stopping the player on the ground. |

---

## Updated move(o) (Gravity/Y-Axis Only)

This version now includes the full vertical physics. It requires the collide(o) function from Step 2 to be defined elsewhere in your code.

-- Remember to define your constants (gravity, terminal_vel) in _init or move(o)!

```
function move(o)
    local ly=o.y -- Save the last safe Y position
    
    -- JUMP LOGIC: If a jump is pressed, set a negative vy value here (will be added later)
    -- ...
    
    -- 1. APPLY GRAVITY & Y-VELOCITY
    o.vy = o.vy + 0.2 -- Use your gravity constant here
    
    -- Clamp max falling speed
    if o.vy > 3 then o.vy = 3 end -- Use your terminal_vel constant here
    
    -- Apply speculative move on Y-axis
    o.y = o.y + o.vy
    
    -- 2. COLLISION CHECK (Y AXIS)
    if collide(o) then
        o.y=ly    -- Collision! Move back to safe position
        o.vy=0    -- Stop vertical velocity (the player has landed or hit a ceiling)
    end
    
    -- HORIZONTAL MOVEMENT (Will be added in Step 4)
    -- ...
    
end
```
