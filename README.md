# PID-Controlled-Buck-Converter
A buck converter controlled by a PID system, The inclusions of a rotary allows for adjustment of output voltage, the output voltage is read from a screen.
The buck converter is to take a 12V input and produce an output from 1.5V to 9V depending on the target set by the rotary.

## Components

mosfet
Gate driver circuit to drive the mosfet, ESP32 does not provide enough volatge
gate resistors

ADC protection
ADC noise capacitors


Schottky diode
Inductor
Input capacitors
Output capacitors
Voltage divider resistors


ESP32
Voltage regulator for ESP32
decoupling capacitors

LCD with I2C backpack
Rotary

## Calculations

The switching frequency is to be 100kHz
The maximum current is 2A
Output Range 1.5V to 9V

