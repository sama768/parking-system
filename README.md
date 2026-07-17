# Smart Parking System

---

# 1. Objective

Develop an Arduino-based Smart Parking System that automatically detects vehicles entering and leaving a parking area.

The system should:

- Detect cars using ultrasonic sensors.
- Control an automatic gate using a servo motor.
- Count the number of cars inside the parking.
- Display parking information using an LCD.
- Indicate parking status using LEDs and buzzer.

The system is designed using modular programming to allow parallel development.

---

# 2. System Architecture

                    Vehicle
                       │
                       ▼
              +----------------------+
              |   SmartParking.ino   |
              |   Main Controller    |
              +----------------------+
                       │
          ┌────────────┼────────────┐
          ▼            ▼            ▼
 +----------------+ +----------------+ +----------------+
 | Hardware Layer | | Parking Logic  | | Display Layer  |
 | Sensor + Gate  | | Business Logic | | LCD + Alarm    |
 +----------------+ +----------------+ +----------------+
          │
          ▼
   Arduino Components


Each module is responsible for exactly one layer.

No module should bypass another layer.

Example:

sensor.cpp → lcd.cpp

SmartParking.ino → parking.cpp → sensor.cpp

# 3. System Data

The parking system stores its state using internal variables.

carsInside

availableSpaces

TOTAL_SPACES

Parking capacity is defined in

config.h

# 4. Module Specifications
## Module 1 — Hardware Layer

## Developer: abdo

## Files:
config.h (already done)

sensor.h

sensor.cpp

gate.h

gate.cpp

Purpose

Controls all hardware devices.

This module communicates directly with the Arduino hardware.

No parking logic is allowed inside this module.

## Required Functions: 
1. Sensor Module
   
initSensors()

getEntryDistance()

getExitDistance()

isCarAtEntry()

isCarAtExit()

2. Gate Module:
   
initGate()

openGate()

closeGate()

updateGate()

isGateOpen()

## Responsibilities:
config.h

Define:

Pin numbers

Detection distance

Parking capacity

Servo angles

Timing constants

sensor.cpp:

Initialize ultrasonic sensors.

Measure entry distance.

Measure exit distance.

Detect vehicles.

gate.cpp:

Initialize servo.

Open gate.

Close gate.

Automatically close the gate after the configured delay.

## Module 2 — Parking Logic

## Developer: Sama

## Files
SmartParking.ino

parking.h
parking.cpp

Purpose:

Contains the complete parking management logic.

## Required Functions:
setup()

loop()

initParking()

processEntry()

processExit()

getCarsInside()

getAvailableSpaces()

isParkingFull()

isParkingEmpty()

## Module 3 — Display Layer

## Developer: fatma

## Files:
lcd.h

lcd.cpp

alarm.h

alarm.cpp

## Purpose:

Responsible for displaying parking information and system status.

This module never handles parking logic.

## Required Functions:
## LCD Module:
initLCD()

showWelcome()

updateLCD()

showParkingFull()

showSensorError()

## Alarm Module:
initAlarm()

normalMode()

parkingFullAlarm()

turnOffAlarm()

## Responsibilities:
## LCD
Display welcome screen.

Display available spaces.
Display parked cars.

Display parking full message.

Display sensor error.

## Alarm:
Normal Mode:

Green LED ON

Red LED OFF

Buzzer OFF

Parking Full Mode:

Green LED OFF

Blink Red LED

Blink Buzzer

# 5. Communication Protocol

Hardware Layer exports

initSensors()

isCarAtEntry()

isCarAtExit()

openGate()

closeGate()

Parking Logic imports them.

Display Layer exports

updateLCD()

showParkingFull()

normalMode()

parkingFullAlarm()

Parking Logic imports them.

SmartParking.ino imports

sensor.h

parking.h

gate.h

lcd.h

alarm.h

# 7. Git Workflow

Each member works on a dedicated branch.

main

├── hardware

├── parking

└── display

Development Process

Clone

↓

Checkout Branch

↓

Implement Module

↓

Commit

↓

Push

↓

Pull Request

↓

Merge

9. Integration Order

Step 1

Hardware Layer

↓

Step 2

Display Layer

↓

Step 3

Parking Logic

↓

Step 4

Testing

↓

Step 5

Final Merge

