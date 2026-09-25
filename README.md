# PID-Controlled-Buck-Converter

This project is currently in development.

| Tasks  | Status | 
| ------------- | ------------- |
| Component Calculation & Selection  | Complete  |
| KiCad Schematics   | Complete  | 
| KiCad PCB   | Incomplete - Estimated completion in late October |
| MODFET & driver Test   | Incomplete - Estimated completion in early October |
| PID code for PID tuning   | Incomplete |


A DC to DC buck converter controlled by a PID system, The inclusions of a rotary allows for adjustment of output voltage, the output voltage is read from a screen.
The buck converter is to take a 12V input and produce an output from 1.5V to 9V depending on the target set by the rotary, this is displayed on the LCD screen, which uses an I2C backpack.
The current sensor is to be implemented via a shunt resistor. The ESP32 cannot handle voltages above 3.3V hence the potential divider will ensure a safe voltage quantity is passed.
A MOSFET driver is used as the ESP32 cannot supply enough voltage normally.
The ESP32 is to be independently powered with USB-C. 

<img width="964" height="610" alt="image" src="https://github.com/user-attachments/assets/ea0364b2-fb53-425b-acd4-b5d6de32e7c7" />

## Circuit Diagram

<img width="976" height="665" alt="image" src="https://github.com/user-attachments/assets/2b41ab7f-ab00-4c71-8162-08f04bf8db34" />

J1 and J2 are used to hold the ESP32, this allows the microcontroller to be remocewd when not in use.

The decoupling capacitor C2 and C5 mitigates high and low frequency noise.

The pull up resistor R7 keeps the MOSFET off during boot up.

The zener diode clamps the voltage to a safe limit preventing damage to the ESP32.

The network formed by R5,R6 and C4 acts as a low pass filter to protect the INA180 from high frequency noise.

R1 prevents ringing.


## Components

Will be updated as research continues, below are the components needed based on current knowledge.

- IRF4905 MOSFET
- - TC4427 MOSFET driver
- - 10Ω gate resistor


- 1N5822 Schottky diode
- 33µH Inductor
- 100n Input capacitor
- 100μ Input capacitor
- 47μF Output capacitor (Seen in most basic buck converter diagrams, stores and releases charge in on and off phase)
  
- 10kΩ & 27kΩ Voltage divider resistors
- 3.3V zener diode
- 100n capacitor
  
- INA180A1 current sensing amplifier, gain 20
- 50mΩ shunt resistor
- 2x 10Ω resistors
- 100nF capacitor

- ESP32
- - Voltage regulator for ESP32
- - ADC protection (protects ESP32 pins)
- - ADC noise capacitors (stops high frequency noise)
 
- Decoupling capacitors

- 2x16 LCD with I2C backpack
- KY-040 Rotary encoder

## Testing

This test is expected to take place in early October 2026.

The IRF4905 is to be tested alongside the TC4427 using an oscilloscope, function generator and PSU.

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

To account for capacitor intolerance and other real world factors we will use 47μF capacitor which is well above the minimum of 33.14μF.
