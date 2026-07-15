# agentic-galaxy-simulation 🌌 Pralaya — a living galaxy of AI civilizations

**A single self-contained HTML file.** No build step, no backend, no dependencies. Open `index.html` and you're inside a real map of the solar neighbourhood — 3,385 catalogued stars and 18 real exoplanet systems — where each world is an autonomous agent that forms goals, builds trust, trades, allies, betrays and goes to war. Connect an Anthropic API key and those decisions are made by **live Claude**.

> Pralaya (प्रलय) — the cosmic dissolution at the end of a cycle, before creation begins again.

**Created by [Saagar Shekara Devadiga](https://www.linkedin.com/).**

---

## ▶️ Try it

- **Run it:** open `index.html` in any modern browser. You'll land on a **PRALAYA** entry screen — click **▶ Enter the simulation** to open into the galaxy, or **🎬 Watch the tour** for a ~60-second auto-playing showcase of every feature (great for screen-recording). It runs fully offline.
- **Deploy it:** drop the one file on GitHub Pages / Netlify / any static host.
- **Go live-agentic (optional):** click **🔌 Claude**, paste your own Anthropic API key, and each civilization's turn is decided by a real model call. Without a key it runs a built-in local simulation, so the demo never breaks.

---

## ✨ What's in it

| | |
|---|---|
| 🪐 **Real astronomy** | 3,385 real stars (HYG catalogue, positions/distances/spectra) and 18 real exoplanet systems (NASA Exoplanet Archive) — TRAPPIST-1's seven worlds, 55 Cancri's lava planet, Proxima b, 51 Peg b. Planets are rendered live from their **real radius + temperature**. |
| 🧠 **Agentic civilizations** | Every world is an agent with a goal (survive / endure / dominate / expand / prosper), a private trust ledger, personality and memory. It perceives its situation, decides one action, and acts — producing **emergent** diplomacy, not scripted events. |
| 🔌 **Live Claude** | Bring your own API key and each decision — who to ally with, who to betray, who to declare war on — is made by Claude, with its reasoning shown in the chronicle. |
| 🔬 **Real ⟷ 🌌 Fantasy toggle** | One switch cleanly separates *verified NASA/HYG data* from the *agentic imagination*, so it's always honest about what's real and what's simulated. |
| 📊 **BI dashboards** | A "Galactic Council" of hand-drawn SVG charts — KPI cards, a discovery timeline, isometric 3-D spectral bars, Sankey flows, a trust-matrix heatmap, a diplomacy network graph, radar mind-scans — no charting library. |
| ⚛️ **The four forces** | Gravity as a warped-spacetime grid, electromagnetism as solar wind + magnetospheres, and the strong/weak forces as a live fusion-engine gauge in the star. |
| ⚔️ **Galactic conflict** | War/peace switches that ignite red conflict-zones between star systems. |
| ☠️ **The end of everything** | A master "terminate" switch that glitches reality and storms the screen with a random crash-culture meme (Nyan cat / Party Parrot / Doge / "this is fine" fire) — then the galaxy reforms. |

---

## 🏗️ Architecture & key design decisions

The interesting part isn't any single feature — it's a set of deliberate constraints that make the whole thing shippable, honest, and genuinely agentic.

### 1. One file. No build. No backend. No dependencies.
The entire application — ~2,500 lines of HTML/CSS/JS, the star catalogue, the exoplanet data, every sprite, and all the WebGL shaders — lives in a single `index.html` you can email, host anywhere, or read top to bottom. Data is inlined as JS objects and base64 data-URIs; there is no `npm install`, no bundler, no CDN.

**Why:** maximum portability and longevity. It will still open in ten years. "View source" is the whole app. Deployment is copying one file.

### 2. Real data, and honest about it.
Real astronomy is prepared offline (Python over the HYG v4.1 catalogue and the NASA Exoplanet Archive TAP service) and inlined as `STAR_DATA` / `SYSTEMS`. A world's *type* (terran / rocky / lava / ice-giant / hot-Jupiter…) is chosen from its **real** radius and equilibrium temperature. A **Real ⟷ Fantasy** toggle strips the simulation entirely — pausing the agents, hiding the diplomacy layer, and showing only verifiable facts and orbital data — so the line between measured reality and generative fiction is never blurred.

**Why:** intellectual honesty. Anyone building with generative AI has to be crisp about what is data and what is invention; the toggle makes that a first-class, visible feature.

### 3. A swappable brain — the agent framework comes first, the LLM is an upgrade.
Each civilization runs a classic **perceive → decide → act** loop. The decision itself is isolated in one function, `decideAction(agent)`, which returns a structured `{ action, target, reason }`. That function has two implementations behind one interface:

- **Local** — deterministic logic over goals, relative strength, trust and personality (fast, free, always available).
- **Live Claude** — the same agent state is sent to the Anthropic Messages API, and Claude returns the decision as JSON.

`agentTurn()` routes to Claude when connected and **gracefully falls back** to the local brain on any error, timeout, or budget cap.

**Why:** the demo must always work (no key, no network → local sim), while the LLM becomes a genuine, drop-in upgrade rather than a hard dependency. Build the orchestration first; make the model pluggable.

### 4. Browser-native LLM calls, with a real security model.
Live Claude runs entirely client-side with a bring-your-own-key model:

- The key lives **only in the tab's memory** (a JS variable) — never written to `localStorage`, disk, or any server.
- A **Content-Security-Policy** (`connect-src https://api.anthropic.com`) makes the API the *only* network destination the page can reach — the key physically cannot be exfiltrated anywhere else.
- **Budget & timeout caps** (max calls, max tokens, 15 s abort) and a **"forget key"** button.

**Why:** a portfolio piece that touches API keys should model responsible handling. The CSP turns "trust me" into an enforced guarantee.

### 5. Custom rendering — no engine, no chart library.
- **Planets** are drawn by a single WebGL2 fragment shader with a *mode-dispatch* design: nine procedural planet/star/black-hole shaders (ported from the MIT-licensed **Deep-Fold "Pixel Planets"** Godot shaders) compiled into one program, switched per-body by a `mode` uniform. Earth's continents come from a locally-built 128×64 land bitmask.
- **Every chart** in the dashboards is hand-drawn SVG (isometric 3-D bars, an animated liquid-wave gauge, Sankey ribbons, radar, network graph) — echoing ECharts looks with zero dependencies.

**Why:** total control, no dependency risk, and it keeps decision #1 intact (one file, offline).

### 6. Emergence over scripting.
Nothing in the diplomacy is hand-authored. Alliances, betrayals and wars *emerge* from agents pursuing goals against a live trust model that shifts with every interaction and decays over time. Run it twice and you get a different history.

---

## 🧰 Tech stack

`Vanilla JS` · `WebGL2 / GLSL ES` · `HTML5 Canvas 2D` · `SVG` · `Anthropic Messages API (optional)` · Python (offline data prep only)

No frameworks. No bundler. No runtime dependencies.

---

## 📚 Data & credits

- **Stars:** [HYG stellar database v4.1](https://www.astronexus.com/hyg) (positions, distances, spectra, B–V colour).
- **Exoplanets:** [NASA Exoplanet Archive](https://exoplanetarchive.ipac.caltech.edu/) (radii, masses, equilibrium temperatures, orbital periods, discovery years).
- **Planet shaders:** [Deep-Fold "Pixel Planet Generator"](https://deep-fold.itch.io/pixel-planet-generator) — MIT licensed, © 2020 Deep-Fold — ported from Godot to WebGL2/GLSL.
- **Live reasoning:** [Anthropic Claude](https://www.anthropic.com/) (optional, user-supplied key).

---

## 🗺️ Why I built it

I wanted to close the gap between "AI as flavour text" and **real multi-agent orchestration** — the pattern agentic-AI work actually hires for. Pralaya is agentic in its architecture (autonomous, goal-driven agents negotiating), and with one API key it becomes agentic in the literal sense: N Claude agents reasoning and negotiating live, in the browser, in a single file you can host for free.

---

*Built as a self-contained experiment in agentic simulation, real-data visualization, and dependency-free web rendering.*

*© Saagar Shekara Devadiga. Deep-Fold Pixel Planet shaders used under their MIT licence.*
