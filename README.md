# PID-Controlled-Buck-Converter
A buck converter controlled by a PID system, The inclusions of a rotary allows for adjustment of output voltage, the output voltage is read from a screen.
The buck converter is to take a 12V input and produce an output from 1.5V to 9V depending on the target set by the rotary.

## Components

- mosfet
- - Gate driver circuit to drive the mosfet, ESP32 does not provide enough volatge
- - gate resistors


- Schottky diode
- 33µH Inductor
- Input capacitor (stops high frequency noise)
- Output capacitor (Seen in most basic buck converter diagrams, stores and releases charge in on and off phase)
- Voltage divider resistors (decreases output voltage so the esp32 can safely measure it)


- ESP32
- - Voltage regulator for ESP32
- - decoupling capacitors

- - ADC protection (protects ESP32 pins)
- - ADC noise capacitors (stops high frequency noise)

- LCD with I2C backpack
- Rotary

## Calculations

The switching frequency is to be 100kHz
The maximum current is 2A
Output Range 1.5V to 9V

Vin = 12V
Voutmax = 9V
Voutmin = 1.5V
Imax = 2A
Fswitch = 100kHz

Iripple = Imax * 0.4 = 2 * 0.4 = 0.8
D = Vout / Vin
L = (D*(Vin - Vout))/(Fswitch * Iripple)

Use the worst case output voltage, hence use Voutmax
L = (D*(Vin - Voutmax))/(Fswitch * Iripple)
D = Voutmax / Vin
D = 9 / 12 = 0.75
L = (0.75 * (12 - 9))/(100k * 0.8)
L = (2.25)/(80k)
L = 0.000028125H
L = 28.125μH

Hence the standard sized 33µH inductor will be used

