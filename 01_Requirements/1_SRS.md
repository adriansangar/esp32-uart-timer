# Software Requirements Specification (SRS)

**Project:** ESP32 UART Stopwatch  
**Author:** Adrian Sanchez Garcia  
**Version:** 1.0  
**Date:** [10-08-2025]  

---

## 1. Introduction

### 1.1 Purpose
This document specifies all software requirements for the ESP32 UART Stopwatch project.  
It provides a complete description of the expected software behavior, constraints, and interfaces, and links each software requirement to its corresponding system requirement for traceability.

### 1.2 Scope
The software controls the ESP32-based stopwatch functionality, communication, persistent storage, and display updates.  
It operates on FreeRTOS and interacts with hardware peripherals such as UART, SPI EEPROM, and an OLED display.

### 1.3 Definitions, Acronyms, and Abbreviations

| Term  | Description |
|-------|-------------|
| SPI   | Serial Peripheral Interface |
| UART  | Universal Asynchronous Receiver/Transmitter |
| I2C   | Inter-Integrated Circuit |
| EEPROM| Electrically Erasable Programmable Read-Only Memory |
| API   | Application Programming Interface |

### 1.4 References
- [System Requirements Specification (SyRS)](01_Requirements/0_SyRS.md)
- [Software Architecture Document (SWAD)](link-to-software-architecture.md)
- [ESP32 Technical Reference Manual](../00_Doc/esp32-wroom-32_datasheet_en.pdf)

---

## 2. Overall Description

### 2.1 Product Perspective
The software is part of an embedded system with:
- UART command interface
- EEPROM for persistent time storage
- OLED display for time visualization
- FreeRTOS-based task scheduling

### 2.2 Product Functions
- Receive and process UART commands
- Start, pause, reset stopwatch
- Store/retrieve elapsed time
- Periodically update display

### 2.3 Assumptions and Constraints
- SPI bus is dedicated to EEPROM
- Timer operations rely on ESP32 hardware timers
- All inter-module communication is through APIs

---

## 3. Software Requirements

> **Format:**  
> - **ID**: Unique identifier for the requirement  
> - **Description**: Functional or non-functional requirement  
> - **Type**: Functional (F) or Non-Functional (NF)  
> - **Traceability**: Links to System Requirement IDs  

### 3.1 Communication Module Requirements

#### REQ-SW-COM-001 — UART Command Handler
- **Description**: The software shall process incoming UART ASCII commands (`START`, `PAUSE`, `RESET`, `STATUS`) and trigger the corresponding actions.
- **Type**: F
- **Traceability**: REQ-SYS-CON-001, REQ-SYS-BEH-001, REQ-SYS-BEH-002, REQ-SYS-BEH-003, REQ-SYS-BEH-004

#### REQ-SW-COM-002 — EEPROM SPI Driver
- **Description**: The software shall provide an SPI-based EEPROM driver to store and retrieve elapsed time data.
- **Type**: F
- **Traceability**: REQ-SYS-STO-001, REQ-SYS-STO-002
- **Note**: System requirement mentions I2C, but SPI is used per design decision.

---

### 3.2 Timer Module Requirements

#### REQ-SW-TIM-001 — Stopwatch Core
- **Description**: The software shall implement start, pause, reset, and elapsed time retrieval using hardware timers.
- **Type**: F
- **Traceability**: REQ-SYS-BEH-001, REQ-SYS-BEH-002, REQ-SYS-BEH-003, REQ-SYS-BEH-004

#### REQ-SW-TIM-002 — Periodic Display Update
- **Description**: The software shall periodically trigger display refreshes with the current stopwatch time.
- **Type**: F
- **Traceability**: REQ-SYS-DSP-001, REQ-SYS-DSP-002

---

## 4. Traceability Matrix

| System Requirement ID | Software Requirement(s) |
|-----------------------|--------------------------|
| REQ-SYS-CON-001       | REQ-SW-COM-001           |
| REQ-SYS-CON-002       | REQ-SW-TIM-002           |
| REQ-SYS-CON-003       | REQ-SW-COM-002           |
| REQ-SYS-BEH-001       | REQ-SW-COM-001, REQ-SW-TIM-001 |
| REQ-SYS-BEH-002       | REQ-SW-COM-001, REQ-SW-TIM-001 |
| REQ-SYS-BEH-003       | REQ-SW-COM-001, REQ-SW-TIM-001 |
| REQ-SYS-BEH-004       | REQ-SW-COM-001, REQ-SW-TIM-001 |
| REQ-SYS-DSP-001       | REQ-SW-TIM-002           |
| REQ-SYS-DSP-002       | REQ-SW-TIM-002           |
| REQ-SYS-STO-001       | REQ-SW-COM-002           |
| REQ-SYS-STO-002       | REQ-SW-COM-002           |

---

## 5. Approval

| Role              | Name                   | Date       | Signature |
|-------------------|------------------------|------------|-----------|
| Software Engineer | Adrian Sanchez Garcia  | [10-08-2025] |           |
| Project Manager   |                        |            |           |
| QA Engineer       |                        |            |           |
