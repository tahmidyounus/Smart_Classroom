# Smart Classroom: Automated Occupancy-Sensing and UPS-Backed Power Management System

A lab project from Islamic University of Technology (IUT), submitted for **EEE-4204, Electronics-I Lab (HW)**.

This repository documents the design, Proteus simulation, and breadboard prototyping of a Smart Classroom system that combines automatic occupancy-based load control with an uninterruptible backup power architecture.

## Overview

The system automates classroom electronics (ceiling fans, LED lighting) based on seat occupancy, while ensuring control logic stays powered during main power outages via a battery-backed UPS stage. It also includes a manual touch-override switch for instructor control.

## How It Works

### 1. Occupancy Sensing (6-Seat Network)
Each of 6 seats uses a mechanical microswitch driving a dedicated BC547 transistor stage. All six stages OR-wire onto a shared "occupancy bus" through steering diodes — the bus goes active if *any* seat is occupied, and only goes inactive once every seat is empty.

### 2. Delay-Off Timer (NE555)
When the last occupied seat empties, the falling edge on the occupancy bus triggers an NE555 monostable timer, holding the output relay active for ~5 seconds before cutting the load — giving a grace period after someone stands up.

### 3. Touch-Override Circuit
A single-BJT (BC547) override circuit lets a user manually force the output active by touching two exposed wire ends — the resistance of human skin provides enough bias current to switch the transistor on, overriding sensor logic.

### 4. UPS / Power Management
A relay (RL1) acts as both a power-presence sensor and a changeover switch: with main power present, it connects a 7.4V main rail to an LM7805 regulator (producing the 5V logic rail) while also running 12V loads directly. On power loss, RL1's spring-return switches the regulator input to a 7.4V backup battery — heavy 12V loads drop out immediately, but logic power is preserved. A second switch allows the backup battery to be manually isolated.

### 5. Incomplete Modules (Simulation Only)
Two additional modules were designed in Proteus but not carried into the physical build due to time constraints:
- **IR door sensor** — an LM339 comparator-based break-beam sensor for entry/exit detection
- **Thermistor fire alarm** — an NTC thermistor + comparator circuit that triggers an LED/buzzer alarm above ~40°C

## Hardware Notes

During physical assembly, a defective batch of BC547 transistors caused instability in the original multi-transistor override design. This was resolved by redesigning it into the simpler single-BJT touch circuit described above, which achieved reliable operation.

## Repository Contents

- [`report.pdf`](report.pdf) — full submitted lab report
- [`/schematics`](schematics/) — Proteus circuit diagrams for each module
- [`/hardware`](hardware/) — breadboard build photos

## Tools Used

- **Proteus 8 Professional** — circuit design and simulation
- Physical prototyping on breadboard
