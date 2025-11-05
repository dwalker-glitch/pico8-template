# The PICO-8 Core Game Loop

Every PICO-8 game runs on a loop of three special functions. The computer uses these three functions to manage all game logic, input, and graphics.

Think of the loop as a constant, rapid film production cycle:

| Function | When It Runs | Role in the Cycle |
| :--- | :--- | :--- |
| **`\_init()`** | **ONLY ONCE** when the game starts. | This is the **Setup**. It defines all starting variables, creates the player object, and sets up empty tables (like for enemies or blasters). |
| **`\_update()`** | **60 times per second.** | This is the **Brain**. It processes all game logic, including checking button presses, moving objects, and handling collisions. Nothing here is visible. |
| **`\_draw()`** | **60 times per second.** | This is the **Screen**. It runs after `\_update()` to clear the screen, draw the map, and render the updated positions of the player and all objects for the player to see. |

---

## The Loop

The game automatically repeats \_update() and \_draw() over and over, sixty times every second, until the game is stopped!

For detailed explanations, please see the individual files: init_func.md, update_func.md, and draw_func.md.
