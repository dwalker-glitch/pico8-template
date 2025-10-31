```
function move(o)
	local lx=o.x
	local ly=o.y
	
	if (btn(2)) o.y-=1 -- up arrow
	if (btn(3)) o.y+=1 -- down arrow
	if (btn(1)) o.x+=1 -- right arrow
	if (btn(0)) o.x-=1 -- left arrow
	
	-- if it collides, move back
	if collide(o) then
		o.x=lx
		o.y=ly
	end
end

function collide(o)
	local x1=o.x/8
	local y1=o.y/8
	local x2=(o.x+7)/8
	local y2=(o.y+7)/8
	
	local a=fget(mget(x1,y1),0)
	local b=fget(mget(x1,y2),0)
	local c=fget(mget(x2,y1),0)
	local d=fget(mget(x2,y2),0)
	
	if a or b or c or d then
		return true
	else
		return false
	end
end
```

# Movement and Simple Collision (Move and Check)

This method of collision is called "Move and Check." You move the player, and then immediately check if they landed inside a solid tile. If they did, you revert them to their previous position.

## The move(o) Function: The Controller

The move() function handles the player's inputs and uses the collide() function to decide whether to allow the movement. It requires the player's table (o) to be passed to it.

| Line | Code | Explanation |
| :--- | :--- | :--- |
| `1-2` | `local lx=o.x` <br> `local ly=o.y` | Saves the **last known safe position**. If movement causes a collision, the player will be moved back to `(lx, ly)`. |
| `4-9` | `if (btn(..)) o.y+=1` | Checks if any arrow buttons are pressed. If so, it updates the player's position (`o.x`, `o.y`) by 1 pixel. **This is the speculative move.** |
| `12` | `if collide(o) then` | Calls the `collide` function to check the player's **new position**. If `collide` returns `true`, we have hit a solid tile. |
| `13-14` | `o.x=lx` <br> `o.y=ly` | **Collision Response:** Moves the player back to the last safe position before the movement was applied. |

## The collide(o) Function: The Detector

The collide() function uses the PICO-8 map tools (mget and fget) to check the new position. It assumes the object (o) is an 8×8 sprite.

Four-Corner Check

Since collision is based on 8×8 tiles, you must check all corners of your object's 8×8 area to make sure no part of it is inside a wall.

| Variable | Calculation | What it Checks |
| :--- | :--- | :--- |
| `x1, y1` | `o.x/8`, `o.y/8` | **Top-Left** Corner |
| `x1, y2` | `o.x/8`, `(o.y+7)/8` | **Bottom-Left** Corner |
| `x2, y1` | `(o.x+7)/8`, `o.y/8` | **Top-Right** Corner |
| `x2, y2` | `(o.x+7)/8`, `(o.y+7)/8` | **Bottom-Right** Corner |

Why +7? Since an 8×8 sprite goes from pixel 0 to 7, you add 7 to find the coordinate of the far right/bottom edge. Dividing that by 8 gives you the tile number.

| Line | Code | Explanation |
| :--- | :--- | :--- |
| `18-21` | `local a = fget(mget(..), 0)` | For each corner, the code calls `mget` to get the tile ID, then calls `fget(..., 0)` to check if **Flag 0** (the solid flag) is set on that tile. The result is stored as a boolean (`a`, `b`, `c`, `d`). |
| `23` | `if a or b or c or d then` | This is the final decision. If the Top-Left **OR** the Bottom-Left **OR** the Top-Right **OR** the Bottom-Right is touching a solid tile, the function returns `true`. |
| `24-26` | `return true` / `return false` | Sends the result back to the `move()` function. |
