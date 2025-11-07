# Top-Down Movement Basics

For adventure or RPG-style games, movement is usually direct and instant. When a button is pressed, the player moves by a fixed speed, and when the button is released, movement stops. No gravity or friction is needed.

## The move(o) Function (Input Only)

This basic function applies player input directly to the x and y positions. At this stage, the player can move right through walls! In order to prevent that, check the Collision.md file.

```
function move(o)
    -- Apply horizontal movement (Left/Right)
    if (btn(1)) o.x += o.speed -- Right
    if (btn(0)) o.x -= o.speed -- Left
    
    -- Apply vertical movement (Up/Down)
    if (btn(3)) o.y += o.speed -- Down
    if (btn(2)) o.y -= o.speed -- Up
    
    -- Note: Collision checks will be added in the next step!
end
```
