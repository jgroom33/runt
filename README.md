# How the Runt Works — interactive 3D animation

A single self-contained page that explains the **Diaz Suspension Design "Runt"** dual-chamber air
spring. It shows **three sectioned (cutaway) forks under the same applied load** — a stock single-chamber
air spring (left), a **coil-spring fork** (middle), and one with the Runt installed (right) — so you
watch each settle to its **own compression depth**, alongside a **force-vs-travel graph** that marks all
three on the same load.

## Run it

Just open `index.html` in any modern browser.

> It loads three.js from a CDN via an import map, so the **first load needs an internet connection**.
> After that the browser caches it. To serve locally instead:
>
> ```bash
> cd this-folder
> python3 -m http.server 8080      # then open http://localhost:8080
> # or:  npx --yes serve
> ```

## What you're looking at

- **Cutaways (left):** three half-section forks, each settling to its own depth for the shared load.
  - **Stock air spring** (left): a plain single-chamber air spring — one blue **main (low-pressure)
    chamber**, a main piston on the steel shaft, a simple top cap with a single air valve. No floating
    piston, no second chamber.
  - **Coil fork** (middle): a steel **coil spring** on the same piston/shaft (no air chambers), with a
    preload-adjuster cap. The coil visibly bunches up as it compresses. Its curve is nearly linear but
    **preloaded** — it carries force even at zero travel — with a firmer bottom-out ramp near the end.
  - **With Runt** (right): styled after the real DSD Runt — matte-black anodized body with white
    "THE RUNT / Diaz Suspension Design" laser-etching, the stepped black cap with the signature
    **twin red air valves** (one per chamber), and gold-anodized pistons with white glide-ring / black
    O-ring seals. Inside: the orange **secondary (high-pressure) chamber**, the gold **floating piston**,
    and the blue **main (low-pressure) chamber**. It is called out with the glowing teal "THE RUNT"
    bracket; chamber colors brighten with pressure.

  All three forks see the **same load**. The stock and Runt start identical (and the coil starts a little
  firmer, from its preload). Past engagement the Runt's second stage adds support, lifting its curve above
  stock — so it balances that load at **less** travel and rides **higher**, keeping travel in reserve
  while the stock air spring and coil dive deeper. That growing support gap is the point.
- **Graph (right):** solid teal = **with Runt**, dashed gray = **stock air spring**, solid violet =
  **coil fork** (note it starts *above* zero — that's the coil preload). A horizontal line marks the
  current load; a dot sits on each curve at that load, so their spread is how differently the three forks
  use their travel. The gap between the stock and Runt dots is annotated `N mm more support`, and a teal
  tick marks where the Runt activates.
- **Readouts:** applied load (force), floating-piston state (HELD → ENGAGED), and the resulting **stock
  travel** vs **with-Runt travel**.

### The mechanism it illustrates

The Runt is **set to a specific pressure** (here ~125 psi). Until the load pushes the main air pressure
to that set point, the floating piston can't move, so the Runt does *nothing* and the fork compresses
exactly like a stock air spring — same load, same depth (the two curves sit on top of each other). Once
the load reaches the Runt's set pressure, the floating piston engages the second stage, which **adds
support** — lifting the mid-stroke and stiffening the end-stroke for more mid-stroke support and
bottom-out resistance. Because that curve now sits **above** stock, **for the same load the Runt fork
rides higher and keeps travel in reserve**, while the stock fork dives deeper and bottoms out first.

## Controls

- **Play/Pause** — auto-loops a load-up / unload cycle.
- **Load slider** — dial the applied load by hand; each fork settles to its own depth.
- **Drag the 3D view vertically** — scrub the load (drag up = more load). While paused you can instead
  drag to **rotate** the model and scroll to zoom.
- **Drag across the graph** — scrub to any load level.

## A note on the physics

The air-spring curves come from a polytropic model (`P·Vⁿ = const`). The "stock" and "Runt" cases share
the **same main chamber**, so they are identical until the main pressure reaches the Runt's set pressure;
past that, the Runt adds a smoothly-ramping second-stage support term (the floating piston compressing
the secondary chamber), lifting its curve above stock. The **coil** curve is a preloaded near-linear rate
(`preload + rate·travel`) with a gentle bottom-out ramp — real coils aren't a line through the origin,
the assembly preload gives them force at zero travel. To show **depth** rather than force, the animation
drives all three forks with the same load and inverts each curve (by bisection) to find the travel where
that fork balances the load. It is **illustrative**, tuned to read clearly on screen — not a calibrated
model of any specific fork or pressure setup. All parameters live in the `PHYS`/`COIL` objects near the
top of the inline script in `index.html` if you want to retune them (travel, chamber sizes, main pressure,
Runt set pressure, second-stage `support`, coil `preload`/`rate`/`bump`, polytropic exponent).
