# NexaStack — TCP/IP Stack in Modern C++

A TCP/IP networking stack implemented in modern C++ as an educational project based on the concepts and assignments covered in Stanford's CS144: Introduction to Computer Networking.

The project explores how reliable data transfer and network communication work internally, from managing byte streams and TCP sequence numbers to resolving hardware addresses and forwarding IP datagrams.

**Repository:** [Pragati4566/NexaStack](https://github.com/Pragati4566/NexaStack)

## Overview

NexaStack focuses on implementing the fundamental components of a TCP/IP stack from the ground up. Instead of relying entirely on existing networking abstractions, the project works through the core mechanisms that make reliable communication possible.

The implementation is organized into incremental checkpoints, with each component building on the functionality developed in the previous stages.

The main components include:

- **Reliable Byte Stream:** A capacity-bounded in-memory byte stream with separate reading and writing interfaces.
- **Stream Reassembler:** Reconstructs an ordered byte stream from incoming substrings that may arrive out of order or overlap.
- **TCP Receiver:** Processes incoming TCP segments and manages acknowledgments, sequence numbers, and advertised receive windows.
- **TCP Sender:** Handles transmission, outstanding segments, retransmission timers, and flow-control behavior.
- **Network Interface:** Encapsulates IP datagrams into Ethernet frames and resolves IP-to-MAC address mappings using ARP.
- **IP Router:** Forwards IP datagrams between network interfaces using longest-prefix-match routing.

Together, these components demonstrate how networking protocols coordinate to provide reliable communication across networked systems.

## Features

### 1. Reliable Byte Stream

Implements an in-memory byte stream with a configurable capacity.

Key concepts include:
- Separate `Writer` and `Reader` interfaces.
- Buffer capacity management.
- Tracking bytes written and read.
- Handling end-of-stream and input errors.
- Preventing writes from exceeding available capacity.

### 2. Stream Reassembly

Combines incoming data fragments into a continuous byte stream in the correct order.

Key concepts include:
- Out-of-order data handling.
- Overlapping substring management.
- Reassembly of contiguous bytes.
- Capacity-aware buffering.
- Tracking the next expected stream index.

### 3. TCP Receiver

Processes incoming TCP segments and translates wrapped TCP sequence numbers into absolute stream positions.

Key concepts include:
- 32-bit TCP sequence numbers.
- Sequence-number unwrapping.
- SYN and FIN handling.
- Acknowledgment number generation.
- Receive-window management.
- Integration with the stream reassembler.

### 4. TCP Sender

Manages outgoing TCP segments while respecting the receiver's advertised window and the protocol's reliability requirements.

Key concepts include:
- Window-based transmission.
- Outstanding segment tracking.
- Retransmission timers.
- Exponential retransmission backoff.
- SYN and FIN sequence-space handling.
- Zero-window probing and flow control.

### 5. Network Interface and ARP

Implements the network-interface functionality needed to transmit and receive IP datagrams over Ethernet.

Key concepts include:
- IP datagram encapsulation into Ethernet frames.
- Ethernet frame processing.
- ARP requests and replies.
- IP-to-MAC address resolution.
- Address mapping caches.
- Managing datagrams while address resolution is pending.

### 6. IP Router

Forwards IP datagrams between multiple network interfaces according to routing-table entries.

Key concepts include:
- Routing-table lookup.
- Longest-prefix matching.
- Next-hop selection.
- Time-to-Live (TTL) handling.
- IPv4 header checksum updates where required.

## Architecture

The implementation follows a layered design in which each component has a clearly defined responsibility.

| Component | Responsibility |
|---|---|
| Byte Stream | Stores and exposes a reliable sequence of bytes |
| Stream Reassembler | Restores the original order of received data |
| TCP Receiver | Processes incoming segments and generates acknowledgments |
| TCP Sender | Transmits segments and handles retransmission |
| Network Interface | Connects IP-layer communication with Ethernet |
| IP Router | Routes datagrams between network interfaces |

This separation makes the individual components easier to reason about, implement, and test.

## Repository Structure

```text
NexaStack/
├── src/
│   ├── byte_stream.hh
│   ├── byte_stream.cc
│   ├── reassembler.hh
│   ├── reassembler.cc
│   ├── wrapping_integers.hh
│   ├── wrapping_integers.cc
│   ├── tcp_receiver.hh
│   ├── tcp_receiver.cc
│   ├── tcp_sender.hh
│   ├── tcp_sender.cc
│   ├── network_interface.hh
│   ├── network_interface.cc
│   ├── router.hh
│   └── router.cc
├── apps/
│   └── webget.cc
├── tests/
├── util/
├── CMakeLists.txt
└── README.md
```

*The tree above shows the expected layout of the implementation files. Exact filenames and directories may differ depending on the current repository structure.*

## Technology Stack

- **Language:** C++20
- **Build System:** CMake
- **Testing:** CTest
- **Debugging and Runtime Checks:** AddressSanitizer and UndefinedBehaviorSanitizer, where configured
- **Supporting Components:** C++ standard library and course-provided networking utilities

## Build and Run

### Prerequisites

A Linux development environment with the following tools is recommended:

- A C++ compiler supporting C++20.
- CMake 3.24 or later.
- Make or another supported build tool.
- The dependencies required by the repository's CMake configuration.

### 1. Clone the Repository

```bash
git clone https://github.com/Pragati4566/NexaStack.git
cd NexaStack
```

### 2. Configure the Project

```bash
cmake -S . -B build
```

### 3. Build the Code

```bash
cmake --build build -j
```

### 4. Run the Tests

```bash
ctest --test-dir build --output-on-failure
```

To run the tests associated with an individual checkpoint, use the corresponding targets if they are available in the current build configuration:

```bash
cmake --build build --target check0
cmake --build build --target check1
cmake --build build --target check2
cmake --build build --target check3
cmake --build build --target check4
cmake --build build --target check5
```

Refer to the project's CMake configuration for the targets supported by the current version.

## What This Project Demonstrates

Working on NexaStack provides practical exposure to several important computer networking concepts:

- **Protocol layering:** How individual networking components interact through defined interfaces.
- **Reliable data transfer:** How TCP uses sequence numbers, acknowledgments, and retransmissions to manage delivery.
- **Sequence-number arithmetic:** How wrapped 32-bit sequence numbers relate to absolute stream positions.
- **Flow control:** How advertised receive windows regulate the amount of data sent.
- **Out-of-order delivery:** How received fragments can be reconstructed into the correct byte sequence.
- **Address resolution:** How ARP maps network-layer IP addresses to link-layer MAC addresses.
- **Packet forwarding:** How routers choose the appropriate route using longest-prefix matching.
- **Testing and debugging:** How modular tests and runtime checks help identify implementation errors.

## Learning Outcomes

This project brings together concepts from the transport, network, and link layers in a practical C++ implementation.

It provides an opportunity to understand the mechanisms behind TCP reliability, buffering, retransmission, flow control, address resolution, and IP routing rather than treating networking as a black box.

## Acknowledgment

This educational project is based on concepts and assignment specifications from **[Stanford CS144 — Introduction to Computer Networking](https://cs144.github.io/)**.

The course provides the learning framework and networking specifications used as a reference for implementing the stack's core components.
