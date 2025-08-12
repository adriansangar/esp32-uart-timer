# Software Detailed Design (SDD) — UART Driver

**Project:** ESP32 UART Stopwatch  
**Author:** Adrian Sanchez Garcia  
**Version:** 1.0  
**Date:** [dd-mm-yyyy]  

---

## 1. Introduction

### 1.1 Purpose
This document describes the detailed design of the **UART Driver** module, which provides low-level UART communication services for the UART Command Handler module.

### 1.2 Scope
The module operates in the **Driver Layer** of the software architecture.  
It interacts with:
- **UART Command Handler** (to receive and transmit data)
- **ESP32 UART hardware peripheral**

---

## 2. References

| Document | Version | Link |
|----------|---------|------|
| Software Requirements Specification (SRS) | 1.0 | link-to-srs.md |
| Software Architecture Document (SWAD) | 1.0 | link-to-swad.md |
| ESP32 Technical Reference Manual — UART section | — | vendor-link |

---

## 3. Detailed Design

### 3.1 Responsibilities
- Initialize UART peripheral with the specified configuration
- Transmit and receive data over UART
- Handle UART interrupts and callbacks
- Provide blocking and non-blocking data transfer functions

### 3.2 Interfaces

#### 3.2.1 Provided Interfaces
| API Function | Description | Parameters | Return Value |
|--------------|-------------|------------|--------------|
| `UART_Init(uart_port, baud_rate, data_bits, parity, stop_bits)` | Configures and initializes the UART peripheral | UART port ID, baud rate, data bits, parity, stop bits | int (status code) |
| `UART_Write(uart_port, uint8_t *data, size_t length)` | Sends data via UART | port ID, buffer, length | int (status code) |
| `UART_Read(uart_port, uint8_t *buffer, size_t length)` | Reads data from UART | port ID, buffer, length | int (bytes read) |
| `UART_SetCallback(uart_port, callback)` | Registers callback for received data | port ID, callback pointer | int (status code) |

#### 3.2.2 Required Interfaces
| API Function | Provided By | Purpose |
|--------------|-------------|---------|
| ESP-IDF UART APIs | ESP-IDF | Low-level hardware UART access |
| `UART_CMD_Process()` | UART Command Handler | Parses and processes received commands |

---

### 3.3 Data Structures

```c
typedef struct {
    int uart_port;
    int baud_rate;
    int data_bits;
    int parity;    // 0=None, 1=Odd, 2=Even
    int stop_bits;
    void (*rx_callback)(uint8_t *data, size_t length);
} uart_driver_config_t;
```

## 3.4 Processing Flow
### 3.4.1 UART Initialization
```mermaid
%% Paste diagram here
```

### 3.4.2 UART Data Reception
```mermaid
%% Paste diagram here
```
## 4. Dependencies
ESP-IDF UART API

UART Command Handler module

ESP32 UART hardware peripheral

## 5. Design Constraints
Baud rate: 115200 bps (default for this project)

Data format: 8 data bits, no parity, 1 stop bit (8N1)

RX and TX buffers size: 256 bytes

Uses UART interrupts for non-blocking operation

## 6. Testing Considerations
Loopback mode for testing TX/RX functionality

Simulated UART input for unit testing

Stress testing with high data rates

Error handling for framing, parity, and overrun errors

## 7. Traceability to SRS
Software Requirement ID	Description
REQ-SW-COM-001	Provide UART interface for command reception and response
