# PID-Controlled-Buck-Converter
A DC to DC buck converter controlled by a PID system, The inclusions of a rotary allows for adjustment of output voltage, the output voltage is read from a screen.
The buck converter is to take a 12V input and produce an output from 1.5V to 9V depending on the target set by the rotary, this is displayed on the LCD screen, which uses an I2C backpack.
The current sensor is to be implemented via a shunt resistor. The ESP32 cannot handle voltages above 3.3V hence the potential divider will ensure a safe voltage quantity is passed.
A MOSFET driver is used as the ESP32 cannot supply enough voltage normally.
The ESP32 is to be independently powered with USB-C. 

<img width="953" height="606" alt="image" src="https://github.com/user-attachments/assets/0ff63263-d62e-45c1-b514-b9a0f6ad5688" />

## Components

Will be updated as research continues, below are the components needed based on current knowledge.

- MOSFET
- - Gate driver circuit to drive the mosfet, ESP32 does not provide enough volatge
- - gate resistors


- Schottky diode
- 33µH Inductor
- 100n Input capacitor (stops high frequency noise)
- 0.68μF Output capacitor (Seen in most basic buck converter diagrams, stores and releases charge in on and off phase)
- Voltage divider resistors (decreases output voltage so the esp32 can safely measure it)
- Shunt resistor ciruit (allows for current to be measured)

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

### Values

Vin = 12V

Voutmax = 9V

Voutmin = 1.5V

Imax = 2A

Fswitch = 100kHz

Iripple = Imax * 0.4 = 2 * 0.4 = 0.8

D = Vout / Vin

### Inductor Calculations

L = (D*(Vin - Vout))/(Fswitch * Iripple)

Use the worst case output voltage, hence use Voutmax

L = (D*(Vin - Voutmax))/(Fswitch * Iripple)

D = Voutmax / Vin

D = 9 / 12 = 0.75

L = (0.75 * (12 - 9))/(100k * 0.8)

L = (2.25)/(80k)

L = 0.000028125H

L = 28.125μH

Hence a 33µH inductor will be used.

### Output Capacitor Calculations

C = Iripple / (8 * Fswitch * Vout)

Use worst case senario for Vout, hence use Voutmin

C = Iripple / (8 * Fswitch * Voutmin)

C = 0.8 / (8 * 100k * 1.5)

C = 0.8 / 1200k

C = 0.000000666666...

C = 0.667μF

Hence a 0.68μF capacitor will be used instead.

