# Spin coater controller

`spin_coater_controller.ino`: closed-loop RPM control for a 4-wire PC fan, running on an Arduino Nano or UNO.

**Wiring**
- Fan tach → D2, with a 10 kΩ pull-up to 5 V (the tach line is open-collector)
- Fan PWM → D9 (Timer1 set to 25 kHz)
- Potentiometer wiper → A0 (sets the target RPM)
- Push button → D7 to GND (uses `INPUT_PULLUP`)
- Fan +12 V → external supply directly, never through the Arduino
- Ground shared between the supply, the fan and the Arduino

**How it works**
- RPM is measured from the time between tach pulses (2 pulses per revolution), not by counting pulses in a window.
- A feedforward table from the duty sweep supplies most of the duty cycle.
- A PI loop corrects whatever error is left. Kd is set to 0.

Power-up order: USB first, then 12 V. Power-down: 12 V first, then USB.

`phase_d_duty_sweep.ino` steps the duty cycle from 100% down in 5% steps and logs duty against RPM as CSV.
