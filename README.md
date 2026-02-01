# Interactive Whale (HTML/CSS/JS)

An interactive front-end animation that renders a whale-like shape in **SVG** and makes it **follow the cursor** with a smooth trailing effect. No build step, no framework.

**Author / maintainer:** Sergio Sediq — `https://github.com/SergioSediq`

![Interactive Whale preview](assets/preview.png)

## Demo / What it does

- **Interactive animation**: the whale follows the mouse position (`mousemove`).
- **Smooth motion**: movement is eased so the body “trails” naturally.
- **Responsive**: scales to fill the viewport.
- **No build step**: runs as a static page.

## Technologies used

- **HTML5**: page structure
- **CSS3**: full-viewport layout
- **JavaScript**: generates and animates an inline SVG in real time

## Project structure

- `interactive-whale.html`: entry page
- `interactive-whale.css`: minimal layout styles (full viewport)
- `interactive-whale.js`: whale renderer + motion logic (SVG generation + easing)
- `jquery.js`: included in the HTML (not required by the current `interactive-whale.js`)

## How to run

### Option A: Open directly

Open `interactive-whale.html` in your browser.

### Option B (recommended): Run a local server

From PowerShell in this folder:

```powershell
python -m http.server 8000
```

Then open `http://localhost:8000/interactive-whale.html`.

If you have Node.js, you can also do:

```powershell
npx serve .
```

## How it works (technical)

### Rendering model

`interactive-whale.js` builds an SVG string and assigns it to `#whale` via `element.innerHTML` on a fixed interval.

- Each “segment” is an entry in `parts[]` that contains:
  - `x`, `y`: current segment position (in screen pixels)
  - `z`: segment index used to stagger updates (creates the trailing effect)
  - `data`: pre-authored SVG path markup for that segment
- On every tick:
  - The code schedules a `transform()` for each part with `setTimeout(..., part.z * delay)`.
  - `transform()` nudges each segment towards the current mouse position with easing + max speed clamp.
  - The SVG is regenerated from the updated segment positions.

### Input handling

Mouse position is captured via:

- `document.addEventListener('mousemove', mousemove)`
- `mousemove()` stores `mouse = { x: e.clientX, y: e.clientY }`

### Motion / easing math

For each segment:

- Compute delta: `dx = mouse.x - part.x`, `dy = mouse.y - part.y`
- Clamp delta to `[-maxspeed, maxspeed]`
- Apply easing: `part.x += dx / easy`, `part.y += dy / easy`

This makes the whale move smoothly rather than snapping directly to the cursor.

## Customization ideas

### Tweak motion/feel (recommended)

In `interactive-whale.js`, try adjusting:

- `fps`: update frequency for the main loop (higher = smoother, higher CPU)
- `easy`: easing divisor (higher = slower response + “floatier” trailing)
- `maxspeed`: per-axis clamp for the per-tick delta (higher = faster “catch up”)
- `delay`: stagger multiplier in ms per segment index `z` (higher = longer trail/lag)

Tip: if it feels jittery, try lowering `maxspeed` or increasing `easy`.

### Visual upgrades

- Add an ocean gradient / background image to `interactive-whale.css`
- Add bubbles, waves, or particles (CSS or Canvas)
- Change colors by editing the SVG fill/gradient data in `interactive-whale.js`
- Add sound effects on interaction (e.g., click to “splash”)

## Notes

- This is intentionally “raw” DOM/SVG string rendering (not Canvas/WebGL). If you want higher performance, the next step would be to create the SVG once and update transforms/attributes instead of rewriting `innerHTML` every tick.

## Use cases

- Marine education pages
- Kids’ learning platforms
- Animation / creative coding portfolios
- Front-end motion practice

