# Two-Ink Tic-Tac-Toe

A playable tic-tac-toe game for the web with a two-color risograph print look —
red draws the X strokes, blue draws the O rings.

## Play

Open `index.html` in any modern browser. No build step, no dependencies.

Live version: https://claude.ai/code/artifact/7f86cf9b-a16f-4676-bc1e-553f3c202f6e

## Features

- **2 Players** (pass-and-play) or **vs Computer**
- Computer difficulty: **Casual** (blocks/wins, otherwise wanders) or **Ace** (minimax — unbeatable)
- Score tracking across rounds; first move alternates each round
- Winning line is struck through
- Keyboard navigable board (arrow keys to move focus, Enter/Space to play)
- Respects `prefers-reduced-motion`

## Files

| File | Purpose |
|------|---------|
| `index.html` | The complete game — HTML, CSS, and JS in one file |
| `tictactoe.html` | Identical copy |
