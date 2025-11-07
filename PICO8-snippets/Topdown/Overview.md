# Role Playing Games (RPG)

The RPG category focuses heavily on player progression, exploration, and narrative.

An RPG's main goal is to let the player take on a role, usually starting weak and growing powerful through progression.

---

### 1. Player Setup

**Initialization:** The player object is initialized with crucial values before the game begins (in _init), establishing the character's starting state.

**Stats and State:** The player is defined by internal variables (HP, speed, inventory) rather than just their graphics.

**Determining Capability:** These variables determine everything the player can do, such as how fast they move, how much damage they can withstand, and what tools they can carry.

### 2. Level Design

**Static Maps:** Instead of a long, continuously scrolling world (like a platformer), an RPG world is typically divided into a large grid of fixed screens (or "rooms").

**Encouraging Exploration:** The entire map often starts shrouded in mystery. The game design encourages the player to seek out exits, secrets, and unusual areas because that is where new items, keys, and progress are found.

**Map Transitions:** When the player reaches a screen edge or a doorway, the screen instantly switches, and a new map is loaded. This is a very efficient way to build a massive world with limited computer resources.

### 3. Progression

**Player Progression:** The player's strength is defined by the tools they carry and their stats. Finding a new sword, a shield, or a key that unlocks a new area is the main reward, rather than just earning points.

**Resource Management:** The player must manage core resources like Health Points (HP), which track damage, and Inventory, which holds items like bombs, arrows, or keys. Managing these resources is a puzzle in itself.

### 4. Movement

**Simple & Deliberate:** Movement is designed for careful exploration, not complex platforming challenges.

**Direct Control:** When an arrow key is pressed, the character moves at a fixed speed instantly. When the key is released, movement stops immediately.

**No Physics:** There is no complex physics like gravity, momentum, or friction to complicate precise movement around traps and enemies.

### 5. Combat

**Controlled Exchange:** Combat is a controlled exchange of damage, revolving around managing your own health and dealing with enemy actions.

**Timers and Cooldowns:** Key to combat balance, a timer prevents the player from attacking again immediately after swinging a sword, forcing strategic action.

**Invulnerability (I-frames):** If the player gets hit, a brief invulnerability timer protects them from being instantly overwhelmed by continuous damage.

### 6. Dialogue

**Narrative Focus:** Dialogue is the primary mechanism for delivering story, hints, and crucial information from Non-Player Characters (NPCs).

**Game State Pause:** When dialogue begins, the entire game, including player movement and enemy actions, must pause to ensure the player can focus.

**Text and Input:** The dialogue system displays text boxes and waits for the player's input (a button press) to advance the conversation or close the window.
