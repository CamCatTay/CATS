# CATS — C/C++ Ambient Telemetry System

> A distributed field telemetry system built with embedded C on ESP32 and a C++ UDP server, designed to monitor and log environmental data in real time from remote outdoor locations.

---

## Motivation

Commercial hiking apps track routes. CATS tracks _conditions_ — broadcasting live sensor data from a handheld field node to a base station over a custom UDP protocol, logging everything for post-hike analysis. Built from scratch in C and C++ with no telemetry frameworks, because understanding what's underneath is the point.

---

## System Architecture

```
┌─────────────────────────┐         UDP/WiFi           ┌──────────────────────────┐
│      FIELD NODE         │  ──────────────────────►   │      BASE STATION        │
│  ESP32 (C / ESP-IDF)    │                            │   C++ Server (Linux/Win) │
│                         │   Custom Binary Packets    │                          │
│  • BME280 sensor        │   (fixed-width structs)    │  • UDP socket listener   │
│  • Temperature          │                            │  • Packet parser         │
│  • Humidity             │                            │  • Live terminal display │
│  • Altitude (pressure)  │                            │  • CSV data logger       │
│  • Simulated GPS coords │                            │  • Anomaly alerts        │
│  • Packet sequence no.  │                            │  • Session summary       │
└─────────────────────────┘                            └──────────────────────────┘
```

---

## Components

### 1. Field Node — `cats_node/` (C, ESP-IDF)

The ESP32 firmware, written in C using the ESP-IDF framework (no Arduino layer).

**Responsibilities:**

- Initialize and poll BME280 over I2C at a configurable interval (default: 1 Hz)
- Pack sensor readings into a fixed-width binary `cats_packet_t` struct
- Assign a monotonically increasing sequence number to each packet
- Transmit packets via UDP to a configured base station IP and port
- Blink onboard LED on successful transmission; fast-blink on sensor error

**Key source files:**

```
cats_node/
├── main/
│   ├── main.c              # Entry point, task init
│   ├── sensor.c/.h         # BME280 driver wrapper
│   ├── protocol.c/.h       # Packet construction & serialization
│   ├── wifi.c/.h           # WiFi connection management
│   └── transmitter.c/.h    # UDP socket and send loop
└── CMakeLists.txt
```

---

### 2. Base Station Server — `cats_server/` (C++)

A multithreaded C++ server that runs on a laptop in the field (hotspot) or at home.

**Responsibilities:**

- Open a non-blocking UDP socket and listen for incoming `cats_packet_t` packets
- Validate packet checksum and sequence continuity (detect drops)
- Parse and display a live-updating terminal dashboard (ncurses)
- Log all packets to a timestamped `.csv` file for post-session analysis
- Alert (terminal flash + buzzer command) when readings exceed thresholds

**Key source files:**

```
cats_server/
├── src/
│   ├── main.cpp            # Entry point, arg parsing
│   ├── server.cpp/.h       # UDP listener, recv loop
│   ├── parser.cpp/.h       # Packet deserialization & validation
│   ├── dashboard.cpp/.h    # ncurses live display
│   ├── logger.cpp/.h       # CSV logging with timestamps
│   └── alerts.cpp/.h       # Threshold monitoring
├── include/
│   └── cats_protocol.h     # Shared packet definition (mirrored in node)
├── tests/
│   └── test_parser.cpp     # Google Test unit tests
└── CMakeLists.txt
```

---

### 3. Shared Protocol — `cats_protocol.h`

The same header is used (or mirrored) on both sides to define the wire format.

```c
#pragma pack(push, 1)
typedef struct {
    uint8_t  magic[2];       // 0xCA, 0x75 — CATS identifier
    uint32_t sequence;       // Monotonic packet counter
    uint32_t timestamp_ms;   // Uptime in milliseconds (node side)
    float    temperature_c;  // Celsius
    float    humidity_pct;   // Relative humidity %
    float    pressure_hpa;   // Hectopascals
    float    altitude_m;     // Derived from pressure
    float    lat_sim;        // Simulated latitude
    float    lon_sim;        // Simulated longitude
    uint8_t  status_flags;   // Bit flags: sensor OK, GPS lock, battery low
    uint16_t checksum;       // Simple additive checksum
} cats_packet_t;
#pragma pack(pop)
```

Total packet size: **38 bytes** — deliberately compact, fixed-width, no dynamic allocation.

---

## Technology Stack

|Layer|Technology|Why|
|---|---|---|
|Field node firmware|C (ESP-IDF)|Closer to bare-metal than Arduino; more transferable to STM32|
|Transport|UDP over WiFi|Low overhead, fire-and-forget — mirrors real telemetry links|
|Server|C++ (C++17)|OOP for server components, raw sockets for the network layer|
|Terminal UI|ncurses|Lightweight, runs anywhere, no GUI dependency|
|Build system|CMake|Industry standard; required familiarity for defense software roles|
|Testing|Google Test|Unit tests on the parser and checksum logic|
|Logging|CSV (custom writer)|Simple, portable, importable into Python/Excel for post-analysis|

---

## Features

- **Custom binary protocol** — no JSON, no MQTT, no frameworks. Raw structs over UDP.
- **Packet loss detection** — server tracks sequence numbers and reports drop rate
- **Checksum validation** — every packet verified before processing
- **Threshold alerts** — configurable limits (e.g., alert if temp > 38°C or altitude drops unexpectedly)
- **Session summaries** — on server shutdown, prints min/max/avg for all fields and total packets received vs expected
- **Portable server** — compiles and runs on Linux and Windows (WSL)

---

## Skills Demonstrated

This project was built to directly develop and demonstrate skills relevant to embedded and ground systems software engineering:

|Skill|Where demonstrated|
|---|---|
|C socket programming|`transmitter.c`, `server.cpp`|
|Binary protocol design|`cats_protocol.h`, `protocol.c`|
|Memory layout & packing|`#pragma pack`, fixed-width types throughout|
|Multithreading (pthreads / std::thread)|Server recv + display on separate threads|
|Embedded C (ESP-IDF)|Entire `cats_node/` firmware|
|CMake build system|Both `CMakeLists.txt` files|
|Unit testing|`tests/test_parser.cpp`|
|Documentation|Doxygen comments throughout|

---

## Build & Run

### Server (Linux / WSL)

```bash
cd cats_server
mkdir build && cd build
cmake ..
make
./cats_server --port 5005 --log session_01.csv --alert-temp 38.0
```

### Field Node (ESP-IDF)

```bash
cd cats_node
idf.py set-target esp32
idf.py menuconfig   # Set WiFi SSID, password, and server IP
idf.py build flash monitor
```

---

## Future Extensions

- **Encryption** — XOR or AES-128 packet encryption to simulate secure comms links
- **STM32 port** — Rewrite `cats_node` for STM32 Nucleo over UART → ESP32 WiFi bridge
- **Replay tool** — C++ utility to replay a `.csv` log through the server as if live
- **Multi-node support** — Server handles packets from multiple field nodes simultaneously
- **Python post-processor** — Plot altitude/temp profiles on a map from saved CSV

---

## Hardware

|Component|Part|Cost|
|---|---|---|
|Field node MCU|ESP32 DevKit|~$8|
|Environmental sensor|BME280 breakout|~$4|
|Base station|Any laptop (Linux/WSL)|—|

**Total hardware cost: ~$12** (if you don't already own the ESP32)

---

_CATS is a personal project built to develop real embedded and systems programming skills while doing something genuinely useful outdoors. Every design decision — fixed-width packets, raw UDP, no frameworks — was made deliberately to maximize learning and resume relevance._
