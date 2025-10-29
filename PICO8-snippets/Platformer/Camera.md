# the CAMERA Function

```
-- camera(x, y)
camera(0, 0)
```

## 📸 Camera Control in PICO-8

The `camera()` function controls the **viewport**—the specific area of your game world currently visible on the 128x128 pixel screen. By default, the camera is always at `(0, 0)`, which means the top-left corner of the screen is drawing the top-left corner of the entire game world.

---

## 🌎 How it Changes the World

The camera doesn't move objects; instead, it **shifts the origin point** for all drawing operations.

When you call `camera(x, y)`:
* The point in your game world that corresponds to **$(x, y)$** will now be drawn at the **top-left corner of the screen $(0, 0)$**.
* All subsequent drawing functions like `circ()`, `rect()`, `spr()`, and `map()` will have their coordinates shifted by the camera's position.

### Coordinate Example

| Function Used | No Camera (`camera(0, 0)`) | Camera On (`camera(10, 20)`) |
| :--- | :--- | :--- |
| `circ(50, 50, 5, 7)` | Draws a circle at game world position **$(50, 50)$**. | Draws a circle at game world position **$(60, 70)$**. |
| `map(0, 0, 0, 0, 16, 16)` | Draws the map starting at game world tile **$(0, 0)$**. | Draws the map starting at game world tile **$(10, 20)$**. |

---

### Explanation of Parts

Calling the `camera()` function with no parameters will force everything to default to `(0, 0)`.

| **Parameter** | **Value** | **Explanation** |
| :--- | :--- | :--- |
| **`x`** | `0` | The **X-coordinate** (measured in **pixels**) in the game world that you want to appear at the **left edge** of the screen. |
| **`y`** | `0` | The **Y-coordinate** (measured in **pixels**) in the game world that you want to appear at the **top edge** of the screen. |
| **No Parameters** | `camera()` | Resets the camera back to the default position, making the top-left of the screen correspond to game world **$(0, 0)$**. |

---

## 🏃 Making the Camera Follow the Player

To make the camera track the player, you need to calculate a new position for the camera in every frame of the game (inside the `_update()` function).

The goal is to position the camera so the player remains **centered** on the screen. Since the PICO-8 screen is 128 pixels wide and 128 pixels tall, the center is at $\mathbf{(64, 64)}$.

### The Follow Formula

```
-- The required camera position (cam_x) is:
-- Player's X position (player_x) MINUS half the screen width (64)
cam_x = player_x - 64

-- The required camera position (cam_y) is:
-- Player's Y position (player_y) MINUS half the screen height (64)
cam_y = player_y - 64
```
