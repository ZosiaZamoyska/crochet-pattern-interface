# Crochet Pattern Builder (Svelte)

This project is a web-based interface for building crochet patterns visually, using Svelte.

## Step-by-Step Implementation Plan

### 1. Project Setup
- [x] Scaffold a new Svelte project using Vite.
- [x] Install dependencies and verify the dev server runs.

### 2. Basic UI Layout
- [x] Create a menu with stitch options:
    - Chain
    - Magic Ring
    - Single Crochet
    - Half Double Crochet
    - Double Crochet
    - Treble Crochet
- [ ] Add a main pattern canvas area for drawing/building the pattern.

### 3. Stitch Placement & Snapping
- [ ] Implement grid/row/round snapping for stitch placement.
- [ ] Allow batch placement (drag or click to place multiple stitches).
- [ ] Auto-connect new stitches to previous ones.
- [ ] Allow user to select which stitch below to connect to (for increases/decreases).

### 4. Stitch & Pattern Data Model
- [ ] Define a data structure for stitches (type, position, connections, round/row).
- [ ] Store and update the pattern as the user interacts.

### 5. Interactivity & Usability
- [ ] Allow moving, editing, or deleting stitches.
- [ ] Add undo/redo functionality.
- [ ] Make the UI intuitive for crocheters (snapping, batch actions, clear visuals).

### 6. Future Improvements
- [ ] Export pattern as image or text instructions.
- [ ] User accounts and pattern saving.
- [ ] More stitch types and custom symbols.

---

## Getting Started

1. Install dependencies:
   ```bash
   npm install
   ```
2. Run the development server:
   ```bash
   npm run dev
   ```
3. Open your browser at the provided local address.

---

## Project Structure
- `src/` — Svelte components and logic
- `public/` — Static assets
- `README.md` — This file

---

## Contributing
Pull requests and suggestions are welcome!
