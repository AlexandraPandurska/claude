# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Running the games

No build step, server, or dependencies. Open any `.html` file directly in a browser:

```
start tictactoe.html
start shooter.html
```

There are no lint, test, or compile commands — all logic is vanilla JS embedded in each HTML file.

## Repository structure

Each game is a **single self-contained HTML file** with CSS and JavaScript inlined. No shared utilities, no external assets, no modules.

| File | Tech | Description |
|---|---|---|
| `tictactoe.html` | DOM + vanilla JS | Tic Tac Toe with 2-player and vs-CPU modes |
| `shooter.html` | HTML5 Canvas + vanilla JS | Flower Shooter arcade game |

## Architecture patterns

### tictactoe.html
- State lives in plain variables (`board`, `current`, `over`, `mode`, `difficulty`).
- The minimax AI (`minimax()`) mutates the board array in-place and restores it — no copies made.
- `easyMove()` plays optimally 50% of the time, randomly otherwise.
- Switching mode or difficulty resets scores and calls `init()`.

### shooter.html
- Driven by `requestAnimationFrame` running unconditionally; `update()` early-returns based on `state`.
- Game state machine: `START → PLAYING → LEVELUP → PLAYING → GAMEOVER`.
- All game objects (`flowers`, `hearts`, `particles`) are plain arrays of plain objects; iterated backward when splicing.
- Mouse and keyboard controls coexist: mouse takes over for 100 ms after any `mousemove` event, then keyboard resumes.
- `heartPath()` is a shared Canvas path used by both enemy hearts and the HUD life icons.
- Difficulty scales per level: heart speed = `70 + level * 12` px/s; spawn interval = `max(500, 1200 - (level-1) * 100)` ms; 2 HP hearts from level 4; horizontal wobble from level 5.

## Commit convention

```
Commit #[N]: [short description for peer reviewer]
```

Commits are numbered sequentially starting from 1. The current latest is **Commit #2**.

## Branch and PR workflow

- Feature branches follow the pattern `feature/[kebab-case-name]`.
- Remote: `https://github.com/AlexandraPandurska/claude.git`
- PR URL pattern: `https://github.com/AlexandraPandurska/claude/pull/new/<branch-name>`
