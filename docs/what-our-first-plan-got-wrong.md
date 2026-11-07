# What our first plan got wrong

We wrote our first build plan using public DIY spin coater builds and HackerFab's material. Once we started buying parts and wiring things up, a lot of it turned out to be wrong. We're keeping track of each one here, along with what we changed, so that anyone building something similar doesn't have to run into the same problems.

## Parts and specs

### 1. The fan we picked couldn't be controlled

Our plan said to use a Delta AFB0612VHC. When we checked Delta's datasheet, we found it's a 2-wire fan: it only has power and ground. There's no wire to control its speed and no wire that reports how fast it's spinning, so it could never have worked for closed-loop control. We used a Delta AFC0612DB instead, which is a 4-wire fan rated at 7,200 RPM. Fans in the same family can look almost identical and still have different numbers of wires, so we now check the wire count on the datasheet before ordering.

### 2. The wire colours didn't match the usual chart

Most guides online assume the standard Intel colours for 4-pin fans (black, yellow, green, blue). Our fan has black, red, yellow and blue, with no green wire. If we had gone by a generic chart, we might have connected 12 V to a signal wire and damaged the fan's internal driver chip. So we worked out each wire with a multimeter instead:
1. A resistance check with the fan unpowered.
2. Powering only black and red.
3. Measuring the voltage on the two remaining wires.
4. Measuring the frequency through a pull-up resistor.

That gave us the tach wire on D2 and the PWM wire on D9.

### 3. The tachometer sends 2 pulses per rotation, not 1

If the code assumes 1 pulse per rotation, every speed reading comes out exactly double. Nothing about it looks wrong, so it would be very easy to miss. We checked it by running the fan at full power. With the code set to 2 pulses per rotation, the reading was about 7,200 RPM, which matches the datasheet.

### 4. The Arduino's default PWM frequency is too low

By default, the Arduino's PWM runs at about 490 Hz. 4-wire fans expect around 25 kHz. We set up Timer1 manually to get 25 kHz on pin 9. One thing to watch out for: using `analogWrite()` on that pin afterwards resets the timer settings.

### 5. The screws were wrong in two ways

Our plan listed a countersunk M3 screw kit. Countersunk heads don't sit flat in the fan's straight holes. On top of that, the longest screw in the kit was 16 mm. Once we added up the real stack (15 mm fan, 2 mm foam, 15 mm plywood and a nut), we needed about 37 mm. We switched to M3×40 pan-head screws with washers.

### 6. The resin filter didn't work by hand

The plan said to push the resin through 0.45 µm syringe filters. Resin is much too thick for that, roughly 200 times thicker than water, and it just won't go through by hand. We now filter it in batches through 100-mesh paper cones sitting in a steel funnel, into an opaque bottle.

### 7. Eight things we thought we had, we didn't

When we went through everything physically, eight items marked as "already have" were actually missing: the wafer, UV torch, resin, IPA, gloves, UV glasses, USB microscope and safety glasses. The "12 V supply" also turned out to be a battery pack, not a regulated adapter. Since then we mark every item as HAVE, ORDERED or OPEN, and only mark it HAVE once someone has actually checked.

## Control

### 8. The first controller kept overshooting, and tuning wasn't the problem

Our first controller kept overshooting the target and then dropping about 1,000 RPM below it. It looked like the control settings were off, but changing them wouldn't have fixed it. There were two actual problems:
- The control calculation was written assuming it ran five times a second, but it was running on every pass of the loop.
- We were counting pulses over 200 ms windows, which meant the speed reading could only change in steps of 150 RPM.

Arjan changed the code so the control only updates when there's a new reading, and so the speed is measured from the time between two pulses. That gives about 1 RPM resolution. He also added a lookup table from our earlier speed tests, so the controller starts close to the right value. The steady-state graph in the [README](../README.md) is from this version.

## Process (things we found while planning the lithography steps)

### 9. The mask shouldn't touch the resin

3D-printer resin stays liquid until it's exposed to UV. If you press the mask onto it, the mask gets coated in resin and ends up stuck to the slide wherever the light gets through. So the mask now sits on thin strips of tape, about 50 µm above the film. That gap blurs the edges by an estimated 5 to 20 µm, which is fine for now since our first target features are around 500 µm.

### 10. Spin time matters as much as spin speed

Normal photoresist has solvent in it. As the solvent evaporates during spinning, the film stops getting thinner. 3D-printer resin has no solvent, so it just keeps thinning as long as it spins. This means that when we compare different speeds, we have to keep the spin time exactly the same. Otherwise we can't tell whether a difference in thickness came from the speed or the time.

### 11. Thermocol doesn't work for reducing vibration

We'd thought about using thermocol between the fan and the baseplate. It turned out to be a bad choice for three reasons:
- It gets crushed permanently under the screws.
- It sheds small particles that stick to everything because of static.
- It dissolves in the kind of resin we're using.

We're using foam gasket tape instead.

### 12. Black and white resin don't work for this

Black resin doesn't let the UV light go deep enough, so the film doesn't stick properly to the slide. White resin has titanium dioxide in it, which scatters the 405 nm light and blurs the edges of the pattern. That's why we picked grey.
