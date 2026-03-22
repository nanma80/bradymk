# FlatCraft — 2D Minecraft Clone

A browser-based 2D Minecraft clone built as a single HTML file using HTML5 Canvas and vanilla JavaScript.

**Play:** Open `index.html` in any browser.

---

## Current State (v0.3)

### Core Systems

**World Generation**
- 512x200 tile world with procedural terrain using value noise (3 octaves)
- Surface biomes: grass plains, sand beaches near water level
- Underground: stone with cave systems (noise-based carving)
- Ore distribution at varying depths: coal, iron, gold, redstone, lapis, emerald, diamond
- Trees on grass with wood trunks + leaf canopy
- Gravel patches underground, clay near water
- Worldgen water fills low-elevation areas as lakes/oceans
- Bedrock floor (unbreakable)
- Random seed per world, displayed in HUD

**Block Types (65 total)**
- Natural: Dirt, Grass, Stone, Sand, Gravel, Clay, Snow, Ice, Water, Lava
- Ores: Coal, Iron, Gold, Diamond, Emerald, Redstone, Lapis
- Wood/Plants: Wood, Leaves, Planks
- Building: Cobblestone, Brick, Glass, Sandstone, Obsidian, Mossy Cobblestone
- Functional: Crafting Table, Furnace, Chest, Torch, Bookshelf, TNT, Ladder
- Redstone: Redstone Wire, Lever, Button, Redstone Torch, Door (top/bottom), Piston, Piston Head, Repeater, Pressure Plate, Sticky Piston, Observer
- Decorative: 7 wool colors, Iron/Gold/Diamond blocks, Glowstone
- Food/Farm: Melon, Pumpkin, Hay Bale, Sponge
- Nether: Netherrack, Soul Sand, Nether Brick
- Special: Bedrock (unbreakable), Water, Lava

**Item Types (24 total)**
- Resources: Stick, Coal, Iron Ingot, Gold Ingot, Diamond, Emerald, Redstone Dust, Lapis
- Tools (4 tiers each — wood/stone/iron/diamond): Pickaxe, Axe, Shovel, Sword

**Player**
- 20x44px character with head, body, arms, legs rendering
- Held item shown in hand (both tools AND blocks now render)
- Gravity, jumping, swimming, **ladder climbing**
- Ground collision detection

### Game Modes

**Creative Mode** (default on start, toggle with `C`)
- Free flight (WASD + Space/Shift), toggle flight with `F`
- Instant block breaking
- Infinite blocks — all blocks + items available
- Extended reach (50 blocks)
- Scroll through pages of items with `E`

**Survival Mode** (toggle with `C`)
- Normal physics: gravity, jumping, swimming
- Tool-based mining with speed multipliers and tier requirements
- Inventory management (9 hotbar slots, stack to 64)
- Crafting system with 30+ recipes (hand crafting + crafting table)
- Start with empty inventory — punch trees to begin

### Mining & Tools

**Tool Speed System**
- Hand: 1x speed on everything
- Wood tools: 2x, Stone: 4x, Iron: 6x, Diamond: 8x
- Tools only speed up their matching block category:
  - Pickaxe: stone, ores, brick, obsidian, furnace, etc.
  - Axe: wood, planks, crafting table, chest, bookshelf, ladder
  - Shovel: dirt, grass, sand, gravel, clay, snow, soul sand
  - Sword: 2x on leaves

**Tier Requirements (pickaxe)**
- Wood+: Stone, Coal, Sandstone
- Stone+: Iron Ore, Lapis
- Iron+: Gold, Diamond, Emerald, Redstone
- Diamond: Obsidian
- Mining without required tier: block breaks but drops nothing

**Block Drops**
- Most blocks drop themselves
- Stone -> Cobblestone, Grass -> Dirt
- Ores drop items: Coal Ore -> Coal, Iron Ore -> Iron Ingot, etc.
- Glass drops nothing

### Crafting

**Two-tier crafting system (MC-style):**
- **Hand crafting** (`E` or `Tab`): Planks, Sticks, Crafting Table only
- **Crafting Table** (right-click a placed crafting table): All recipes — tools, weapons, blocks, etc.

UI shows "Hand Craft" or "Crafting Table" title, recipe list with green/red ingredient counts. Click to craft. Scroll for more. Hand craft UI hints to use a crafting table for more recipes.

Recipes include:
- **Basics (hand):** Wood -> 4 Planks, 2 Planks -> 4 Sticks, 4 Planks -> Crafting Table
- **Tools (table):** All 4 tiers of Pickaxe, Axe, Shovel, Sword (material + sticks)
- **Blocks (table):** Furnace (8 cobble), Chest (8 planks), Torches (coal + stick), Sandstone (4 sand), Brick (4 clay + coal), Glass (4 sand + coal), Bookshelf (6 planks), Ladder (7 sticks -> 3)
- **Mineral Blocks (table):** 9 ingots/gems -> block, block -> 9 ingots/gems
- **Other (table):** White Wool (4 leaves)

