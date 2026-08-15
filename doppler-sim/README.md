# The Siren Problem — A Doppler Effect Simulation

**Live demo:** open `index.html` in any browser (no build step, no dependencies except a Google Fonts CDN link).
**Topic:** Doppler Effect for a moving source and stationary observer (Class 11 Physics, Waves).

## Pedagogical Choice

Most students can recite the Doppler formula but misunderstand what actually happens physically. Two specific misconceptions are targeted here:

1. **"The pitch fades smoothly from high to low."** In reality the frequency swings through the source's *true, unshifted* frequency f₀ at one precise instant — when the source is exactly beside the observer, because at that instant the source's velocity has zero component *toward* the observer (radial velocity = 0). Students expect a gradual blend of the "approach" and "recede" pitches; the simulation shows it's actually a value the sound passes *through*, not an average.
2. **"The full speed of the source is what shifts the pitch."** Only the radial component of velocity matters. The simulation exposes this by letting students change their perpendicular distance from the track: the high/low frequency plateaus stay identical, but the *sharpness* of the transition changes — because a closer observer experiences a faster-changing radial component.

The visual architecture resolves both: concentric wavefronts are emitted from the source's actual position at each instant (not artificially bent), so compression ahead and stretching behind emerge naturally from geometry rather than being hard-coded. A synchronized graph plots heard frequency against source position in real time, with the closest-approach point marked explicitly, so the "swing-through-f₀" moment is visible both in the animation and in the numbers simultaneously.

## Technical Notes

- Physics: `f_observed = f₀ · c / (c − v·cosθ)`, where `v·cosθ` is the source's radial velocity component toward the observer, computed from live source/observer geometry each frame. `c = 343 m/s` throughout.
- Wavefront emission rate on screen is a **visual proxy**, not literal Hz (real audio frequencies can't be rendered as individual circles) — this is disclosed directly in the page footer rather than hidden.
- No frameworks; single HTML file with Canvas 2D for both the simulation and the graph, for zero-dependency portability.

## AI Workflow & Edits

I used Claude to scaffold the HTML/CSS/JS in one pass, then reviewed and corrected it against the physics before treating it as done.

**What the AI got wrong initially:**
- The first draft computed the radial velocity component with the wrong sign convention in one branch, which made the frequency curve dip *before* the source reached the observer instead of exactly at x = 0. I caught this by checking the graph's crossing point against `x = 0` directly rather than trusting it visually.
- The initial wavefront emission tied the on-screen ring frequency directly to the real f₀ slider value, which produced either an unreadable solid smear (at 2000 Hz) or almost no rings (at 200 Hz). I asked for a decoupled "visual emission rate" clamped to a legible range, and added the disclosure note so it's clear the rings are illustrative, not literally-scaled audio frequency.
- Graph axis auto-scaling initially clipped the curve when the Mach slider was pushed high (v approaching c made the denominator `c − v` shrink toward zero, producing a very tall spike). I added a clamp on the minimum denominator to keep the curve numerically stable and added a Mach ceiling of 0.9 on the slider to stay in a physically well-behaved, non-degenerate range for a Class 11 audience.

All formula outputs were independently re-derived by hand for the preset scenarios (ambulance, train, bicycle) and checked against the on-screen readouts before finalizing.
