### Player Table (Platformer Base)

This snippet defines the core data required for the player: its position, size, and movement state. Students should copy this code into the top of their project's Lua file.

##Code Snippet

```-- PLAYER DATA
player = {
    x = 64,
    y = 64,
    w = 8,
    h = 16,
    dx = 0,
    dy = 0,
    name = 1,
    is_grounded = false,
    facing_right = true
}```


## Explanation of Parts

This structure uses a Lua table named player to hold all the unique information about our main character in one place.

| **Key** | **Meaning** | **Explanation** |
| :--- | :--- | :--- |
| **`x`**, **`y`** | Position | The player's current screen coordinates. PICO-8 is 128x128 pixels. |
| **`w`**, **`h`** | Width, Height | The size of the player's sprite and **collision box** (default sprite size is 8x16). |
| **`dx`** | Delta X (Velocity X) | How fast the player is moving **horizontally** (left/right). **`0`** means they are still. |
