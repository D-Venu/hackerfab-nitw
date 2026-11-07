# Spin coater controller

`spin_coater_controller.ino` controls the speed of a 4-wire PC fan using an Arduino Nano or UNO.

## Wiring

- Fan tach wire to D2, with a 10 kΩ pull-up resistor to 5 V (the tach line won't read anything without it)
- Fan PWM wire to D9 (Timer1 is set up to run at 25 kHz)
- Potentiometer middle pin to A0, for setting the target speed
- Push button between D7 and GND (uses the Arduino's internal pull-up)
- Fan +12 V straight from the external supply, not through the Arduino
- Ground shared between the supply, the fan and the Arduino

## How it works

The code measures speed from the time between two tach pulses (the fan sends 2 per rotation). A lookup table from our earlier speed tests gives the starting duty cycle, and a PI loop corrects whatever error is left.

When powering on, plug in USB first and then the 12 V supply. When powering off, do it the other way round.

`phase_d_duty_sweep.ino` steps the fan from 100% down in 5% steps and logs the speed at each step as CSV.
