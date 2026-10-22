# Vue 2048

A browser-based version of the classic 2048 number puzzle, built with Vue.js. Players move numbered tiles on a grid, merge tiles with matching values, and aim to create the 2048 tile.

## Gameplay

- Move tiles in four directions.
- Matching tiles combine into a tile with their summed value.
- A new tile appears after a successful move.
- The score increases when tiles merge.
- The game ends when no valid moves remain.

## How to play

1. Open the game in your browser.
2. Use the supported directional controls to move the board.
3. Combine equal-valued tiles to build larger numbers.
4. Plan moves to keep enough space for future merges.
5. Continue until you reach 2048 or no valid moves remain.

The exact keyboard and touch controls depend on the implementation.

## Built with

- Vue.js
- JavaScript
- HTML
- CSS

The project demonstrates reactive UI updates, component-based frontend development, event handling, and state-driven rendering.

## Game logic overview

A typical 2048 implementation maintains a two-dimensional board and applies a directional transformation when the player moves:

1. Read each row or column in the direction of movement.
2. Remove empty cells from the sequence.
3. Merge adjacent equal values once per move.
4. Restore empty cells to the board.
5. Compare the updated board with the previous board to determine whether the move changed anything.
6. If the board changed, spawn a new tile and update the score.
7. Check whether any valid move remains.

This describes the standard game algorithm; consult the source for the exact implementation details.

## Run locally

The exact package manager and scripts depend on the repository. For a typical Vue project:

```bash
npm install
npm run dev
```

If those scripts are not present, inspect `package.json` for the supported development and build commands. Open the local URL printed by the development server.

## Production build

For a Vue project configured with a standard build script, the command is commonly:

```bash
npm run build
```

Use the script defined by this repository and deploy the generated output directory using your preferred static hosting service.

## Potential improvements

- Responsive layouts for mobile and desktop
- Touch and keyboard controls
- Tile movement and merge animations
- Best-score persistence
- Undo and restart controls
- Accessibility improvements and reduced-motion support
- Unit tests for movement, merge, scoring, and game-over logic

## Project status

This README covers the game's standard mechanics and Vue-based implementation. Update screenshots, controls, and commands to match the actual project.
