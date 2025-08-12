# Software Detailed Design (SDD) — I2C Driver

**Project:** ESP32 UART Stopwatch  
**Author:** Adrian Sanchez Garcia  
**Version:** 1.0  
**Date:** [dd-mm-yyyy]  

---

## 1. Introduction

### 1.1 Purpose
This document describes the detailed design of the **I2C Driver** module, which provides low-level I2C communication services for the Display Service module.

### 1.2 Scope
The module operates in the **Driver Layer** of the software architecture.  
It interacts with:
- **Display Service** (to send data and commands to the OLED display)
- **ESP32 I2C hardware peripheral**

---

## 2. References

| Document | Version | Link |
|----------|---------|------|
| Software Requirements Specification (SRS) | 1.0 | link-to-srs.md |
| Software Architecture Document (SWAD) | 1.0 | link-to-swad.md |
| ESP32 Technical Reference Manual — I2C section | — | vendor-link |
| OLED Display Module Datasheet | — | link-to-display-datasheet.pdf |

---

## 3. Detailed Design

### 3.1 Responsibilities
- Initialize the I2C peripheral with the specified configuration
- Transmit commands and data to the OLED display
- Handle I2C errors and retries
- Support blocking and non-blocking transactions

### 3.2 Interfaces

#### 3.2.1 Provided Interfaces
| API Function | Description | Parameters | Return Value |
|--------------|-------------|------------|--------------|
| `I2C_Init(i2c_port, scl_pin, sda_pin, frequency_hz)` | Initializes the I2C peripheral | port ID, SCL pin, SDA pin, frequency in Hz | int (status code) |
| `I2C_Write(i2c_port, device_addr, uint8_t *data, size_t length)` | Sends data to an I2C device | port ID, device address, data buffer, length | int (status code) |
| `I2C_WriteCommand(i2c_port, device_addr, uint8_t command)` | Sends a single command byte to an I2C device | port ID, device address, command byte | int (status code) |
| `I2C_Read(i2c_port, device_addr, uint8_t *buffer, size_t length)` | Reads data from an I2C device | port ID, device address, buffer, length | int (bytes read) |

#### 3.2.2 Required Interfaces
| API Function | Provided By | Purpose |
|--------------|-------------|---------|
| ESP-IDF I2C APIs | ESP-IDF | Low-level I2C hardware access |
| `DisplayService_Update()` | Display Service | Sends updated data to display |

---

### 3.3 Data Structures

```c
typedef struct {
    int i2c_port;
    int scl_pin;
    int sda_pin;
    uint32_t frequency_hz;
} i2c_driver_config_t;
```

## 3.4 Processing Flow
### 3.4.1 I2C Initialization
```mermaid
%% Paste diagram here
```
### 3.4.2 I2C Data Transmission
```mermaid
%% Paste diagram here
```

## 4. Dependencies
ESP-IDF I2C API

Display Service module

ESP32 I2C hardware peripheral

## 5. Design Constraints
I2C frequency: 400 kHz (fast mode)

OLED device address: configurable (default 0x3C)

SCL and SDA pins must support I2C functionality

Pull-up resistors required on SCL and SDA lines

## 6. Testing Considerations
Verify initialization with logic analyzer

Unit test with simulated I2C slave device

Integration test with actual OLED display

Error handling for NACK, arbitration loss, and bus busy

## 7. Traceability to SRS
Software Requirement ID	Description
REQ-SW-TIM-002	Provide I2C interface for updating OLED display