**Recipe differences from MC:**
- Axes use 2 material + 2 sticks (differentiated from pickaxe's 3 + 2)

### Physics

**Sand & Gravel Gravity**
- Fall when air or water is below them
- Processed every 3 frames for performance
- Dig under a sand column and it collapses

**Liquid Flow (MC-style)**
- All water/lava (including worldgen) flows DOWN into air below — fills caves under oceans
- Source block stays in place (oceans don't drain)
- Player-placed liquids also spread sideways: water 7 blocks, lava 3 blocks from source
- Sideways spread is gradual (1 block per tick), hard-capped by distance from origin
- Water + Lava interaction: water touching lava -> cobblestone, lava touching water -> obsidian

**Swimming & Climbing**
- Swimming: W/Space to swim up (-4 velocity), S to swim down, low gravity (0.1)
- Water detection checks both player center AND feet — player can swim out of water onto land
- Ladder climbing: same physics as swimming — W/Space up, S down, low gravity
- Ladder detection checks center AND feet — player can climb off the top of a ladder

**TNT**
- Left-click: mines normally (drops as block)
- Right-click with torch: ignites (like flint & steel)
- Lava adjacent to TNT: auto-ignites
- Explosion chain reaction: TNT caught in blast detonates after 150ms delay
- Blast radius: 4 blocks. Destroys everything except obsidian, bedrock, water
- Fireball animation with expanding ring + white flash

### Visual

- Day/night cycle (24000 tick cycle): day, sunset, night with stars, sunrise
- **Stars parallax scroll** with camera movement at night
- Night overlay darkens the world
- Block-specific pixel art details for all 53 block types
- **Ladder block**: two rails with wood grain highlights + rungs with light edges
- Mining crack animation (progressive overlay)
- Block highlight on hover (within reach)
- Explosion fireball animation
- Torch has transparent background with flame

**Hotbar item icons:**
- **Pickaxe**: T-shape (handle connects into horizontal head with down-curved tips)
- **Axe**: filled r-shape (handle + solid head extending right)
- **Shovel**: vertical handle + flat rectangular head
- **Sword**: vertical blade + crossguard + handle + edge highlight
- **Torch**: brown stick with orange/yellow flame (custom mini-canvas icon)
- **Ladder**: two rails with horizontal rungs (custom mini-canvas icon)
- **Coal**: irregular chunky lumps (brightened for visibility on dark background)
- **Ingots**: trapezoid shape with highlight strip
- **Gems**: diamond shape
- **Redstone**: scattered dot particles
- All item canvases have subtle light background for visibility

### Controls

| Key | Action |
|-----|--------|
| WASD / Arrows | Move |
| Space / Up Arrow | Jump (survival) / Fly up (creative) |
| Shift | Fly down (creative) |
| Left Click | Mine block |
| Right Click | Place block / Ignite TNT with torch / Open crafting table / Toggle lever / Press button / Toggle door |
| 1-9 | Select hotbar slot |
| Shift+, / Shift+. | Scroll hotbar |
| Scroll Wheel | Scroll hotbar (or crafting list) |
| E | Creative: next item page / Survival: hand crafting |
| Tab | Toggle hand crafting (survival) |
| C | Toggle Creative/Survival |
| F | Toggle flying (creative) |
| Escape | Close crafting |

### HUD & UI
- Top-left info panel: "FlatCraft" branding, mode, seed, controls, held item name, pointed-at block, time of day
- Top-right **?** button: reopens the tutorial slideshow at any time
- Bottom hotbar: 9 slots with block/item previews, names, counts (infinity symbol in creative)

### Redstone System

**Power Sources:**
- Lever (toggle on/off, power 15), Button (momentary ~20 ticks, power 15)
- Redstone Torch (always on, inverts when block below is powered, 4-tick anti-oscillation cooldown)
- Pressure Plate (power 15 when player stands on it)
- Observer (emits pulse from back when block in front changes)

**Signal Transmission:**
- Redstone Wire: carries signal, decays by 1 per block (max range 15)
- Repeater: boosts signal back to 15
- Torch towers: torch directly powers block above it for vertical signal

**Powered Devices:**
- Door: opens when powered (auto-closes when power drops), or toggle with right-click. 2-tall block.
- Piston: pushes block in front when powered. Faces away from player on placement (4 directions).
- Sticky Piston: same as piston but pulls block back when retracting.
- Observer: detects block changes in facing direction, emits 4-tick pulse from back.

**Overlay System:**
- Levers and buttons can be placed in front of solid blocks (overlay layer) OR in air (standalone)
- `worldOverlay` Uint8Array stores overlay block IDs
- Right-click to toggle lever / press button (works for both overlay and standalone)

**Power Propagation:**
- 2-pass BFS system running every 2 frames
- Pass 1: Non-torch sources (levers, buttons, plates, observers) → BFS through wire/repeaters → power adjacent blocks
- Pass 2: Torches check `prevWorldPower` for inversion → BFS from active torches
- Torches directly power block above (enables torch towers)

**Placement:**
- Torches/redstone torches: right-click solid block → placed in air above
- Levers/buttons: right-click solid block → overlay, or right-click air → standalone
- Pistons/sticky pistons/observers: face away from player (uses `worldFacing` array)

**Crafting Recipes (all table-required):**
- Lever: 1 Stick + 1 Cobblestone
- Button: 1 Stone
- Redstone Torch: 1 Redstone Dust + 1 Stick
- Redstone Wire: 1 Redstone Dust
- Door: 6 Planks
- Piston: 4 Cobblestone + 1 Redstone Dust + 3 Planks
- Sticky Piston: 1 Piston + 1 Leaves
- Repeater: 3 Stone + 2 Redstone Dust
- Pressure Plate: 2 Stone
- Observer: 6 Cobblestone + 2 Redstone Dust

### Tutorial Slideshow
- 7-slide instruction sequence shown before game starts
- Covers: movement, mining/building, crafting (hand vs table), modes (creative/survival), extra tips, redstone
- Advance with Space, Enter, right arrow, or click
- Can be reopened anytime via the **?** button in top-right corner

---

## Architecture

Single-file HTML (`index.html`, ~2300 lines). All logic in one `<script>` tag.

**Key data structures:**
- `world`: `Uint8Array(WORLD_W * WORLD_H)` — flat array of block IDs (0-64)
- `worldOverlay`: `Uint8Array` — overlay layer for levers/buttons on solid blocks
- `worldOverlayState`: `Uint8Array` — on/off state for overlay levers/buttons
- `worldPower`: `Uint8Array` — redstone power levels 0-15
- `prevWorldPower`: `Uint8Array` — previous frame's power (for torch inversion)
- `worldFacing`: `Uint8Array` — facing direction for pistons/observers (0=right,1=down,2=left,3=up)
- `inventory`: Array of 9 `{id, count}` objects
- `liquidSources`: Array of source objects with origin, frontier BFS, placed set
- `explosions`: Array of active explosion animations
- `doorOpen`: Set of encoded positions for open doors
- `buttonTimers`: Map of encoded position -> frames remaining
- `torchCooldown`: Map of encoded position -> cooldown frames (anti-oscillation)
- `observerState`: Map of encoded position -> `{watching, pulse}`
- `TOOL_INFO`: Map of item ID -> `{type, tier, speed}`
- `RECIPES`: Array of `{result, count, ingredients, name, table}`
- `craftingAtTable`: boolean — whether crafting UI was opened via a crafting table

**Game loop:** `requestAnimationFrame` calls `update()` -> `render()` -> `updateUI()` each frame. Game loop only starts after tutorial slideshow is dismissed.

**Performance notes:**
- Only renders blocks visible on screen (camera culling)
- Falling blocks processed every 3 frames
- Liquid flow every 6 frames
- TNT lava check every 10 frames
- Redstone power propagation every 2 frames, camera-region only

---

## Development History

1. **Initial build** — Procedural world, player movement, block mining/placing, hotbar, day/night cycle, caves, ores, trees, water, swimming
2. **Creative mode** — Flight, instant break, infinite blocks, extended reach, item pages (E key)
3. **Creative collision fix** — Flying still collides with blocks (no phasing through)
4. **Tree density fix** — Lowered spawn threshold so trees are common
5. **Block expansion** — Added 38 new blocks: crafting table, furnace, chest, brick, glass, obsidian, TNT, bookshelf, torch, lava, snow, ice, clay, gravel, wool (7 colors), mineral blocks, nether blocks, melon, pumpkin, hay, sponge, glowstone, sandstone. Plus new ores (gold, emerald, redstone, lapis)
6. **Crafting system** — 30+ recipes, ingredient checking, UI with scroll, click to craft
7. **Tool & mining system** — 4 tool types x 4 tiers, speed multipliers, block categories, tier requirements for ore harvesting, ore items (ingots/gems)
8. **Sand/gravel gravity** — Falling physics for sand and gravel
9. **TNT rework** — MC-style activation: torch right-click, lava proximity, explosion chains. Removed left-click-to-explode
10. **Liquid flow v1** — Water/lava spreading. Had world-flooding bug
11. **Liquid flow v2** — Source tracking, limited sideways spread. Had tsunami cascading bug
12. **Liquid flow v3 (current)** — Per-source BFS with origin-relative distance cap, gradual frontier expansion, worldgen water static except downward fill
13. **Liquid gravity** — All water/lava flows down into air (MC-style: fills caves, source stays)
14. **Hotbar controls** — Added Shift+, / Shift+. for slot scrolling

### v0.2 Session (2026-03-15)

15. **Ladder block** — New block type (B.LADDER = 52). Climbable (no collision, W/S to climb up/down). Crafted from 7 sticks -> 3 ladders (requires crafting table). Custom pixel art: rails with wood grain + rungs with light edges. Axe breaks it faster. Custom hotbar icon.
16. **Survival water/block placement fix** — Can now place blocks on water in survival mode (was creative-only)
17. **Held block rendering** — Blocks held in hand now show as small colored squares on the player character (previously only items/tools rendered)
18. **Axe recipe differentiation** — Axes now use 2 material + 2 sticks (pickaxes use 3 + 2) to avoid identical recipes
19. **Swimming/climbing physics overhaul** — Both `inWater()` and `onLadder()` now check player center AND feet. Stronger upward speed (-4) for both swimming and climbing. Players can now reliably exit water and climb off the top of ladders.
20. **Item icon overhaul** — All hotbar item icons redrawn:
    - Pickaxe: T-shape with connected handle
    - Axe: filled r-shape
    - Shovel: flat rectangular head
    - Sword: blade + crossguard + handle with edge highlight
    - Coal: chunky lumps (brightened from #333 to #555)
    - Ingots: trapezoid shape with highlight
    - Redstone: scattered dots
    - Torch: custom flame icon (was invisible due to null BLOCK_COLOR)
    - Ladder: custom rails+rungs icon
21. **Stars parallax** — Night stars now scroll with subtle parallax relative to camera
22. **Crafting table requirement** — Recipes now have `table: true/false` flag. Only Planks, Sticks, and Crafting Table can be hand-crafted (E/Tab). All other recipes require right-clicking a placed Crafting Table. UI shows "Hand Craft" or "Crafting Table" title, and hand craft hints to use a table for more.
23. **Tutorial slideshow** — 6-slide instruction sequence before game starts. Covers movement, mining/building, crafting (hand vs table), modes, extra tips. Advance with Space/Enter/click. Reopenable via **?** button in top-right corner.
24. **HUD branding** — Info panel now says "FlatCraft" instead of "2D Minecraft"

### v0.3 Session (2026-03-21)

25. **Tab title & favicon** — Tab title changed to "FlatCraft" (no version). Dynamically generated grass block favicon.
26. **Redstone system** — Full redstone implementation with 12 new block types (IDs 53-64):
    - Redstone Wire (signal decay), Lever (toggle), Button (momentary), Redstone Torch (always-on, inverts)
    - Door (2-tall, redstone-powered or right-click), Piston (pushes blocks), Piston Head
    - Repeater (boosts signal), Pressure Plate (player-activated)
    - Sticky Piston (pulls blocks on retract), Observer (detects block changes)
27. **Overlay system** — `worldOverlay` array for levers/buttons placed on front of solid blocks. Separate from world array.
28. **Power propagation** — 2-pass BFS: pass 1 for non-torch sources, pass 2 for torch inversion using `prevWorldPower`. Runs every 2 frames.
29. **Piston facing** — `worldFacing` array (0=right,1=down,2=left,3=up). Pistons/sticky pistons/observers face away from player on placement.
30. **Lever/button in air** — Levers and buttons can be placed as standalone blocks in air, not just as overlays on solid blocks.
31. **Torch tower fix** — Redstone torch directly powers block above it, enabling vertical signal towers.
32. **Torch anti-oscillation** — 4-tick cooldown prevents redstone torch spam when toggling.
33. **Torch placement on blocks** — Right-clicking a solid block while holding a torch/redstone torch places it in the air above.
34. **Door auto-close** — Doors now close when redstone power drops (was only opening before).
35. **Tutorial update** — Added 7th slide covering redstone mechanics, sticky pistons, and observers.
36. **Crafting recipes** — 10 new table recipes for all redstone components.

---

## Known Issues / Future Ideas

**Known Issues:**
- Survival inventory is lost when toggling to creative and back (resets to empty)
- No way to drop items from inventory
- Liquid sideways flow only works for player-placed sources, not worldgen water exposed by mining

**Potential Next Features:**
- Health/hunger system and damage (fall damage, lava damage, drowning)
- Mobs (zombies, skeletons, creepers at night, animals during day)
- Actual furnace smelting UI (currently ores drop ingots directly)
- Larger inventory / backpack beyond 9 slots
- Sound effects
- Save/load world (localStorage)
- Biomes (snow, desert, forest variation)
- Better water: flowing level visuals (half-blocks at edges like MC)
- Flint & Steel item for TNT ignition
- Bed for skipping night
- Hopper / item transport
- Comparator / more advanced redstone logic
