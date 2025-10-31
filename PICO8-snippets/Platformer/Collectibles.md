```
function ipickups()
	pu={
	x=63,
	y=63,
	act=true,
	}
	donuts=0
end

function upickups()
	if pu.act then
		if abs(player.x-pu.x)<=4 and abs(player.y-pu.y)<=4 then
			pu.act=false
			donuts+=1
		end
	end
	
end

function dpickups()
	if pu.act then
		spr(30,pu.x,pu.y,2,2)
	end
	print("donuts: "..donuts,10,10,7)
end
```

## Pickup Logic (Init, Update, Draw)

These three functions (`ipickups`, `upickups`, `dpickups`) follow the standard PICO-8 structure (Init, Update, Draw) to manage a game object (the pickup) that is separate from the main map system.

The code uses the **`act`** (active) flag to control whether the donut exists and the **`abs()`** function for quick collection collision.

---

### `ipickups()`: Initialization

This function is called once at the start of the game (usually inside `_init()`) to set up the pickup object (`pu`) and the score counter (`donuts`).

| Line | Code | Explanation |
| :--- | :--- | :--- |
| `2-6` | `pu={ ... act=true, }` | Creates a global table named **`pu`** (pickup). [cite_start]It is positioned at (63, 63) and its **`act`** key is set to `true`, meaning the pickup exists. [cite: 1] |
| `7` | `donuts=0` | [cite_start]Initializes the player's score counter for this item to zero. [cite: 1] |

---

### `upickups()`: Update/Collection Logic

This function is called every frame (inside `_update()`) to check for collection and update the game state.

| Line | Code | Explanation |
| :--- | :--- | :--- |
| `1` | `if pu.act then` | [cite_start]**Condition Check:** Only run the collision logic if the pickup is currently **active** (`true`). [cite: 1] |
| `2` | `if abs(player.x-pu.x)<=4 and ...` | **Proximity Check:** This is a simple collision check using $\text{abs}()$. [cite_start]It checks if the absolute X distance **AND** the absolute Y distance between the `player` and the `pu` is 4 pixels or less. [cite: 1] |
| `3` | `pu.act=false` | [cite_start]**Collection Response:** If the proximity check is `true`, the pickup is collected, so its `act` flag is set to `false`. [cite: 1] |
| `4` | `donuts+=1` | [cite_start]Increments the donut score by 1. [cite: 1] |

---

### 🎨 `dpickups()`: Drawing Logic

This function is called every frame (inside `_draw()`) to render the pickup sprite and the score text.

| Line | Code | Explanation |
| :--- | :--- | :--- |
| `1` | `if pu.act then` | **Drawing Condition:** Only draw the sprite if the pickup is **active** (`true`). [cite_start]Since the flag was set to `false` upon collection, the sprite disappears immediately. [cite: 1] |
| `2` | `spr(30,pu.x,pu.y,2,2)` | Draws the sprite. `spr(30, ...)` draws **Sprite 30** at the pickup's `x` and `y` coordinates. [cite_start]The last two arguments, `2, 2`, mean the sprite is drawn at **$2 \times 2$ scale** (a $16 \times 16$ area). [cite: 1] |
| `4` | `print("donuts: "..donuts,10,10,7)`| [cite_start]Displays the current donut count as a score on the screen at coordinates (10, 10), using color 7 (white). [cite: 1] |
