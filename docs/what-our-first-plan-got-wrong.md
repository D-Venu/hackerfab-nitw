# What our first plan got wrong

Our first build plan came from public DIY spin coater builds and HackerFab material.
Then we started buying parts and wiring them up.
Each item below is a place where the plan didn't match reality. For each one we list what we changed and what it would have cost us if we hadn't caught it.

---

## Hardware and specs

**1. We specified a fan that couldn't run the design.**
The plan named a Delta AFB0612VHC. Delta's own datasheet shows it's a **2-wire** fan: power and ground only, with no PWM input and no tachometer output.
Closed-loop speed control is impossible with it.
We used a Delta AFC0612DB (4-wire, 7,200 RPM) instead.
*Lesson: check the wire count on the datasheet. Part numbers in the same family differ by exactly this.*

**2. The wire colours weren't the standard ones.**
Most guides assume the Intel 4-pin colours: black, yellow, green, blue. Our fan is black, red, yellow and blue, with no green wire at all.
Following a generic colour chart could have put +12 V on a signal line and killed the fan's driver chip.
We identified each wire with a multimeter instead:
1. A resistance check with the fan unpowered.
2. Power on black and red only.
3. A DC voltage reading on each unknown wire.
4. A frequency reading through a pull-up resistor.

Result: the tach wire went to D2 and the PWM wire to D9.

**3. The tachometer gives 2 pulses per revolution, not 1.**
This error is dangerous because nothing breaks.
If the code assumes 1 pulse per revolution, every reading comes out exactly 2× too high and still looks believable. A whole coating recipe would then be calibrated against the wrong numbers.
We checked it with a duty sweep. At 100% duty the reading was about 7,200 RPM with `PULSES_PER_REV = 2`, which matches the datasheet.

**4. The Arduino's default PWM is the wrong frequency.**
The Arduino puts out about 490 Hz by default. 4-wire fans expect about 25 kHz.
We set Timer1 by hand to give 25 kHz on pin 9. `analogWrite()` can't do this, and calling it afterwards overwrites the timer setup.

**5. The screws were wrong in two ways.**
- The plan had a countersunk M3 kit. Countersunk heads won't sit flat in the fan's straight holes.
- The longest screw in the kit is 16 mm. The real stack is about 37 mm: 15 mm fan + 2 mm foam + 15 mm plywood + nut.

We switched to M3×40 pan-head screws with washers.

**6. The resin filter doesn't work by hand.**
The plan used 0.45 µm syringe filters. Resin is about 200 times thicker than water, so pushing it through those by hand stalls.
We now batch-filter the resin through 100-mesh paper cones in a steel funnel, into an opaque bottle.

**7. Eight items marked "have" were wrong.**
When we actually checked, eight items listed as already in hand were missing: wafer, UV torch, resin, IPA, gloves, UV glasses, USB microscope and safety glasses.
Also, the "12 V supply" turned out to be a battery pack, not a regulated adapter.
We now track every part as HAVE, ORDERED or OPEN.

## Control

**8. The first controller oscillated, and no tuning could have fixed it.**
The speed spiked above target and then crashed about 1,000 RPM below it. That looked like a gain-tuning problem, but it wasn't. There were two real causes:
- The control maths ran on every pass of the loop, but it assumed it ran only 5 times a second.
- Counting pulses over 200 ms windows meant readings could only change in steps of 150 RPM.

The fix was to update the control once per new reading, and to time the gap between pulses instead of counting them (that gives about 1 RPM resolution). We also added a feedforward table taken from the duty sweep.
The result is the ±0.25% steady-state plot in the [README](../README.md).

## Process (from planning the lithography steps)

**9. The mask can't touch the resin.**
3D-printer resin stays liquid until it's exposed. A mask pressed onto it gets coated, and then glued to the slide everywhere light passes through.
The mask now rests on thin tape "feet" about 50 µm above the film. That gap adds an estimated 5–20 µm of blur, which doesn't matter against our ~500 µm first target.

**10. Spin time matters as much as spin speed.**
Real photoresist contains solvent. As the solvent evaporates, the film's thickness locks in.
3D-printer resin has no solvent, so it keeps thinning for as long as it spins.
Every speed comparison therefore has to hold spin time exactly the same. Otherwise we'd be measuring a different variable than we think.

**11. Thermocol is not a vibration gasket.**
It crushes permanently under load, sheds static-charged particles onto the substrate, and dissolves in acrylate resin.
We use foam gasket tape instead.

**12. Some resin colours rule themselves out.**
- **Black resin:** light doesn't cure deep enough, so the film loses grip on the slide.
- **White resin:** the TiO₂ pigment scatters 405 nm light, which blurs the edges.

We're using grey.
