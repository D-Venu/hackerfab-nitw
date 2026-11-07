# HackerFab NITW

A hostel-room photolithography setup, built by second-year students at NIT Warangal.
We're working through the [HackerFab](https://docs.hackerfab.org/home) path, with IIT Bombay's HackerFab community as our backstop.

**Iteration one:** build a spin coater from a PC fan, then run one full lithography cycle (coat, expose, develop) on glass, then on a silicon fragment.
Oxide growth, etching, doping and metallisation come later and aren't in scope yet.

---

## Where we actually are (Nov 2026)

| Phase | Status |
|---|---|
| A: Procurement | Mostly done. Fan, Nano, 12 V supply, control electronics and slides are in hand. Resin, IPA and the UV torch are deliberately on hold (see roadmap). |
| B: Fan bench test | ✅ Done |
| C: Control circuit | ✅ Done on breadboard. Perfboard is next. |
| D: Closed-loop speed control | 🟡 **Working on the bare fan.** The final tune has to wait for the chuck. |
| E: Mechanical mounting (baseplate, chuck disc) | ⏭ Next. Parts are in hand. |
| F–H: Dry run, substrate prep, first coat | Not started |
| I–K: Mask, exposure, development | Test mask v1 is designed ([`mask/`](mask/)). Exposures need a spun film first. |

**Done criteria for iteration one:** 0 of 4 met so far. Criterion #1 (a logged, repeatable spin profile) is the closest.

We'd rather you see exactly where we are than a polished overclaim.

---

## The result we have: closed-loop RPM control

An Arduino reads the fan's tachometer and adjusts its 25 kHz PWM drive to hold a target speed.

**Steady state at a 4000 RPM target.** The readings stay between about 3,990 and 4,008 RPM, which is within ±0.25%.
Our pass bar was ±1% mean and ±2% ripple.

![steady state](media/plot-steady-4000rpm.png)

**Spin-up from rest.** It overshoots to about 5,100 RPM (roughly +27%) and takes several seconds to settle.
This is the next thing to fix. A spin coater's spread stage needs a clean ramp, not a spike.

![spin-up](media/plot-spinup-overshoot.png)

![bench](media/bench-nano-breadboard-fan.png)

Recorded 6 Nov 2026, with the fan blades still on and no chuck fitted. Adding the chuck changes the load, so the final tune happens after Phase E.

Videos:
- [`closed-loop-4000rpm-screen.mp4`](media/closed-loop-4000rpm-screen.mp4): the controller code and the live plot, from spin-up to steady state
- [`bench-setup-running.mp4`](media/bench-setup-running.mp4): the bench while it runs
- [`bench-start-stop.mp4`](media/bench-start-stop.mp4): button start and stop

---

## What's in here

| Path | What it is |
|---|---|
| [`docs/what-our-first-plan-got-wrong.md`](docs/what-our-first-plan-got-wrong.md) | Every place our original plan collided with reality, and what we changed. **Start here.** |
| [`docs/roadmap.md`](docs/roadmap.md) | Plan through April 2027 |
| `docs/build-guide-rev2.pdf` | Our full team build guide: phases, precautions, materials audit, test mask design |
| [`firmware/`](firmware/) | Spin coater controller (Arduino Nano/UNO) |
| [`mask/HF-NITW_mask_v1.pdf`](mask/HF-NITW_mask_v1.pdf) | Test mask with a resolution ladder (50–1500 µm line/space) and an 8-step dose ladder |
| [`media/`](media/) | Plots, photos, videos |

---

## Team

- **Venu Gopal Dash**: coordination, build guide, procurement and spec audit
- **Arjan Singh Aujla**: controller firmware
- **[name]**: lithography prep (mask, exposure jig, development station)

EEE, NIT Warangal, 2nd year.
