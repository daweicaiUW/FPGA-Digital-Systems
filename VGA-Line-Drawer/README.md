# VGA Line Drawing Engine (FPGA Graphics)

This project implements a hardware graphics system that draws lines directly on a VGA display using an FPGA.

The system generates pixel coordinates in hardware and writes them into a framebuffer, allowing the FPGA to render graphics without a CPU.

---

## Project Overview

The design uses Bresenham’s line drawing algorithm implemented as a finite state machine (FSM).

Each clock cycle:

* a new pixel coordinate is generated
* the pixel is written into video memory
* the VGA controller outputs the pixel to the monitor

The display is first cleared and then the line is drawn pixel-by-pixel.

---

## System Architecture

The hardware system consists of several modules:

**Line Generator**

* Implements Bresenham's algorithm
* Generates one pixel coordinate per clock cycle

**Framebuffer**

* Stores pixel data for the VGA display
* Allows pixel-by-pixel updates

**VGA Controller**

* Generates horizontal and vertical sync signals
* Streams pixel data to the monitor

**Control FSM**

States:

* CLEAR
* DRAW
* HOLD

The FSM first clears the screen, then draws the line, and finally holds the image on the display.

---

## Key Concepts Demonstrated

* real-time graphics rendering
* framebuffer architecture
* VGA timing generation
* FSM controlled hardware pipeline
* FPGA hardware verification

---

## Simulation Verification

The system was verified using ModelSim simulation to confirm:

* correct pixel coordinate generation
* valid and done signals from the line generator
* correct framebuffer write behavior

---

## Hardware Demonstration

The design was implemented on an Intel DE1-SoC FPGA board connected to a VGA monitor.

The screen is first cleared and then a line appears at the expected coordinates.

A stable image remains on the screen after drawing is completed.
