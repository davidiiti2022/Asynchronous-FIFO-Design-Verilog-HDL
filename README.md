# Asynchronous FIFO Design using Verilog HDL

## Overview

An **Asynchronous FIFO (First-In, First-Out)** is a memory buffer used to transfer data between two independent clock domains.

This project implements a **parameterized asynchronous FIFO in Verilog HDL** using:

- Independent read and write clock domains
- Binary and Gray-code pointers
- 2-FF clock-domain synchronizers
- Full and empty flag generation
- FIFO memory control
- Verilog testbench for functional verification

The main focus of this project is **Clock Domain Crossing (CDC)** and reliable data transfer between asynchronous clock domains.

---

## Key Features

- Parameterized data width and FIFO depth
- Independent `wclk` and `rclk`
- Binary read/write pointer generation
- Binary-to-Gray code conversion
- 2-FF pointer synchronization
- Full and empty condition detection
- Dual-port FIFO memory
- Protection against write when FIFO is full
- Protection against read when FIFO is empty
- Functional verification using Verilog testbench

---

## Architecture

```text
                     ASYNCHRONOUS FIFO

     WRITE DOMAIN                              READ DOMAIN
     ────────────                              ───────────

       wclk                                      rclk
        │                                          │
        ▼                                          ▼
 ┌───────────────┐                         ┌───────────────┐
 │ Write Pointer │                         │  Read Pointer │
 │    Logic      │                         │     Logic     │
 └───────┬───────┘                         └───────┬───────┘
         │                                         │
         ▼                                         ▼
    Binary → Gray                              Binary → Gray
         │                                         │
         ▼                                         ▼
 ┌───────────────┐                         ┌───────────────┐
 │    2-FF Sync  │                         │    2-FF Sync  │
 └───────┬───────┘                         └───────┬───────┘
         │                                         │
         │              Clock Domain               │
         │              Crossing (CDC)             │
         └───────────────────┬─────────────────────┘
                             │
                             ▼
                  ┌────────────────────┐
                  │    FIFO MEMORY     │
                  │                    │
                  │  Dual-Port Buffer  │
                  └─────────┬──────────┘
                            │
                     Data Read / Write


## Working Principle
Write Operation

The write side operates using the wclk clock.

Write Request
      │
      ▼
Check FIFO Full
      │
      ├── Full ──► Block Write
      │
      ▼
Write Data to Memory
      │
      ▼
Increment Write Pointer
      │
      ▼
Convert Binary Pointer
      │
      ▼
Gray-Code Pointer
      │
      ▼
Synchronize to Read Domain

When a valid write request is received and the FIFO is not full, the input data is stored in the memory location pointed to by the write address.

The write pointer is then incremented.
