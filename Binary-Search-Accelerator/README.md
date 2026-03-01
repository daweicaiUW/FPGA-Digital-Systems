# Binary Search Hardware Accelerator (FPGA)

This project implements the binary search algorithm entirely in hardware using SystemVerilog on an Intel DE1-SoC FPGA.

Instead of running on a CPU, the search is performed by a finite state machine (FSM) controller connected to synchronous on-chip memory.

---

## Goal

Search a sorted memory array for an 8-bit target value and return whether the value exists and its address.

Inputs

* target value (switches)
* start signal
* reset

Outputs

* FOUND indicator
* NOT FOUND indicator
* address displayed on 7-segment display

---

## Hardware Architecture

The system consists of two major parts:

### Control Path

Finite State Machine (FSM)
States:

* IDLE
* INIT
* SET_ADDR
* WAIT_DATA
* COMPARE
* FOUND
* NOT_FOUND

### Datapath

* address register
* comparator
* low/high registers
* midpoint calculation
* synchronous memory

---

## Important Design Challenge

The on-chip memory is synchronous.

This means:
When an address is applied, the data is NOT immediately available.

The data becomes valid **one clock cycle later**.

Because of this, the FSM must pause for one cycle before comparison.
This is why the WAIT_DATA state exists.

Without this state, the system compares incorrect data.

This behavior is similar to how CPUs must wait for cache or memory access latency.

---

## Verification

Simulation:

* ModelSim waveform verification
* checked state transitions
* verified correct search sequence

Hardware:

* implemented on FPGA board
* tested multiple values
* correct FOUND and NOT FOUND behavior observed

---

### Example Simulation Waveform

Below is a ModelSim waveform captured during a successful search.

The waveform shows:

* FSM state transitions
* memory address changes during binary search
* synchronous memory latency (data valid one clock after address)
* correct assertion of the FOUND signal

![Binary Search Waveform](binary_search_waveform.png)


### Debugging Notes

During testing, the design initially produced incorrect comparisons because the memory output was sampled in the same cycle the address changed.

I fixed this by adding a WAIT state in the FSM so the system waits one clock cycle for the synchronous memory to return valid data before performing the comparison.

After this change, the search correctly identified both found and not-found values on hardware.

---


## What I Learned

* synchronous memory timing
* FSM control vs datapath separation
* hardware debugging
* handling real hardware latency
* designing sequential algorithms in RTL
