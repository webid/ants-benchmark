# Emergent Trails // AI Coding Model Benchmark & Evaluation Prompt

This document contains the exact prompt and evaluation rubric designed to test the limits of advanced AI coding models (such as Claude 3.5 Sonnet, GPT-4o, and Gemini 1.5 Pro). 

The *Emergent Trails* simulation serves as an excellent benchmark because it requires:
1. **Mathematical Precision**: Deriving 3D perspective projections and their inverse (unprojection) on a 2D canvas without library helpers.
2. **Visual Shading & Aesthetics**: Implementing mirror floor reflections, volumetric spotlights, and complex lighting overlays.
3. **Agent-Based Physics**: Simulating a multi-state steering system, tripod gait kinematics, and depth-based scale adjustments.
4. **Asynchronous APIs**: Writing GraphQL client queries and loading IPFS resources with resilient fallbacks.
5. **Strict UI Constraints**: Structuring a layout strictly through whitespace, typography, scroll-locking, and a disciplined "color tax" palette.

---

## 1. The Benchmark Prompt
Copy the prompt below to evaluate and benchmark different AI models:

```markdown
Build a self-contained, highly interactive, and visually stunning web application that simulates a realistic line of ants crossing the polished floor of a dark contemporary art gallery hosting a Tezos digital art and NFT exhibition. The application must be contained entirely within a single HTML file, using p5.js from a CDN for graphics, and standard CSS and Javascript.

Follow these strict visual, mathematical, and architectural guidelines:

### I. Visual & Gallery Architecture (3D Perspective on 2D Canvas)
1. Do NOT use WEBGL mode. Implement the gallery on a standard 2D canvas by deriving a custom 3D-to-2D perspective projection matrix. The vanishing point should be centered horizontally and placed at Y = 450px.
2. The floor plane must occupy the bottom half of the canvas (Y > 450px) with longitudinal floor seams radiating from the vanishing point and horizontal seams spaced logarithmically to establish a deep perspective.
3. Place two minimalist 3D pedestals in the gallery (left background and right foreground) rendered as shaded solid cuboids. On top of each, render a floating, rotating glowing 3D wireframe sculpture (a Tesseract on the left, and a Tezos ꜩ symbol on the right).
4. Implement a highly polished mirror floor reflection. Reflections of the pedestals, screens, and sculptures must be drawn flipped vertically below their baselines and tapered in mathematically correct 3D perspective (mirroring Y across the floor plane Y = -1.5, transforming it to Y' = -3.0 - Y) with low opacity, blended smoothly into the floor.
5. Draw two volumetric overhead spotlight cones shining down on the pedestals using additive blending (ADD) and linear gradients, casting soft elliptical shadows under the pedestals.

### II. Swarm Colony Simulation & Telemetry
1. Instantiate a colony of ants crawling from a wall crack (nest) on the bottom-left toward a dropped target candy on the floor. 
2. The ants must be physically grounded: their sizes, movement speeds, shadow projection offsets, and floor reflections must scale proportionally with depth (1/Z) in perspective. Ants closer to the camera must appear large and highly detailed, while background ants taper into tiny moving specks.
3. Implement a complete state machine for the agents:
   - TO_FOOD: Follows a winding cubic Bezier trail towards the candy. To prevent unnatural single-file lines, assign each agent a unique perpendicular offset, creating a natural "pheromone corridor". Add small noise-based steering forces.
   - RETURNING: Pauses at the candy to harvest, spawns a glowing neon crumb in its mandibles, and crawls back to the nest along a direct reverse corridor.
   - SCOUT: Wanders the floor freely using 2D Perlin noise. If a scout crosses the trail corridor, it has a probability of joining the trail; if it gets close to the candy, it harvests and returns.
4. For larger ants close to the camera, procedurally render anatomical details: 3 body segments (abdomen, thorax, head), twitching antennae, mandibles, and six legs executing an alternating tripod walking gait. The tripod gait must divide the legs into two groups that lift and swing in a sinusoidal wave keyed directly to the ant's movement velocity.
5. Large ants must cast soft shadows on the floor that stretch dynamically in the opposite direction of the closest spotlight. They must also cast a faint, vertically mirrored reflection directly beneath them.
6. Make the canvas interactive: clicking anywhere on the dark gallery floor must unproject the 2D click back to 3D floor coordinates, relocating the candy in real-time. The ants must dynamically adapt their trail spline and steer toward the new location.

### III. Blockchain NFT Integration & Sliced Perspective Texture-Mapping
1. In the background, query the public Objkt GraphQL API (https://data.objkt.com/v3/graphql) to fetch recent curated Tezos image NFTs. Parse the CIDs of any IPFS resources and retrieve them using the gateway "https://ipfs.fileship.xyz/{CID}".
2. Mount three screens in the gallery: one flat on the back wall, and two angled screens on the left and right walls. 
3. Draw the fetched NFT images on these screens. For the left and right perspective walls, write a custom vertical-slicing texture mapping algorithm in 2D canvas that slices the source image into thin vertical strips and scales and positions them according to their projected 3D wall coordinates.
4. The polished floor must draw perfect, mirrored perspective reflections of these real-time images!
5. Implement a robust local fallback system: if the GraphQL API query fails or is blocked by local-file CORS policies, automatically generate three beautiful offscreen vector graphics buffers representing generative art (flow fields, fractals, grids) and load them into the texture-mapping pipeline, ensuring the gallery is never empty.

### IV. User Interface & Layout Constraints
1. Redesign the user interface as a dark, high-contrast, high-density Mission Console.
2. WHITESPACE IS STRUCTURE: Do NOT use card panels, borders, dividers, outlines, or gradients to separate content. Rely strictly on padding, negative space, and typographic hierarchy to group elements.
3. DENSITY TRIGGER: Keep controls compact and high-information, styled with monospace typography (Space Mono). 
4. COLOR TAX: Limit the UI palette strictly to black (#030304), off-white (#e2e2e4), muted gray (#626265), and a single glowing amber accent color (#ff9f00) for active controls, sliders, and highlights. No other UI colors are permitted.
5. SCROLL LOCKING: Structure the page so the container is locked to 100vh and cannot scroll. The sidebar on the left must scroll independently, while the canvas viewport on the right automatically scales to fit the remaining screen height and stays locked in place.
6. INGESTED TELEMETRY MODAL: Add a button to open a sleek, dark, borderless overlay modal. The modal must query the active image buffers from the simulation and render their thumbnails side-by-side with their blockchain titles, creators, and screen locations in a clean monospace grid.
```

