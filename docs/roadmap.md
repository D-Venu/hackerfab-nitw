# Roadmap to April 2027

Each step has to work before the next one starts. "Working" means a measured number, not just "assembled".

| When | Goal | How we'll know it's done |
|---|---|---|
| **Dec 2026** (winter break) | Mechanical mounting: plywood baseplate, foam gasket, blades off, 50 mm FR4 chuck balanced and centred on the true axis | The bare-hub sweep and the with-chuck sweep are both logged. The chuck spins without wobble. |
| **Jan 2027** | Final controller tune with the chuck fitted. Fix the spin-up overshoot. Add a two-stage spin profile (spread, then thin-down) and a one-line summary after every run. | Mean within ±1% of target, ripple under ±2%. Each run prints a logged summary line. |
| **Jan–Feb** | Prove the coater with a cheap stand-in liquid (glycerin) before buying resin. | Visually even films with no centring asymmetry |
| **Feb** | First resin coats on half-slides. Measure thickness by weighing the slide before and after coating, at 2 speeds, with spin time held fixed. | Two thickness numbers. The ratio between them should roughly follow 1/√(RPM). |
| **Mar** | Half-slide mask v2. Exposures with a 405 nm torch. Read the dose ladder and the resolution ladder on glass. | A correct exposure time for this resin and lamp, and the smallest line/space that resolves |
| **Apr** | Repeat the run on a cleaved silicon fragment. Write up the recipe. | A developed pattern on silicon, plus a documented recipe we can take to IIT Bombay to ask for real photoresist |

Exposure doses measured on glass won't carry over directly to silicon, because silicon reflects 405 nm light back into the resin. That's why glass comes first.
