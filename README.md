# PID-Controlled-Buck-Converter
A DC to DC buck converter controlled by a PID system, The inclusions of a rotary allows for adjustment of output voltage, the output voltage is read from a screen.
The buck converter is to take a 12V input and produce an output from 1.5V to 9V depending on the target set by the rotary, this is displayed on the LCD screen, which uses an I2C backpack.
The current sensor is to be implemented via a shunt resistor. The ESP32 cannot handle voltages above 3.3V hence the potential divider will ensure a safe voltage quantity is passed.
A MOSFET driver is used as the ESP32 cannot supply enough voltage normally.
The ESP32 is to be independently powered with USB-C. 

<img width="964" height="610" alt="image" src="https://github.com/user-attachments/assets/ea0364b2-fb53-425b-acd4-b5d6de32e7c7" />

## Components

Will be updated as research continues, below are the components needed based on current knowledge.

- MOSFET
- - Gate driver circuit to drive the mosfet, ESP32 does not provide enough volatge
- - gate resistors


- Schottky diode
- 33µH Inductor
- 100n Input capacitor (stops high frequency noise)
- 33μF Output capacitor (Seen in most basic buck converter diagrams, stores and releases charge in on and off phase)
- Voltage divider resistors (decreases output voltage so the esp32 can safely measure it)
- Shunt resistor ciruit (allows for current to be measured)

- ESP32
- - Voltage regulator for ESP32
- - decoupling capacitors

- - ADC protection (protects ESP32 pins)
- - ADC noise capacitors (stops high frequency noise)

- LCD with I2C backpack
- Rotary

## Current detection

Current detection is done via a shunt resistor and an amplifier, the INA180 is being used due to it being especially designed for current sensing.

<img width="518" height="598" alt="image" src="https://github.com/user-attachments/assets/e43990d6-975d-4c48-ba00-905138ef3bc4" />

C1 is used to decouple the 3.3V power supply from the ESP32.

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

C = Iripple / (8 * Fswitch * ΔVout)

First calculate the actual inductor ripple current

Iripple = D*(Vin - Vout) / (Fswitch * L)

Use Voutmin

D = Vout / Vin

D = 1.5 / 12

D = 0.125

Iripple = D*(Vin - Voutmin) / (Fswitch * L)

Iripple = 0.125*(12-1.5) / 100k*33µH

Iripple = 0.125*(12-1.5) / 100k*33µH

Iripple = 0.3977...A

C = Iripple / (8 * Fswitch * ΔVout)

ΔVout = Voutmin / 100 = 1.5 / 100 = 0.015
1% of 1.5

C = 0.3977 / (8 * 100k * 0.015)

C = 33.14μF

Hence a 33μF capacitor will be used instead.

