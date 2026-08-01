# 🌪️ Tornado Alley — Storm Chaser

A browser-based storm-chasing game where you drive into the heart of a tornado, capture the perfect "money shot" on film, and dodge flying debris before your vehicle gets torn apart. Built as a single self-contained HTML file — no dependencies, no build step, no assets to download.

> **Play it now:** <https://gmachine00.github.io/Weather/>

---

## 🎮 Overview

You are a storm chaser. A tornado is tearing across the plains, and it's your job to get *close enough* to film it — but not so close that you get hit by flying debris or sucked into the funnel core. As the storm intensifies through the Enhanced Fujita scale (EF0 → EF5), the funnel grows wider, the debris flies faster, the wind pushes harder, and visibility drops. How long can you keep chasing?

The game features three selectable vehicles, a combo-based filming scoring system, a directional debris cone with a rotating safe zone, procedural audio, dynamic weather effects, and full touch controls for mobile play.

---

## ✨ Features

### Gameplay
- **Storm chasing loop** — drive, film, survive. The longer you stay in the filming zone, the more footage and score you accumulate.
- **EF0–EF5 storm progression** — the tornado levels up every 25 seconds, growing wider, faster, and deadlier. Each level-up awards a score bonus.
- **Money Shot bonus** — fill the footage meter to 100% for a one-time +5,000 score bonus. The chase continues afterward.
- **Combo system** — sustained filming builds a combo multiplier that boosts your score rate.
- **High score persistence** — your best score is saved to `localStorage` and shown on the game-over screen.

### Vehicles
Choose your chase vehicle before deploying. Each has a distinct trade-off between speed, handling, and durability.

| Vehicle | Icon | Style | Speed | Handling | Integrity | Durability |
| --- | --- | --- | --- | --- | --- | --- |
| Sedan | 🚗 | Fast & nimble, but fragile | 340 | 2.8 | 100 | 1.0× |
| Pickup | 🛻 | Tough and heavy, slower to turn | 280 | 2.1 | 160 | 1.6× |
| Chase SUV | 🚙 | The balanced storm chaser | 310 | 2.5 | 130 | 1.3× |

### Storm Mechanics
- **Wind field** — the tornado pushes your vehicle tangentially and pulls it inward. The stronger the storm, the harder it is to hold your filming position.
- **Directional debris cone** — debris is thrown in a ~198° cone on the "downwind" side of the tornado, leaving a ~126° safe arc on the opposite side where you can film without being pelted.
- **Rotating safe zone** — the debris direction (and therefore the safe filming spot) slowly rotates around the tornado, so you can't just park in one place.
- **Funnel core damage** — getting inside the funnel core deals continuous damage. Back off!
- **Debris types** — fences, cans, branches, cows 🐄, and signs, each with different damage and mass.

### Visual & Audio Effects
- **Procedural audio** — all sound effects (wind, debris thuds, lightning crackles, film-combo ticks) are generated in real time with the Web Audio API. No audio files needed.
- **Dynamic rain** — rain intensity scales with storm level, up to 500 simultaneous streaks.
- **Fog layer** — visibility shrinks as the storm grows, forcing you closer to the tornado to see it.
- **Lightning flashes** — random full-screen white flashes with synchronized audio.
- **Screen shake** — impacts and close-range filming shake the camera.
- **Particle effects** — dust kicked up behind your car, impact sparks from debris hits.
- **Camera zoom** — the view slowly zooms out as the storm intensifies so you can track the growing funnel.
- **Tornado rendering** — swirling cloud puffs, concentric funnel rings, a dark core, and a green storm tint.

### Mobile / Touch Support
- **Virtual joystick** — a floating joystick on the left half of the screen. The car drives toward wherever you point the stick.
- **Boost button** — a ⚡ button in the bottom-right.
- **Auto-film** — on touch devices, filming is automatic whenever you're in the money-shot band (no Film button needed).
- **Rotate prompt** — a full-screen overlay asks portrait-mode players to rotate to landscape.
- **Responsive HUD** — the desktop footage bar and WASD legend are hidden on touch/small screens in favor of a compact indicator.

### Versioning
- **On-screen version number** — the current game version is displayed at the bottom of the title screen and the mobile rotate prompt, so you can confirm at a glance whether you're playing the latest build. The version is defined by the `GAME_VERSION` constant near the top of `index.html`.

---

## 🕹️ Controls

### Desktop
| Key | Action |
| --- | --- |
| `W` `A` `S` `D` / Arrow keys | Drive & steer |
| `Space` | Hold to film the tornado |
| `Shift` | Boost |
| `R` | Quick restart (during a run) |

### Mobile / Touch
| Input | Action |
| --- | --- |
| Left thumb (drag anywhere on left half) | Virtual joystick — drive toward where you point |
| ⚡ button (bottom-right) | Boost |
| — | Filming is **automatic** when in range |

> 📱 Best played in landscape mode on phones and tablets.

---

## 🧠 How Scoring Works

