# Software Detailed Design (SDD) — SPI Driver

**Project:** ESP32 UART Stopwatch  
**Author:** Adrian Sanchez Garcia  
**Version:** 1.0  
**Date:** [12-08-2025]  

---

## 1. Introduction

### 1.1 Purpose
This document describes the detailed design of the **SPI Driver** module, which provides low-level SPI communication services for the EEPROM Service module.

### 1.2 Scope
The module operates in the **Driver Layer** of the software architecture.  
It interacts with:
- **EEPROM Service** (to perform read/write operations to EEPROM memory)
- **ESP32 SPI hardware peripheral**

---

## 2. References

| Document | Version | Link |
|----------|---------|------|
| Software Requirements Specification (SRS) | 1.0 | [SRS](../../01_Requirements/1_SRS.md) |
| Software Architecture Document (SWAD) | 1.0 | [SWAD](../../01_Requirements/2_SWAD.md) |
| ESP32 Technical Reference Manual — SPI section | — | [ESP32](../../00_Doc/esp32-wroom-32_datasheet_en.pdf) |
| AT24C256 Datasheet | — | [AT24C256](../../00_Doc/AT24C256.pdf) |

---

## 3. Detailed Design

### 3.1 Responsibilities
- Initialize the SPI peripheral for EEPROM communication
- Transmit and receive data over SPI bus
- Provide blocking and non-blocking SPI transfer functions
- Abstract hardware-specific configuration for portability

### 3.2 Interfaces

#### 3.2.1 Provided Interfaces
| API Function | Description | Parameters | Return Value |
|--------------|-------------|------------|--------------|
| `SPI_Init()` | Initializes SPI peripheral with EEPROM configuration | None | int (status code) |
| `SPI_Transmit(uint8_t *data, size_t length)` | Sends data over SPI | data: pointer to buffer, length: number of bytes | int (status code) |
| `SPI_Receive(uint8_t *buffer, size_t length)` | Receives data from SPI | buffer: pointer to buffer, length: number of bytes | int (status code) |
| `SPI_TransmitReceive(uint8_t *txData, uint8_t *rxData, size_t length)` | Full-duplex SPI transfer | txData, rxData: buffers, length: bytes | int (status code) |

#### 3.2.2 Required Interfaces
| API Function | Provided By | Purpose |
|--------------|-------------|---------|
| ESP-IDF SPI Master APIs | ESP-IDF | Low-level SPI hardware access |
| `EEPROM_Service_Read()` | EEPROM Service | Initiate EEPROM read |
| `EEPROM_Service_Write()` | EEPROM Service | Initiate EEPROM write |

---

### 3.3 Data Structures

```c
typedef struct {
    int miso_pin;
    int mosi_pin;
    int sclk_pin;
    int cs_pin;
    uint32_t clock_speed_hz;
    spi_mode_t spi_mode; // SPI_MODE0, SPI_MODE1, etc.
} spi_driver_config_t;
```

---

### 3.4 Processing Flow

#### 3.4.1 SPI Initialization
```mermaid
flowchart TD
    A[System Startup] --> B[SPI_Init()]
    B --> C[Configure SPI bus pins]
    C --> D[Set SPI clock speed and mode]
    D --> E[Attach SPI device]
    E --> F[Ready for communication]
```

#### 3.4.2 SPI Transmission
```mermaid
flowchart TD
    A[EEPROM Service Request] --> B[SPI_Transmit()]
    B --> C[Load TX buffer]
    C --> D[Send data to SPI peripheral]
    D --> E[Wait for completion]
    E --> F[Return status]
```

---

## 4. Dependencies
- ESP-IDF SPI Master API
- EEPROM Service module
- ESP32 SPI hardware peripheral

---

## 5. Design Constraints
- SPI clock speed: 1 MHz (as per EEPROM datasheet)
- SPI mode: MODE 0 (CPOL=0, CPHA=0)
- Chip Select (CS) line must be manually controlled per transaction
- Dedicated SPI bus for EEPROM to avoid conflicts

---

## 6. Testing Considerations
- Unit tests will mock SPI peripheral responses
- Loopback mode testing for verifying transmission/reception
- Integration tests with real EEPROM to validate data integrity
- Boundary tests for maximum transfer size

---

## 7. Traceability to SRS
| Software Requirement ID | Description |
|-------------------------|-------------|
| REQ-SW-COM-002 | Provide SPI-based driver for EEPROM storage operations |
