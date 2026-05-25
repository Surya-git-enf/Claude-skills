# /portals

Build interactive hero landing pages with portal reveal animation, cursor-reactive foliage, and parallax depth — inspired by Daniel Snows / immersive studio websites.

## Trigger phrases
- "build a portal landing"
- "step into wonder style page"
- "portal reveal animation"

## Required user inputs
9 PNG assets in ./images/:
- sky.png, full-scene.png
- portal-left.png, portal-right.png  
- foreground-flowers.png
- foliage-tl.png, foliage-tr.png, foliage-bl.png, foliage-br.png

All foliage PNGs must be pre-oriented for their corners. Never apply flip transforms.

## Architecture
Layers (back to front):
1. Sky background (sky.png)
2. Portal layers (portal-left.png + portal-right.png overlapping at center)
3. Full scene revealed after portal zoom out (full-scene.png)
4. Foreground flowers (foreground-flowers.png)
5. Four corner foliage elements (foliage-tl.png, foliage-tr.png, foliage-bl.png, foliage-br.png)

Timeline:
- 0-2500ms: Portal zoom from scale(1.8) to scale(1.0) with easeInOutCubic
- 2200-3200ms: Crossfade - portal layers fade out (opacity 1→0), full-scene fades in (opacity 0→1) simultaneously
- After 3200ms: Full scene visible, foliages react to cursor, parallax effect on mouse move

## Critical rules
- Do NOT apply flip transforms to foliage PNGs - they must be pre-oriented
- Do NOT use scale transforms on portal elements beyond the zoom animation
- Do NOT modify the base opacity animations without adjusting crossfade timing
- Do NOT remove cursor-reactive behavior from foliage elements
- Do NOT change the easeInOutCubic easing without updating related timing values

## Default values
- Portal zoom: 1.8 → 1.0 over 2500ms
- Crossfade window: 2200—3200ms
- Foliage push radius: 400px, strength 45—50px
- Cursor lerp: 0.18
- Foliage lerp: 0.08
- Easing function: easeInOutCubic

## When user wants to modify
- Slower animation: increase ZOOM_DURATION (default 2500ms)
- Stronger foliage push: increase data-strength attribute on each foliage element (default 45-50px)
- Bigger zoom: increase the starting scale value (default 1.8)
- Different easing: replace easeInOutCubic with another easing curve (e.g., easeInOutQuad, linear, etc.)
- Adjust foliage sensitivity: modify data-radius attribute (default 400px)
- Change lerp values: adjust cursor lerp (default 0.18) and foliage lerp (default 0.08) for different responsiveness

## Implementation notes
The skill assumes a modern web stack with HTML/CSS/JavaScript. The implementation should:
1. Use requestAnimationFrame for smooth animations
2. Implement pointer event listeners for cursor tracking
3. Use CSS transforms for optimal performance
4. Apply the specified easing functions to animation timing
5. Ensure all foliage elements maintain their pre-oriented corners
6. Maintain the parallax depth effect on mouse movement