# Software Detailed Design (SDD) — Hardware Timer Driver

**Project:** ESP32 UART Stopwatch  
**Author:** Adrian Sanchez Garcia  
**Version:** 1.0  
**Date:** [dd-mm-yyyy]  

---

## 1. Introduction

### 1.1 Purpose
This document describes the detailed design of the **Hardware Timer Driver** module, which provides low-level access to the ESP32 hardware timers for the Timer Service module.

### 1.2 Scope
The module operates in the **Driver Layer** of the software architecture.  
It interacts with:
- **Timer Service** (to start/stop timers and set interrupts)
- **ESP32 hardware timer peripheral**

---

## 2. References

| Document | Version | Link |
|----------|---------|------|
| Software Requirements Specification (SRS) | 1.0 | link-to-srs.md |
| Software Architecture Document (SWAD) | 1.0 | link-to-swad.md |
| ESP32 Technical Reference Manual — Timer section | — | vendor-link |

---

## 3. Detailed Design

### 3.1 Responsibilities
- Configure hardware timers with specified frequency and mode
- Start, stop, and reset timers
- Register interrupt callbacks for timer events
- Provide accurate time measurement in microseconds or milliseconds

### 3.2 Interfaces

#### 3.2.1 Provided Interfaces
| API Function | Description | Parameters | Return Value |
|--------------|-------------|------------|--------------|
| `HW_Timer_Init(timer_id, frequency_hz, callback)` | Initializes hardware timer with frequency and interrupt callback | timer_id: int, frequency_hz: float, callback: function pointer | int (status code) |
| `HW_Timer_Start(timer_id)` | Starts the specified hardware timer | timer_id: int | int (status code) |
| `HW_Timer_Stop(timer_id)` | Stops the specified hardware timer | timer_id: int | int (status code) |
| `HW_Timer_Reset(timer_id)` | Resets the specified hardware timer count to zero | timer_id: int | int (status code) |
| `HW_Timer_GetCount(timer_id)` | Returns current timer count | timer_id: int | uint64_t (current count) |

#### 3.2.2 Required Interfaces
| API Function | Provided By | Purpose |
|--------------|-------------|---------|
| ESP-IDF Timer APIs | ESP-IDF | Low-level hardware timer access |
| `TimerService_Update()` | Timer Service | Handle periodic update triggered by timer interrupt |

---

### 3.3 Data Structures

```c
typedef struct {
    int timer_id;
    double frequency_hz;
    void (*callback)(void* arg);
    bool is_running;
} hw_timer_config_t;
```
### 3.4 Processing Flow

#### 3.4.1 Timer Initialization and Start
```mermaid
%% Paste diagram here
```

### 3.4.2 Timer Interrupt Handling
```mermaid
%% Paste diagram here
```

## 4. Dependencies

ESP-IDF hardware timer API
Timer Service module
ESP32 hardware timer peripheral

## 5. Design Constraints

Timer resolution: microseconds
Maximum frequency: limited by ESP32 timer hardware (~80 MHz APB clock)
Interrupt latency must be minimal for stopwatch accuracy
Timer should run independently of FreeRTOS tick

## 6. Testing Considerations
Unit tests will simulate timer interrupts using mock callbacks
Frequency accuracy will be validated with oscilloscope measurement
Stress tests with high-frequency interrupts to detect missed events
Integration tests with Timer Service for stopwatch accuracy

## 7. Traceability to SRS
Software Requirement ID	Description
REQ-SW-TIM-001	Provide precise timing for stopwatch core logic
REQ-SW-TIM-002	Trigger periodic events for display updates
