# RPG Map Transitions

In a classic top-down RPG, the entire world is stored in PICO-8's single, large map memory (128 tiles wide by 32 tiles high). The challenge is not loading a new map, but teleporting the player and changing the viewport (what part of the big map is drawn).

## 1. PICO-8 Map Structure

The PICO-8 screen is 16 tiles wide (128 pixels) by 16 tiles high (128 pixels).

The map editor can hold a total of 16 screens horizontally (128 / 8) and 4 screens vertically (32 / 8).

We need two global variables, map_screen_x and map_screen_y, to track which of these screens is currently visible.

| Variable | Initial Value | Purpose |
| :--- | :--- | :--- |
| `map_screen_x` | `0` | Stores the **tile column** where the current screen begins (e.g., 0, 16, 32, 48...). |
| `map_screen_y` | `0` | Stores the **tile row** where the current screen begins (e.g., 0, 16). |

### Drawing the Map

Your _draw() function must now use these variables with the PICO-8 map() function to draw the correct screen:

```
-- map( cel_x, cel_y, sx, sy, w, h )
map(map_screen_x, map_screen_y, 0, 0, 16, 16)
```

## 2. Transition Logic (check_map_transition)

This function runs after the player has moved (in _update) and checks if the player's position (o.x, o.y) has crossed the 128-pixel screen boundary.

If a boundary is crossed, two things happen:

The player's position is teleported to the opposite side of the screen.

The global map_screen_x or map_screen_y is shifted by 16 tiles to reveal the next map area.

```
-- Must be called after the player's position (o.x, o.y) has been updated!
function check_map_transition(o)
    
    -- RIGHTWARD TRANSITION (Player walks past X=120)
    if o.x > 120 then
        map_screen_x += 16   -- Move map column 16 tiles right
        o.x = 0              -- Teleport player to the left edge (X=0)
    end
    
    -- LEFTWARD TRANSITION (Player walks past X=0)
    if o.x < 0 then
        map_screen_x -= 16   -- Move map column 16 tiles left
        o.x = 120            -- Teleport player to the right edge (X=120)
    end
    
    -- DOWNWARD TRANSITION (Player walks past Y=120)
    if o.y > 120 then
        map_screen_y += 16   -- Move map row 16 tiles down
        o.y = 0              -- Teleport player to the top edge (Y=0)
    end
    
    -- UPWARD TRANSITION (Player walks past Y=0)
    if o.y < 0 then
        map_screen_y -= 16   -- Move map row 16 tiles up
        o.y = 120            -- Teleport player to the bottom edge (Y=120)
    end

    -- IMPORTANT: Add a bounds check here to prevent going past the map limits!
    map_screen_x = mid(0, map_screen_x, 112)
    map_screen_y = mid(0, map_screen_y, 48)

end
```

*Key Considerations*

**Boundary Check:** The player hits the boundary when they are near the edge (e.g., o.x > 120). Since the player sprite is 8 pixels wide, 120 is the last safe position before crossing the right boundary.

**Map Limits:** The mid() function prevents map_screen_x from exceeding the limits of PICO-8's map (128 columns total, so the last screen starts at column 112). You must check the map size for your specific project!

## 3. Integration into the Game Loop

You must initialize the map variables in _init() and call the function in _update().

```
In _init():

function _init()
    -- Map Viewport Tracking
    map_screen_x = 0
    map_screen_y = 0
    
    -- (Other player setup...)
end


In _update():

function _update()
    move(player)
    check_map_transition(player) -- Run the check after player movement!
    -- (Other update logic...)
end
```
