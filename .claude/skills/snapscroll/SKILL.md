---
name: snap-scroll-builder
description: Build a snap-scroll landing page with flexible sections, data-driven animations, and step-by-step approval workflow.
---

# Snap-Scroll Skill

This skill guides Claude Code through building a snap-scroll website that supports any number of sections, with flexible data-driven sections and layered animations.

## How to Use

Invoke this skill when you have a project folder containing:
- `CLAUDE.md` – the specification (as provided in the guide)
- `reference.html` – the standalone demo HTML from Claude.ai (Prompt 1)
- `public/images/` – folder with your asset images

The skill will:
1. Read the spec and reference demo.
2. Propose a **build plan only** (no code changes).
3. Wait for your approval.
4. After approval, execute the build **step by step**, pausing after each major step to show what changed and ask for confirmation to continue.
5. Explain each change in plain English (non‑technical).

## Skill Workflow

### Phase 0 – Kickoff & Plan
- Read `CLAUDE.md` and `reference.html`.
- Analyze the section data structure implied by the spec.
- Propose a concrete build plan covering:
  - Project initialization (Vite, Tailwind v4, GSAP)
  - HTML skeleton with dynamic section generation
  - CSS layout ported from reference
  - Snap‑scroll JS logic (wheel, touch, keyboard, dots)
  - Data‑driven per‑layer animations (entrance + idle)
- Output the plan in plain English.
- **Pause** and wait for your explicit approval before proceeding.

### Phase 1 – Project Initialization
- Scaffold a Vite vanilla‑JS project.
- Install Tailwind CSS v4 and GSAP.
- Set up folder structure (`public/`, `src/css/`, `src/js/`).
- Show what files were created.
- Explain: *We are setting up the development toolchain so we can write modern HTML/CSS/JS and see live previews.*
- Pause for approval to continue.

### Phase 2 – HTML Base
- Create `index.html` with:
  - Basic meta tags and links to CSS/JS.
  - Placeholder for dynamic sections (we will fill via JS).
  - Font links for Bricolage Grotesque and Inter.
  - Container for progress dots (right‑edge) and side indicators if specified.
- Show the generated HTML.
- Explain: *This is the skeleton that will hold all sections; right now it’s empty but ready for content.*
- Pause for approval.

### Phase 3 – CSS Layout
- Create `src/css/style.css` (or Tailwind config) that reproduces the layout from `reference.html`:
  - Full‑height sections with flex layout for background/foreground layers.
  - Classes for text alignment, image arrangement, and section‑specific styles.
  - Utility classes for entrance animations (opacity, yPercent) and idle animations (scale/drift).
- Show the CSS file.
- Explain: *We are copying the visual look from the demo so the site matches your design before adding motion.*
- Pause for approval.

### Phase 4 – First Preview (Layout Only)
- Run `npm run dev` (or equivalent) to start the dev server.
- Confirm that the layout looks correct: sections stack, images appear, text is positioned as per spec.
- No animations or snap behavior yet.
- Show a brief description of what you should see.
- Pause for approval to move to interactivity.

### Phase 5 – Snap‑Scroll JavaScript
- Create `src/js/snap.js` that implements:
  - Section‑by‑section snapping (one wheel/swipe/arrow/space/dot = one section).
  - Input locking during transition.
  - Support for wheel, touch swipe (≥50px), arrow keys, Space, and click on progress dots.
  - Automatic generation of progress dots based on number of sections.
- Show the JS file.
- Explain: *This adds the “snap” feel: each scroll moves exactly one panel,no scrubbing, and locks UI while animating.*
- Pause for approval.

### Phase 6 – Data‑Driven Section & Layer Model
- Define a JavaScript array `SECTION_DATA` (in `src/js/data.js` or inline) that holds:
  - `id`, `brand`, `title`, `body`, `layoutStyle`
  - `background` (array of image srcs)
  - `foreground` (array of image srcs)
  - `decorative` (optional array)
  - Per‑layer animation settings (type → animation map)
- Show the data structure.
- Explain: *Instead of hardcoding three heroes, we store everything in data so you can add/remove sections by editing this array only.*
- Pause for approval.

### Phase 7 – Dynamic Section Rendering
- Update the HTML generation loop to:
  - Iterate over `SECTION_DATA`.
  - For each section, create a container with the correct layout‑style classes.
  - Append background, foreground, and decorative layers as `<img>` or `<div>` elements with appropriate classes.
  - Insert title, body, brand text.
- Show the updated JS.
- Explain: *Now the page builds itself from the data – no need to touch HTML when you change sections.*
- Pause for approval.

### Phase 8 – Per‑Layer Animations (Entrance + Idle)
- For each layer, read its `type` (background, main subject, decorative, text, etc.) and map to an animation preset:
  - background → slow fade + scale (`opacity 0→1`, `scale 1→1.1x` long duration)
  - main subject → faster yPercent/opacity entrance, optional scale tween
  - decorative → optional drift/scale
  - text → staggered entrance (delay based on index)
  - unknown → sensible default (fade in)
- Use GSAP to create two parallel tweens per layer when a scale tween is needed (one short for opacity/y, one long for scale).
- Ensure all layer animations start at the same time as the snap transition.
- Show the animation JS.
- Explain: *Each visual piece now animates in sync with the snap, and the system adapts automatically if you add more or fewer layers.*
- Pause for approval.

### Phase 9 – Final Review & Polish
- Run the dev server and test:
  - Snapping with mouse wheel, touch, keyboard, dot clicks.
  - Input lock during transitions.
  - Correct animations for each layer type.
  - Responsive behavior (sections full viewport).
- Show a short summary of what works and any open issues.
- Pause for approval to declare the skill complete or request further tweaks.

## What You Should See After Each Step
- After **Project Initialization**: a fresh Vite project with `node_modules`, `vite.config.js`, `package.json`.
- After **HTML Base**: an `index.html` file you can open in a browser to see a blank page with correct title and meta tags.
- After **CSS Layout**: styled sections with placeholder backgrounds and text, matching the reference layout.
- After **First Preview**: a localhost page showing all sections stacked, images visible, no motion yet.
- After **Snap‑Scroll JS**: scrolling (wheel/touch/arrow/dot) moves exactly one section at a time, UI locks during movement.
- After **Data‑Driven Model**: a `SECTION_DATA` array visible in the JS file; changing it updates the page without touching HTML/CSS.
- After **Dynamic Rendering**: the page populates with your actual images, titles, body copy as per the data.
- After **Animations**: each layer fades/scales or drifts in sync with the snap; idle animations (slow scale/drift) continue after entrance.
- After **Final Review**: a fully working snap‑scroll site ready for production build.

## What Could Go Wrong & How We’ll Handle It
- **Missing images**: The console will show 404 errors; we will pause and ask you to verify the filenames in `public/images/` match the data.
- **Animation timing feels off**: We will expose the duration/ease values as variables at the top of the animation file for easy adjustment.
- **Input lock not working**: We will check that the lock variable is set before each snap transition and cleared on completion.
- **Progress dots count mismatch**: We will regenerate dots whenever `SECTION_DATA.length` changes.
- **Layout breaks on resize**: We will use `vh/vw` units and flexible flexbox; if needed we’ll add extra responsive classes.

## Completion
When you approve the final review, the skill will output a message that the snap‑scroll site is complete and ready for:
- `npm run build` (production bundle)
- Deployment to Vercel or any static host.

You can then run the build and deploy as you wish.

---