# function _draw() The Screen

This function is the graphics department. It runs every single frame (just like \_update()), but its only job is to draw things onto the screen. This is the last thing that happens before the player sees the new frame.

## Purpose and Use

| Purpose | What it does | Analogy |
| :--- | :--- | :--- |
| Clear Screen | Clears the previous frame's drawing. You must do this first! | Wiping the screen clean before drawing the next scene. |
| Graphics | Draws the map, player, enemies, and score text. | The camera takes a snapshot of the stage at this exact moment. |
| Order Matters | Items drawn first will be covered up by items drawn later. | Drawing the background *before* the player means the player is visible. |

---

## Example Code

Use functions like cls(), spr(), map(), and print() here.

```
function _draw()
    -- 1. Clear the screen (cls)
    cls() -- Resets the screen so it can be redrawn fresh
    
    -- 2. Draw the background map
    map(0, 0, 0, 0, 16, 16)
    
    -- 3. Draw the player sprite
    spr(1, player.x, player.y, 1, 1, false, false)
    
    -- 4. Use an outside function to draw more complicated sprites
    draw_blasters()
    
    -- 5. Draw the score text on the HUD
    print("score: "..score, 5, 5, 7) -- White text at (5, 5)
end
```