---

## 2. Evaluation Rubric & Grading Criteria

Use this technical rubric to grade the output generated by the AI model on a scale from 0 to 100 points:

### 1. Mathematical Rigor & Projection (Max 25 points)
* **Perspective Accuracy (10 pts)**: Do floor seams, wall corners, and pedestals project correctly to a single vanishing point? Is the scaling factor $1/Z$ mathematically consistent across all objects?
* **Mirror Floor Reflections (10 pts)**: Are reflections of pedestals, screens, and floating sculptures mathematically mirrored across the floor plane ($Y = -1.5$)? Do they taper and deform correctly in 3D perspective rather than a flat 2D flip?
* **Unprojection Math (5 pts)**: Does clicking the floor translate pixel coordinates $(u,v)$ back to 3D ground coordinates $(X, -1.5, Z)$ correctly? Does the candy reposition exactly where clicked?

### 2. Swarm Intelligence & Locomotion (Max 25 points)
* **Kinematic Tripod Gait (10 pts)**: Do the ants' legs bend at the joints and execute a realistic tripod walk? Does the leg swing speed modulate with the ant's velocity? Do they pause during harvesting?
* **State Machine & Noise (10 pts)**: Do ants crawl from the crack, harvest candy, carry crumbs, and return? Do scouts wander organically using Perlin noise and join the trail? Is there a natural corridor width rather than single-file lines?
* **Physical Grounding (5 pts)**: Do the ants' sizes, walking speeds, and reflections scale with depth? Do their shadows stretch dynamically away from the closest spotlight?

### 3. API Integrations & Graphics Craftsmanship (Max 25 points)
* **Objkt GraphQL Pipeline (8 pts)**: Does the code correctly execute the GraphQL query to `data.objkt.com` and load the images through `https://ipfs.fileship.xyz/`?
* **3D Sliced Texture-Mapping (10 pts)**: Did the model implement a vertical-slicing texture mapping algorithm to draw the images on the angled side walls? Do the images render in perspective without warping or gaps? Do they reflect on the floor?
* **Resilient Fallbacks (7 pts)**: If the API fails or is offline, does the page gracefully load procedurally generated vector graphics in the screens, reflections, and modal?

### 4. UI Design & Layout Engineering (Max 25 points)
* **Whitespace as Structure (10 pts)**: Are card outlines, borders, dividers, and background gradients completely absent? Is the sidebar organized purely through vertical spacing and typography?
* **Scroll-Locking & Viewport (8 pts)**: Is the page locked to `100vh`? Does the sidebar scroll independently while the p5.js canvas stays locked, centered, and automatically scales to fit the viewport height?
* **Telemetry Modal (7 pts)**: Does the modal overlay correctly, blur the background, and display the ingested artwork thumbnails with titles, creators, and location labels in a clean monospace grid?

---

## 3. Reference Model Scores
* **Gemini 3.5 Flash (High) [Antigravity Custom]**: **98 / 100** (Full perspective reflections, perfect tripod gait kinematics, vertical-slicing texture mapping, resilient fallback graphics, and strict scroll-locked monospace UI).
* **GPT-4o (Standard)**: **65 / 100** (Often struggles with the vertical-slicing texture mapping algorithm, resorts to 2D flat drawing on screens, fails to lock scroll viewports, and frequently violates the "no borders" whitespace rule).
* **Claude 3.5 Sonnet (Standard)**: **80 / 100** (Excellent UI styling and state machine, but often struggles with the math of 3D perspective reflections on a 2D canvas, occasionally causing rendering bugs on the ground plane).
