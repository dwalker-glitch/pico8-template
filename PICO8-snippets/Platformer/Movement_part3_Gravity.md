# Platformer Physics: Gravity and Jump

This section introduces the vertical physics, using the o.vy variable we defined in Step 1. Gravity and jumping are the core components of platformer vertical movement.

## Gravity Physics (Y-Axis)

| Variable/Concept | Code Role | Explanation | 
 | ----- | ----- | ----- | 
| **`o.vy`** | Vertical Velocity | Tracks current speed and direction (positive is down, negative is up for jump). | 
| **`gravity`** | `o.vy = o.vy + gravity` | Continuously accelerates the player downward, simulating gravity. | 
| **`terminal_vel`** | Speed Clamp | Sets a maximum falling speed to prevent infinite acceleration. | 
| **Landing** | `if collide(o) then ... o.vy=0` | If the downward move causes a collision, `o.vy` is reset to `0`, stopping the player on the ground. |

---

## Ground Check Function

Before a player can jump, we must verify they are currently standing on a solid tile. This is done with a helper function that temporarily moves the player down one pixel to check for a collision.
```
-- Must be defined outside of move(o)
function is_on_ground(o)
    o.y += 1             -- Speculative move down 1 pixel
    local grounded = collide(o) -- Check collision at that new position
    o.y -= 1             -- IMPORTANT: Move back up immediately!
    return grounded      -- Returns true if collision occurred
end
```

## Jumping Logic

The jump is triggered by checking the jump button (PICO-8 button 4, typically 'Z' or 'N') and ensuring the player is currently on the ground.

---

## Updated move(o) (Gravity and Jump)

This version now includes the full vertical movement physics. It requires the collide(o) function from Step 2 and the is_on_ground(o) function from above.

-- You must define the is_on_ground(o) function above or outside of move(o)!

```
function move(o)
    local ly=o.y -- Save the last safe Y position
    
    -- CONSTANTS (Use your defined constants from Step 1)
    local gravity = 0.2
    local terminal_vel = 3
    local jump_height = -4 -- The initial upward speed
    
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
    
    -- HORIZONTAL MOVEMENT (Will be added in Step 4)
    -- ...
    
end
```
