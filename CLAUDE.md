# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project

Two-Ink Tic-Tac-Toe — a single-file static web game. Pure HTML/CSS/vanilla JS,
no build system, no dependencies, no tests, no package manager. Deployed to
GitHub Pages at https://zuvuyan.github.io/TutorialTim/.

## Critical: keep the two HTML files in sync

`index.html` and `tictactoe.html` are byte-identical copies. Any change to the
game must land in **both** — easiest is to edit `index.html`, then
`cp index.html tictactoe.html`. `index.html` is what GitHub Pages serves;
`tictactoe.html` is a standalone-named copy referenced in the README file table.

## Run / preview

- Open `index.html` directly in a browser, or serve the root:
  `python -m http.server 8000` → `http://localhost:8000/`.
- Regenerate `docs/screenshot.png` with headless Chrome against a temp copy that
  auto-plays a few moves:
  ```
  "/c/Program Files/Google/Chrome/Application/chrome.exe" --headless --disable-gpu \
    --hide-scrollbars --force-device-scale-factor=2 --window-size=760,900 \
    --virtual-time-budget=2000 --screenshot=docs/screenshot.png <url-or-file>
  ```
  Seed a position by appending a `<script>` that calls `.click()` on
  `#board` children before shooting.

## Deploy

Push to `main` → `.github/workflows/deploy.yml` uploads the repo root to GitHub
Pages (also runs on `workflow_dispatch`). No manual step. `docs/` is served too,
which is harmless.

## Architecture (the IIFE at the end of index.html)

- **`state`** is the single source of truth: `cells[9]`, `turn`, `starter`,
  `over`, `mode` (`pvp`/`cpu`), `diff` (`easy`/`ace`), `locked`, `scores`.
- **`place(i, player)`** is the move pipeline: mutate `cells` → `render()` →
  `winnerInfo()` → finish, or flip `turn` and call `maybeCpu()`.
- **`render()`** is the only DOM-writer for the board, scores, and labels; it
  derives everything from `state` and injects SVG marks lazily.
- **CPU**: `bestMove()` + `minimax()` (unbeatable, `diff === 'ace'`);
  `casualMove()` (win/block heuristic plus randomness, `diff === 'easy'`).
  Runs from `maybeCpu()` behind the `locked` flag and a `setTimeout`.
- **`LINES`** — the 8 winning triples, used by `winnerInfo()` and `casualMove()`.
- **Marks and the winning strike** are runtime-generated inline SVG. Strokes use
  `pathLength="100"` so the CSS `stroke-dashoffset` draw-on animation is
  length-independent. `drawStrike()` maps a cell index to a line on a 300×300
  viewBox overlay above the board.
- **Rounds**: `newRound()` clears the board and alternates `starter`;
  `resetMatch()` also zeroes `scores`. Switching mode or difficulty resets the
  match.

## Visual system (intentional constraints)

- One committed theme — a two-ink riso print on newsprint. **No dark mode**, no
  `data-theme` handling; this is deliberate. All colors are CSS custom
  properties on `:root`.
- `--red` is the X / player one, `--blue` is the O / player two, everywhere.
- Fonts load via a Google Fonts `@import` at the top of `<style>`: Bricolage
  Grotesque (`--font-display`), Newsreader (`--font-body`), DM Mono
  (`--font-mono`, used for UI and the status line).

## Conventions

- ES5-style vanilla JS (`var`, function expressions, no modules or bundler) —
  match it when editing.
- The `LF will be replaced by CRLF` git warning on this Windows checkout is
  expected; ignore it.
