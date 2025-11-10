# temperature-sensor-stimulation
 This project involves simulating the behavior of a temperature sensor using software tools or microcontroller platforms. It aims to replicate how a real sensor measures and responds to temperature changes in its environment. 
APPLICATION:
 Embedded systems development, IoT system development, Educational and training purposes, System calibration and debugging, Industrial process simulation, Algorithm development and AI modeling,Prototyping and hardware-in-the-loop testing

 
OVERVIEW:-
This project provides a solution to fix the DS18B20 temperature sensor simulation issue in Proteus, where incorrect readings (e.g., -127.00°C) were observed despite the code and circuit working on real hardware.

PROBLEM:-
The DS18B20 sensor in Proteus simulation returned alternating readings of -127.00°C (indicating a communication failure) and 27.00°C (likely the default simulated temperature). This issue is simulation-specific and does not occur on real hardware due to timing mismatches in the OneWire protocol.

SOLUTION:-
The fix involves adjusting the DS18B20 component properties in Proteus to align with the OneWire timing requirements. Follow these steps:
1. Edit DS18B20 Properties:
  i.Right-click the DS18B20 sensor in the Proteus schematic.
  ii.Select "Edit Properties."
  iii.Update the following timing parameters:
      Data Pulse Delay High: 40µs
      Data Pulse Delay Low: 140µs
      Time Reset Low: 400µs
      Time Slot: 120µs
      Conversion Time: 10ms
      Data Write Time: 1ms
  iv.Save the changes.
2.Verify Pull-Up Resistor:
   Ensure the 4.7kΩ pull-up resistor is set to "Analog" mode (right-click, edit properties).
3.Check Arduino Settings:
   Confirm the Arduino clock is set to 16MHz (internal oscillator) in Proteus.
4.Test the Simulation:
    Rerun the simulation to verify stable temperature readings.


CODE:-
   The Arduino sketch used is a simple temperature reading implementation:

#include <OneWire.h>
#include <DallasTemperature.h>

#define ONE_WIRE_BUS 2
OneWire oneWire(ONE_WIRE_BUS);
DallasTemperature sensors(&oneWire);

void setup(void) {
  Serial.begin(9600);
  Serial.println();
  sensors.begin();
}

void loop(void) {
  sensors.requestTemperatures();
  float temp = sensors.getTempCByIndex(0);
  Serial.print("Temperature : ");
  Serial.println(temp);
}


