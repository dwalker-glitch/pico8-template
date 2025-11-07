# Top-Down Collision and Check

To stop the player from moving through walls, we must use the Move-and-Check Rule separately for the X-axis (left/right) and the Y-axis (up/down).

## The collide(o) Function

The collision function for top-down games is exactly the same as for platformers: it checks the four corners of the 8x8 player sprite to see if any of them are overlapping a solid map tile (a tile with Flag 0 set).

```
function collide(o)
    -- Calculate the four corners of the 8x8 sprite in tile coordinates
    local x1=o.x/8
    local y1=o.y/8
    local x2=(o.x+7)/8
    local y2=(o.y+7)/8
    
    -- Check Flag 0 (solid flag) for all four corners
    local a=fget(mget(x1,y1),0) -- Top-Left
    local b=fget(mget(x1,y2),0) -- Bottom-Left
    local c=fget(mget(x2,y1),0) -- Top-Right
    local d=fget(mget(x2,y2),0) -- Bottom-Right
    
    -- If ANY corner is touching a solid tile, return true
    if a or b or c or d then
        return true
    else
        return false
    end
end
```

## Full move(o) Function with Collision

This function performs the movement and checks in four distinct steps. By checking X and Y separately, the player can slide along a wall instead of stopping completely when moving diagonally into a corner.

```
function move(o)
    local lx=o.x -- Save last safe X position
    local ly=o.y -- Save last safe Y position
    
    -- CONSTANTS (If they aren't global, define them here)
    local speed = 2.0

    -- HORIZONTAL MOVEMENT & COLLISION
    
    -- 1. Apply speculative move on X-axis (Left/Right)
    if (btn(1)) o.x += speed 
    if (btn(0)) o.x -= speed 

    -- 2. Check X Collision
    if collide(o) then
        o.x=lx    -- Collision! Revert X move
    end

    -- VERTICAL MOVEMENT & COLLISION
    
    -- 3. Apply speculative move on Y-axis (Up/Down)
    if (btn(3)) o.y += speed 
    if (btn(2)) o.y -= speed 
    
    -- 4. Check Y Collision
    if collide(o) then
        o.y=ly    -- Collision! Revert Y move
    end
end
```
