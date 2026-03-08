🚀 Zombie Space Shooter — Game Design Specification
Project: Single-page HTML5 Canvas game
Version: 1.0
Platform: Browser (iPad primary, MacBook Pro secondary)

Overview
A space shooter inspired by Space Invaders and Asteroids. The player pilots a triangle-shaped spaceship that always faces upward, shooting twin lasers at geometric zombies that approach from all directions. The game is level-based with escalating difficulty and boss battles every 6 waves.

Controls
Touch (Primary — iPad):

Drag the ship to move it around the screen
Tap a fixed "FIRE" button to shoot

Keyboard (Secondary — MacBook):

Arrow keys to move the ship (up, down, left, right)
Spacebar to shoot


The Ship

Shape: Triangle (pointing upward at all times — never rotates)
Movement: Moves in all four directions; always faces up
Shooting: Fires two parallel lasers straight upward simultaneously
Boundary: Cannot leave the playable screen area
Customizable: Color and visual style configurable in code


Zombies
Regular Zombies:

Shape: Simple geometric design (circle body with blocky zombie features, drawn with canvas)
Behavior: Spawn from random screen edges, drift toward the center/player
Proximity aggression: If the player gets too close, zombies accelerate toward the ship
Screen wrapping: Zombies wrap around screen edges (exit bottom → enter top, etc.)
On death: Disappear, then respawn after a set timer
Goal: Kill all zombies before they respawn

Boss Zombies (every 6th wave):

Shape: Oversized version of the regular zombie (3–4x larger)
Health: Takes many hits to defeat (scales with level)
Movement: Slow in early boss battles, progressively faster in later ones
Music: Triggers ominous boss music when active


Wave & Level System

Wave 1: 3 zombies
Each subsequent wave: +2 zombies
Every 6th wave: Boss battle (replaces normal wave)
Win condition: Kill all zombies before any respawn
Progression: Waves increase in difficulty (more zombies, faster movement, shorter respawn timers)
Goal: Survive as many waves as possible; track highest wave reached


Respawn Mechanic

When a zombie is killed, a timer starts
If the player hasn't cleared all zombies before the timer expires, killed zombies respawn
Respawn timer gets shorter at higher levels, increasing pressure


Audio
Background Music:

Normal waves: Light, ambient space music
Boss waves: Deep, ominous, tense music
Music transitions when boss wave begins/ends

Sound Effects:

Laser fire: Sound plays each time the ship shoots
Zombie ambient noise: Each zombie randomly emits zombie sounds at irregular intervals
More zombies on screen = more frequent zombie noises

Note: Audio will use the Web Audio API or short embedded audio clips for HTML compatibility.

Visuals
ElementDesignShipWhite/green triangle, always pointing upLasersTwo thin parallel vertical beams, neon colorRegular ZombiesGeometric canvas drawing (circle + zombie details)Boss ZombieSame design, 3–4x larger, different color (red/dark)BackgroundBlack space with subtle star fieldUIWave number, score, lives displayed on screen

UI / HUD

Current wave number
Score (zombies killed)
Lives or health indicator
Touch: Visible FIRE button on screen
Game over screen with highest wave reached
"Next Wave" transition screen between levels


Technical Spec

Format: Single .html file (HTML + CSS + JavaScript inline)
Rendering: HTML5 Canvas API
Audio: Web Audio API (generated tones) or base64-encoded audio
No external dependencies (fully self-contained for easy GitHub hosting)
Touch events: touchstart, touchmove, touchend for iPad support
Keyboard events: keydown for arrow keys and spacebar


GitHub / Version Control Notes

Single file: zombie-space-shooter.html
Copy from iPad to MacBook, then git init and push to GitHub
Iterative development: build a playable v1 first, then layer in audio, boss mechanics, and polish


Development Phases
Phase 1 (MVP): Ship movement, laser firing, basic zombies, wave system, win/lose condition
Phase 2: Boss battles, respawn timer mechanic, proximity aggression
Phase 3: Audio (music + SFX + zombie sounds), visual polish, star field background
Phase 4: Touch controls, iPad optimization, HUD polish