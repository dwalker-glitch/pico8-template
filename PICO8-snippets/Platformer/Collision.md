# Platformer Collision Check: collide(o)

Collision is the foundation of platformer games. We use a function called collide(o) to determine if the player's 8x8 sprite is currently overlapping a solid map tile (a tile with Flag 0 set).

## The collide(o) Function

```
function collide(o)
    -- Calculate the four corners of the 8x8 sprite in tile coordinates
    local x1=o.x/8
    local y1=o.y/8
    local x2=(o.x+7)/8
    local y2=(o.y+7)/8
    
    -- Check Flag 0 (solid flag) for all four corners using fget() and mget()
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

## Four-Corner Check

Since collision is based on 8x8 tiles, you must check all four corners to ensure no part of the sprite is inside a wall. We use o.x+7 and o.y+7 to find the exact bottom-right edge of the 8x8 sprite.

### The Move-and-Check Rule

The most important rule for platformer collision is the Move-and-Check Rule:

1. Move the player speculatively (e.g., move them down by o.vy).
2. Check for collision using collide(o).
3. If collision occurs, undo the move and stop the velocity.

This is how we prevent the player from getting stuck inside walls.
