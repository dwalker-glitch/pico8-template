```
-- map(tile_x, tile_y, screen_x, screen_y, width_tiles, height_tiles)
map(0, 0, 0, 0, 16, 16)
```

## Map Creation in PICO-8

The PICO-8 map is a giant grid (128 tiles wide by 32 tiles tall) where you place your background tiles. The `map()` function is the only thing that actually draws this grid onto the screen.

### Coordinates and Size

| Feature | Details | 
 | ----- | ----- | 
| **Map Coordinates** | `tile_x` and `tile_y` are measured in **tiles** (8x8 pixels). | 
| **Screen Coordinates** | `screen_x` and `screen_y` are measured in **pixels** (0 to 127). | 
| **Total Map Size** | **128 tiles wide** (0-127) by **32 tiles high** (0-31). | 
| **Screen View** | The screen only displays **16 tiles wide** by **16 tiles high** at any time. | 

---

## Explanation of Parts

Calling the map() function with no parameters will force everything to default to `(0, 0, 0, 0, 16, 16)`

| **Parameter** | **Value** | **Explanation** | 
 | ----- | ----- | ----- | 
| **`tile_x`** | `0` | The **map column** (measured in tiles) where drawing starts. `0` is the very first column of the map. | 
| **`tile_y`** | `0` | The **map row** (measured in tiles) where drawing starts. `0` is the very first row of the map. | 
| **`screen_x`** | `0` | *optional* The **screen X-coordinate** (measured in pixels) where the map starts drawing. `0` is the top-left corner. | 
| **`screen_y`** | `0` | *optional* The **screen Y-coordinate** (measured in pixels) where the map starts drawing. `0` is the top-left corner. | 
| **`width`** | `16` | *optional* The **number of tiles wide** to draw from the map (16 tiles = 1 screen width). | 
| **`height`** | `16` | *optional* The **number of tiles high** to draw from the map (16 tiles = 1 screen height). | 
