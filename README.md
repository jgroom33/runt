# How the Runt Works — interactive 3D animation

A single self-contained page that explains the **Diaz Suspension Design "Runt"** dual-chamber air
spring. It shows **two sectioned (cutaway) fork cartridges under the same applied load** — a stock
single-chamber air spring (left) and one with the Runt installed (right) — so you watch each settle to
its **own compression depth**, alongside a **force-vs-travel graph** that marks both on the same load.

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

- **Cutaways (left):** two half-section cartridges that compress in lock-step.
  - **Without Runt** (left): a plain single-chamber air spring — one blue **main (low-pressure) chamber**,
    a main piston on the steel shaft, a simple top cap with a single air valve. No floating piston, no
    second chamber.
  - **With Runt** (right): styled after the real DSD Runt — matte-black anodized body with white
    "THE RUNT / Diaz Suspension Design" laser-etching, the stepped black cap with the signature
    **twin red air valves** (one per chamber), and gold-anodized pistons with white glide-ring / black
    O-ring seals. Inside: the orange **secondary (high-pressure) chamber**, the gold **floating piston**,
    and the blue **main (low-pressure) chamber**. It is called out with the glowing teal "THE RUNT"
    bracket; chamber colors brighten with pressure.

  Both forks see the **same load**, so at low load they sink to the same depth. Past engagement the
  Runt's softer curve means it needs more travel to balance that load — so its main piston (and floating
  piston) sink **deeper** than the stock fork's, which rides high. That growing depth gap is the point.
- **Graph (right):** solid teal = **with Runt**, dashed gray = **stock air spring**, run at the *same*
  main pressure. A horizontal line marks the current load; the hollow gray dot and filled teal dot sit on
  each curve at that load — so the **horizontal gap between them is the extra travel** the Runt uses
  (annotated `+N mm travel`). A teal tick marks where the Runt activates.
- **Readouts:** applied load (force), floating-piston state (HELD → ENGAGED), and the resulting **stock
  travel** vs **with-Runt travel**.

### The mechanism it illustrates

The Runt is **set to a specific pressure** (here ~125 psi). Until the load pushes the main air pressure
to that set point, the floating piston can't move, so the Runt does *nothing* and the fork compresses
exactly like a stock air spring — same load, same depth (the two curves sit on top of each other). Once
the load reaches the Runt's set pressure, the floating piston begins to move and the second chamber
engages — flattening the end-stroke and taming the steep ramp a single air chamber suffers. Because that
curve is softer, **for the same load the Runt fork sinks deeper into its travel** (near-linear,
"coil-like"), while the stock fork rides high on its hard end-stroke ramp.

## Controls

- **Play/Pause** — auto-loops a load-up / unload cycle.
- **Load slider** — dial the applied load by hand; each fork settles to its own depth.
- **Drag the 3D view vertically** — scrub the load (drag up = more load). While paused you can instead
  drag to **rotate** the model and scroll to zoom.
- **Drag across the graph** — scrub to any load level.

## A note on the physics

The curves come from a polytropic air-spring model (`P·Vⁿ = const`). Both the "stock" and "Runt" cases
use the **same main pressure**, so they are mathematically identical until the main pressure reaches the
Runt's set pressure; past that, the floating-piston displacement is solved each step so the two chambers'
pressures stay equalized. To show **depth** rather than force, the animation drives both forks with the
same load and inverts each curve (by bisection) to find the travel where that fork balances the load. It
is **illustrative**, tuned to read clearly on screen — not a calibrated
model of any specific fork or pressure setup. All parameters live in the `PHYS` object near the top of
the inline script in `index.html` if you want to retune them (travel, chamber sizes, main pressure, Runt
set pressure, polytropic exponent).
