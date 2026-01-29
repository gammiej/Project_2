# ESC 218 – Project 2: Automatic Headlight System

## Team Members
- James Gammie
- Nada Aloussi

## System Description
This project implements a car safety and headlight control system using an ESP32-S3 microcontroller.  
The ignition subsystem prevents the engine from starting unless both the driver and passenger are seated and wearing seatbelts. If a start attempt is made while conditions are not met, the system provides clear error messages and an audible buzzer alert. Once started, the engine remains running even if seatbelts are removed or seats are vacated, and the ignition button can be pressed again to turn the engine off.

The headlight subsystem allows the user to select OFF, ON, or AUTO mode using a potentiometer. In AUTO mode, a light sensor controls the headlights with timed delays based on ambient light conditions.


---

## Testing Results Summary

### Ignition Subsystem

| Specification | Test Process | Results |
|--------------|-------------|---------|
| Green LED turns ON only when driver seat, passenger seat, driver belt, and passenger belt are all active | All combinations of DS, DB, PS, PB were tested (all pressed, one missing at a time, none pressed) | Passed. Green LED only turned ON when all four conditions were satisfied. |
| Ignition is inhibited when safety conditions are not met | With one or more safety inputs inactive, ignition button was pressed | Passed. Engine did not start, buzzer sounded, and correct error messages were printed. |
| System allows additional start attempts after an inhibited start | Attempted ignition while unsafe, then corrected conditions and retried | Passed. Engine successfully started after conditions were corrected. |
| Engine starts when ignition is pressed and system is ready | All safety inputs active, ignition button pressed | Passed. Red LED turned ON, green LED turned OFF, “Engine Started” message printed. |
| Engine remains running if seatbelts are removed or seats are vacated after start | Engine started, then DS/DB/PS/PB released | Passed. Engine continued running as specified. |
| Pressing ignition button while engine is running turns engine OFF | Engine started, ignition button pressed again | Passed. Engine shut off and red LED turned OFF. |

---

### Headlight Subsystem

| Specification | Test Process | Results |
|--------------|-------------|---------|
| Headlights remain OFF when engine is OFF | Engine off, potentiometer moved through OFF, AUTO, ON | Passed. Headlights stayed OFF regardless of selector position. |
| ON mode turns headlights ON immediately | Engine on, potentiometer set to ON region | Passed. Low beams turned ON immediately. |
| OFF mode turns headlights OFF immediately | Engine on, potentiometer set to OFF region | Passed. Low beams turned OFF immediately. |
| AUTO mode turns headlights ON after 1 second in low light | Engine on, AUTO selected, light sensor covered | Passed. Headlights turned ON after ~1 second delay. |
| AUTO mode turns headlights OFF after 2 seconds in bright light | Engine on, AUTO selected, light sensor illuminated | Passed. Headlights turned OFF after ~2 second delay. |
| AUTO mode holds previous state between thresholds | Light level adjusted between daylight and dusk thresholds | Passed. Headlights maintained previous ON/OFF state. |

---

## Notes
- All buttons are wired as **active-low inputs** using internal pull-up resistors.
- The light sensor (LDR) and potentiometer are read using ADC channels.
- Timing delays were implemented using FreeRTOS task delays.
