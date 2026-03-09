# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project
Single-file HTML5 canvas game. **All code lives in `zombie-space-shooter.html`** — HTML, CSS, JavaScript inline. No build process. No external dependencies. Author: Louis.

**Live:** https://buysacoke.github.io/zombie-space-shooter/zombie-space-shooter.html

## Core Rules
- NEVER rewrite the file from scratch — always make surgical edits to `zombie-space-shooter.html`
- NEVER add external dependencies or CDN links
- ALWAYS use `/plan` mode before making changes
- Audio: Web Audio API only — no external audio files
- Rendering: HTML5 Canvas API only
- Input movement: use the persistent `keys = {}` state object read each frame in `updateShip()` — never handle movement in `keydown` events
- Prune expired particles, lasers, and animation objects from arrays every frame
- Test both keyboard (MacBook) and touch (iPad) paths after every change

## Architecture

### Game Loop
Single `requestAnimationFrame` loop → `update(dt)` → `draw()`. `dt` capped at 50ms.

### State Machine
`gameState` string controls all update/draw branching. Values: `'title'`, `'waveIntro'`, `'playing'`, `'paused'`, `'bossEntry'`, `'bossFight'`, `'bossDying'`, `'zombieLaugh'`, `'dying'`, `'gameOver'`, `'highscores'`, `'nameEntry'`.

### Input
- `keydown`/`keyup` populate `keys = {}`. Escape guard uses `!e.repeat` to prevent rapid-toggle. Movement reads `keys` each frame in `updateShip()`.
- Touch: virtual joystick (bottom-left) + FIRE button (bottom-right). Double-tap outside controls = pause.

### Key Constants (~line 550)
`ZOMBIE_R`, `BOSS_SPEED_SCALE`, `WAVE_INTRO_DUR`, `LIVES`, `BOSS_LASER_SPEED`, `RESPAWN_IMMUNE_TIME`, `MAX_PARTICLES` (200 — caps boss charge-up particles).

### Array Pruning (every frame)
`lasers` filtered in `updateLasers`; `bossLasers` spliced in `updateBossLasers`; `particles` spliced in `updateParticles`.

## Gameplay Rules (don't accidentally break these)
- Wave 1 = 3 zombies; +2 per wave; every 6th wave = boss battle
- After each boss defeated: ship speed +10%, zombie speed +20% (permanent for the session)
- Respawn timer: base 10s + 1s per 2 extra zombies; does NOT scale with level
- Lives: 3 starting; respawn gives 3s flashing immunity (zombies pass through)
- Leaderboard: session-only top 5; new #1 triggers flashing `🏆 New High Score!` banner
- Boss lasers fire from outer edges of sprite (not center); charge-up sequence (0.25s border lighten → particles → flash + thunder)
- Normal wave chiptune melody: DO NOT MODIFY

## Visual Details to Preserve
- Victory/killer zombie: natural `hsl(${hue},52%,28%)` body — never `#00ff44`
- Victory zombie teeth: `toothW * 0.8` width with `toothW * 0.1` gap — not `toothW - 2`
- "by Louis" on title: `italic 18px monospace`, color `#c8a84b`, with breathing room above and below
- Angry zombies: red face, angled eyebrows, snarl mouth
- Boss: enters angry visual state when player is within 2x normal anger radius

## HUD Elements
- Wave number, score (kills), lives (label "Lives: " left of count), countdown respawn timer, version number

## Controls
| Input | Action |
|-------|--------|
| Arrow keys | Move ship |
| Spacebar | Fire lasers |
| Esc | Pause/unpause |
| Touch joystick (bottom-left) | Move ship |
| FIRE button (bottom-right) | Fire lasers |
| Double-tap iPad (outside controls) | Pause/unpause |

## Phase History
| Phase | Summary |
|-------|---------|
| 1 | Core gameplay, ship, zombies, waves |
| 2 | Boss battles, respawn timer, proximity aggression |
| 3 | Original audio |
| 4 | Touch controls, iPad layout |
| 3b | 8-bit music, laser SFX, angry zombie visuals |
| 5 | Ship polish, boss lasers, virtual joystick, death animations, leaderboard |
| 6 | Respawn immunity, boss overhaul, victory zombie smile, lives label, frantic boss music |
| 7 | Zombie laugh fix, speed increases, zombie idle animation, leaderboard name/date/high score |
| 8 | Pause (Esc + double-tap), boss laser overhaul, smile fix, kill sounds, boss anger state |
| 9 | Particle cap (`MAX_PARTICLES`), Escape key-repeat guard, high score flash banner, teeth fix, title styling |

## Future Planned Features
- Global persistent leaderboard via Supabase (POST/GET `/scores`), scores tagged by game version, production vs dev versioning
