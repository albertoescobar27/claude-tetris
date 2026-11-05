# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project

Tetris en JS vanilla + Canvas 2D. Sin build, sin dependencias, sin tests, sin linter. README y UI en español.

## Run

Abrir `index.html` directamente, o servidor estático: `python -m http.server 8000` → `http://localhost:8000`.

## Architecture

- `index.html`: dos canvas (`#board` 300×600, `#next-canvas` 120×120), panel HUD, `#overlay` (pausa/game over, clase `hidden`).
- `game.js`: toda la lógica, un solo script, estado en variables globales de módulo (`board`, `current`, `next`, `score`, ...) declaradas con `let` y asignadas en `init()`.
- `style.css`: tema oscuro.

Puntos clave de `game.js`:
- Piezas: matrices con índice de color 1–7 (`PIECES`/`COLORS` indexados igual; índice 0 = vacío). Rotación vía `rotateCW`; kicks solo horizontales `[0,-1,1,-2,2]` en `tryRotate`.
- Flujo de bloqueo: `lockPiece()` = `merge()` → `clearLines()` → `spawn()`. `spawn()` llama `endGame()` si la pieza nueva colisiona.
- Loop `requestAnimationFrame` (`loop`) acumula `dropAccum` contra `dropInterval` (`max(100, 1000 - (level-1)*90)`, recalculado en `clearLines`). Pausa/game over cancelan `animId`; reanudar llama `loop` manualmente.
- Puntuación: `LINE_SCORES[cleared] * level`; hard drop +2/celda, soft drop +1/fila.
- Si cambias `COLS`/`ROWS`/`BLOCK`, actualizar `width`/`height` de `#board` en `index.html` (el offset de `drawNext` también asume rejilla 4×4 con `NB = 30`).