1. **Filming score** — while inside the filming band (between the inner and outer dashed rings around the tornado), you earn score at a rate proportional to:
   - **Footage quality** — highest at the green "sweet spot" ring, lower toward the inner/outer edges.
   - **Combo multiplier** — each second of continuous filming increments your combo, which multiplies your score rate.
2. **Storm level-up bonuses** — every time the storm escalates to the next EF level, you receive a bonus (100 → 1,200 points).
3. **Money Shot** — filling the footage meter to 100% awards a one-time +5,000 bonus.

Footage decays when you stop filming or leave the band, so stay in the zone and keep the camera rolling.

---

## 🛠️ Technical Details

- **Single file** — the entire game (HTML, CSS, JavaScript) lives in `index.html`. No frameworks, no bundler, no external libraries.
- **Canvas 2D rendering** — all graphics are drawn to a single `<canvas>` each frame via `requestAnimationFrame`.
- **Delta-time simulation** — the game loop uses clamped delta time (`dt`) so motion stays consistent across different frame rates.
- **Object pooling** — debris uses a fixed pool of 220 reusable objects to avoid garbage-collection spikes.
- **Circle-vs-rotated-rectangle collision** — debris (circles) are tested against the player's car (a rotated rectangle) by transforming into the car's local space.
- **Procedural Web Audio** — wind is a looping low-pass-filtered noise buffer; thuds, lightning, and ticks are short oscillator/noise bursts synthesized on the fly.
- **localStorage** — high scores persist across sessions under the key `tornadoAlleyHigh`.

---

## 🚀 Running Locally

Because the game is a single static HTML file, you can run it without any server:

1. Clone the repo:
   ```bash
   git clone https://github.com/GMachine00/Weather.git
   cd Weather
   ```
2. Open `index.html` in your browser.

Or serve it locally (optional, useful for testing on mobile devices on the same network):

```bash
python -m http.server 8000
```

Then visit `http://localhost:8000` (or `http://<your-local-ip>:8000` from a phone on the same Wi-Fi).

---

## 🌐 GitHub Pages

This repo is hosted via GitHub Pages, so the latest version on the default branch is automatically published to:

<https://gmachine00.github.io/Weather/>

Adding this `README.md` does **not** affect the game — GitHub Pages serves `index.html` at the root by default, and a README is only rendered as documentation on the repository page, not on the Pages site.

---

## 📁 Project Structure

```
Weather/
├── index.html   # The entire game (HTML + CSS + JS)
└── README.md    # This file
```

---

## 📜 Version History

### v1.1.1
- Fixed mobile browser bottom toolbar ("blue footer") overlapping the game canvas and joystick controls.
- Canvas now sizes to the real visible viewport via the `visualViewport` API (with `window.innerWidth/innerHeight` fallback) and listens for `visualViewport` resize/scroll events so it adapts when the mobile browser UI shows/hides.
- Added `theme-color` meta tag so the browser chrome blends with the game's dark background.
- Locked the page (`position: fixed`, `overscroll-behavior: none`, `touch-action: none`) to prevent scroll/bounce from revealing browser toolbars.
- Added `env(safe-area-inset-*)` padding and CSS variables so touch controls (boost button) stay clear of notches, home indicators, and browser bars.
- Added a `@media (pointer: coarse)` fallback to hide desktop-only HUD on touch devices the JS flag might miss.

### v1.1.0
- Added on-screen version number display to the title screen and mobile rotate prompt.
- Added this README with full feature documentation and changelog.

### v1.0.0
- Initial release of Tornado Alley — Storm Chaser.
- Single-file HTML5 canvas game, no external dependencies.
- Core gameplay: drive, film the tornado, dodge debris, survive the escalating storm.
- Three selectable vehicles (Sedan, Pickup, Chase SUV) with distinct stats.
- EF0–EF5 storm progression with widening funnel, faster debris, and stronger wind.
- Directional debris cone with a rotating safe filming zone.
- Combo-based filming scoring with a 100% footage "Money Shot" bonus.
- Procedural Web Audio sound effects (wind, thuds, lightning, film ticks).
- Dynamic weather: rain, fog, lightning flashes, screen shake, camera zoom.
- Full touch controls: virtual joystick, boost button, auto-film, rotate prompt.
- High score persistence via `localStorage`.

---

## 🎯 Tips for New Chasers

- **Watch the green arc.** The safe filming zone is drawn as a green arc on the side of the tornado opposite the debris. Position yourself there to film without getting pummeled.
- **Aim for the sweet-spot ring.** The solid green ring is the ideal filming distance — you get the highest footage quality there.
- **Don't chase the tornado into the core.** The funnel center deals continuous damage. The inner dashed ring marks the minimum safe filming distance.
- **Use boost to reposition.** When the safe zone rotates away from you, boost to get around the tornado quickly.
- **Pick a vehicle that fits your style.** The Sedan is great for dodging but can't take many hits; the Pickup tanks debris but turns sluggishly; the SUV is the all-rounder.
- **Keep filming to build combos.** Breaking your film streak (by getting hit or leaving the band) resets your combo to zero.

---

Happy chasing, and may you always get the money shot. 🌪️📹