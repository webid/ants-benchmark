# Emergent Trails // Swarm Colony & Digital Art Curation Matrix

*Emergent Trails* is a high-fidelity interactive simulation that explores the intersection of biological micro-intelligence and human digital abstraction. Set on the polished, reflective floor of a dark contemporary art gallery hosting a Tezos generative NFT exhibition, the project stages a visual dialogue between two self-organizing systems: the pheromone-driven path-finding of an ant colony and the mathematical algorithms of generative digital art.

The application is written as a highly optimized, single-file interactive dashboard using **p5.js** for graphics, custom **3D-to-2D perspective projection**, and the public **Objkt GraphQL API** to ingest real blockchain digital art assets in real-time.

---

## Technical Architecture

The codebase is structured around two core subsystems: the **3D Gallery Environment** and the **Swarm Locomotion Engine**.

```
                           +----------------------------------+
                           |     Objkt GraphQL API Query     |
                           +----------------------------------+
                                            |
                                            v
                             https://ipfs.fileship.xyz/{CID}
                                            |
                                            v
+-----------------------------------------------------------------------------------+
|  3D Perspective Projection Matrix                                                 |
|                                                                                   |
|    Left Wall Screen [Ingested NFT B]               Right Wall Screen [Ingested NFT C]
|   (3D Sliced Perspective Mapping)                (3D Sliced Perspective Mapping)
|                                                                                   |
|                               Center Screen [Ingested NFT A]                      |
|                                                                                   |
|                   [Pedestal 1]                           [Pedestal 2]             |
|                (Floating Tesseract)                    (Floating Tezos Logo)      |
|                         |                                      |                  |
|                         v                                      v                  |
|                   Shadow Cast                            Shadow Cast              |
|                                                                                   |
|  ------------------ HORIZON (Y = 450px) / FLOOR BOUNDARY -----------------------  |
|                                                                                   |
|   Floor Seams (Perspective Rays) & Logarithmic Grid Lines                        |
|                                                                                   |
|   [Wall Crack / Nest]   ====>   * Pheromone Trail Corridor *   ====>   [Candy]    |
|   (Agent Instantiation)                                         (Target Relocation)
|                                                                                   |
|                           [Swarm Agents: Ant State Machine]                       |
|                          (Tripod Gait Kinematics / Reflections)                   |
|                                                                                   |
+-----------------------------------------------------------------------------------+
```

### 1. 3D Gallery Environment (2.5D Vector Graphics)
Rather than relying on resource-intensive 3D WebGL contexts, the gallery is drawn on a 2D canvas using custom mathematical projections, ensuring high performance (60 FPS) and pixel-perfect vector details.
* **Perspective Matrix**: Ground coordinates $(X, Y, Z)$ are projected onto screen coordinates $(u, v)$ where $Z$ represents depth, $Y$ height, and $X$ horizontal offset:
  $$u = \text{centerX} + \frac{X \cdot f_x}{Z}$$
  $$v = \text{horizon} - \frac{Y \cdot f_y}{Z}$$
  The horizon is locked at $v = 450$px, establishing the boundary between the walls and the floor plane.
* **Polished Mirror Floor Reflections**: Perfect 3D perspective reflections are calculated by mirroring any object's 3D $Y$ coordinate across the floor plane ($Y = -1.5$), transforming it to $Y' = -3.0 - Y$. This ensures that mirrored images taper and deform in mathematically correct perspective rather than a cheap vertical 2D flip.
* **Volumetric Spotlight Beams**: Pedestals are illuminated by overhead spotlights projected as semi-transparent 3D cones that blur into the floor, rendered using p5.js `ADD` blend mode and native HTML Canvas 2D linear gradients.
* **3D Perspective Texture-Mapping**: Real Tezos NFT images fetched from the blockchain are mapped onto the angled side-walls by slicing the source image into 70 vertical columns and drawing them as adjacent, scaled rectangles corresponding to their projected 3D coordinates.

### 2. Swarm Locomotion Engine (Agent-Based Simulation)
The ant colony is simulated using autonomous steering behaviors and kinematic locomotion.
* **State Machine**: Swarm agents operate under three states:
  * `TO_FOOD`: Crawls from the nest crack toward the candy, steering toward nodes along a cubic Bezier trail.
  * `RETURNING`: Pauses at the candy to harvest, spawns a cargo crumb, and crawls back to the nest along a direct reverse heading.
  * `SCOUT`: Wanders the floor using 2D Perlin noise. If a scout crosses the pheromone trail corridor, it has a probability of joining the trail; if it gets near the candy, it harvests and returns.
* **Path Wiggle & Corridor Offset**: To prevent unnatural single-file lines, each ant is assigned a unique perpendicular offset from the central Bezier spline, creating a natural "pheromone corridor" width. Noise-based steering forces are added to simulate organic wiggling.
* **Anatomical Consistency**: Large ants close to the camera are procedurally rendered with 3 distinct body segments (abdomen, thorax, head), twitching antennae, mandibles, and carrying cargo. 
* **Tripod Gait Kinematics**: Leg joints are animated using a sinusoidal wave keyed directly to the agent's travel velocity. Six legs are divided into alternating tripods $\{L_1, R_2, L_3\}$ and $\{R_1, L_2, R_3\}$ that lift and swing forward in perfect rhythmic sync while the opposite group remains planted.
* **Physical Grounding**: As ants move deeper into the gallery, their size, movement velocity, shadow projection offsets, and floor reflections scale proportionally by $1/Z$. Shadows stretch dynamically in the direction opposite the closest pedestal spotlight.

---

## File Structure

```
├── index.html       # Self-contained application (UI, CSS, p5.js Engine, Objkt API)
├── philosophy.md    # Artistic manifesto and computational concepts
├── README.md        # This developer guide
└── BENCHMARK_PROMPT.md # The AI model evaluation prompt and rubrics
```

---

## Getting Started

Because the application is entirely self-contained, no local installation or setup is required. 

1. Simply clone or download the repository.
2. Open `index.html` in any web browser (Chrome, Firefox, Safari).
3. If loaded locally, the browser's security policy might block the outgoing GraphQL API query to Objkt. In this case, the resilient fallback system automatically generates three high-tech vector art buffers, enabling full offline functionality, 3D projections, reflections, and modal telemetry.
4. To test with a local server and bypass local file restrictions, run:
   ```bash
   npx -y http-server ./
   ```
   and open the local port (usually `http://localhost:8080`) in your browser.

---

## Developer Customization Guide

Developers can easily customize and extend the simulation by modifying parameters or code in the script block of `index.html`:

* **Changing Wall Art Projections**:
  To display custom local images instead of Objkt blockchain queries, simply load your images in p5's `preload()` or `setup()` and push them into the `loadedArtworks` array:
  ```javascript
  loadedArtworks.push({
      img: myImage,
      title: "My Custom Piece",
      artist: "Developer",
      isFallback: false
  });
  ```
* **Tuning Swarm Physics**:
  Modify the maximum steering force or speed of the ants in `Ant.update()`:
  ```javascript
  let maxForce = 0.04; // Increase for sharper turns
  let maxSpeed = 0.028 * params.antSpeed * this.speedMult; // Base velocity
  ```
* **Customizing the Gallery Layout**:
  You can reposition the nest crack, the default candy drop, or the pedestals in `initializeSystem()`:
  ```javascript
  nestPos3D = createVector(newX, -1.5, newZ);
  candyPos3D = createVector(newX, -1.5, newZ);
  ```
  The coordinate boundaries of the floor plane are $X \in [-1.8, 1.8]$ and $Z \in [1.1, 5.5]$.
