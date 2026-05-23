# Pratt's Place — Design Spec
_Date: 2026-05-23_

## Overview
A second page for Ridge's brother Pratt, living in the same GitHub repo (`ridges_world`). Accessible as `pratt.html`. Linked to/from `index.html`.

## Visual Style
- **Background:** `#0a180a` (dark forest green-black)
- **Accent:** `#2d7a2d` (forest green)
- **Gold:** `#c8a400` (Zelda gold)
- **Text:** `#e8f5e8` (pale green-white)
- **Fonts:** Orbitron (headings), Rajdhani (body) — same as Ridge's page
- Faint paw print watermark in header (echoes Ridge's sheep)

## Header
- Title: **PRATT'S PLACE**
- Subtitle: *"Cat Whisperer. Zelda Master. Legend in Progress."*
- Dark forest green gradient background

## Interest Cards
Same flip-on-click mechanic as Ridge's page (0.8s spin, shows photo back, auto-flips back).
- 🐱 Cats
- 🗡️ Zelda
- Two placeholder slots for future interests/photos

## Mini Game: "TRIFORCE DUNGEON"
### Canvas
- Large canvas (~720×480px)
- Top-down dungeon with hand-crafted stone wall maze (larger and more complex than Bell Drop)

### Player
- Link drawn procedurally: green tunic, pointy hat, small sword
- Arrow key movement (4 directions, tile-based or smooth)
- 3 hearts / lives

### Enemies
1. **Keese x2** — bat enemies, patrol random paths, cost 1 heart on contact, brief invincibility after hit
2. **Bokoblin x1** — larger enemy, patrols a set route, charges toward Link if within 3 tiles, costs 1 heart on contact

### Win Condition
- Triforce (gold triangle) placed at far end of maze
- Reaching it triggers win screen: "YOU FOUND THE TRIFORCE" with gold flash

### Lose Condition
- 0 hearts → "GAME OVER — GANON WINS" screen
- Restart button on both win/lose screens

### HUD
- Heart display (top left)
- Score/steps counter (optional, top right)

## Navigation
- `pratt.html`: small "← RIDGE'S WORLD" link in header
- `index.html`: small "PRATT'S PLACE →" link added to header

## File Structure
- `pratt.html` — new file, self-contained (inline CSS + JS like index.html)
- No new image files required (game is procedurally drawn)
- Interest card images added later when photos are available
