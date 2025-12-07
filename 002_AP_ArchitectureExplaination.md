# Explain the AP Architecture

Adaptive AUTOSAR Platform (AP) is a software standard designed to support high-performance ECUs and complex use cases such as highly automated driving. The platform provides high-performance computing mechanisms and flexible communication to meet the stringent safety, integrity, and security requirements of modern automotive systems.

The architecture of Adaptive AUTOSAR is based on high-level Functional Clusters (FCs) running on a POSIX-based operating system and providing APIs through the AUTOSAR Runtime for Adaptive Applications (ARA).

Below is a summary of the high-level building blocks and their purposes based on the Requirement Specifications (RS):

---

## 1. Core Functional Clusters (Foundation FCs)

These FCs provide the fundamental platform services:

### • Communication Management (ara::com)

* **Purpose:** Provides protocol- and network-independent communication services between applications. It handles all communication levels: IntraProcess, InterProcess, and InterMachine.
* **Key functions:** Offers standardized APIs, supports Service Discovery to find available services at runtime, and implements protocols such as SOME/IP with E2E protection. It handles data transmission via events, triggers, methods, and fields.

### • Execution Management (ara::exec)

* **Purpose:** Responsible for lifecycle management of the platform and applications, including startup and shutdown.
* **Key functions:** Creates an OS process for each Modelled Process and applies process attributes like scheduling policy and access rights. Supports authenticated startup to ensure integrity and authenticity of executables and Manifests. Also enforces resource limits such as CPU time and memory.

### • Cryptography (ara::crypto)

* **Purpose:** Provides standardized interfaces for cryptographic operations, key management, and certificate handling.
* **Key functions:** Hides symmetric key values from users (keys are referenced by identifiers only), supports key generation/derivation, and integrates with PKI for certificate verification.

### • Platform Health Management (ara::phm)

* **Purpose:** Monitors software processes and platform execution regarding timing and logical constraints.
* **Key functions:** Performs Alive Supervision, Deadline Supervision, and Logical Supervision using reported Checkpoints. When errors are detected, it informs State Management (SM) to coordinate recovery or initiates Watchdog resets if SM or EM fails.

### • Operating System Interface (OSI)

* **Purpose:** Defines the execution context and programming interface required for Adaptive Applications (AAs).
* **Key functions:** AAs must comply with the POSIX PSE51 interface. The OS must provide multithreading capabilities and resource budgeting for CPU and memory.

---

## 2. Service Functional Clusters (Platform Service FCs & Vehicle Service FCs)

These FCs provide higher-level services to applications:

### • State Management (ara::sm)

* **Purpose:** Determines and coordinates the desired operational states of the platform.
* **Key functions:** Coordinates Function Group State transitions by sending requests to Execution Management. Works with PHM to handle recovery actions and controls dynamic communication paths via Network Management.

### • Update and Configuration Management (ara::ucm)

* **Purpose:** Manages installation, update, removal, and version reporting of software on the Adaptive Platform in a safe and secure manner.
* **Key functions:** Receives Software Packages, performs cryptographic integrity and authenticity checks, handles sequential installation, provides rollback functionality, and supports updating FCs and the underlying OS.

### • Vehicle Update and Configuration Management (ara::vucm)

* **Purpose:** Coordinates vehicle-wide software update campaigns.
* **Key functions:** Executes updates according to the Vehicle Package Manifest, interacts with OTA Client and Vehicle State Manager (for safety conditions), and supervises lower-level UCM instances.

### • Safe Hardware Acceleration (ara::shwa)

* **Purpose:** Enables applications to use hardware accelerators (GPU, FPGA, DSP, etc.) for parallel computing while maintaining safety.
* **Key functions:** Manages data exchange between applications and HWA on the same ECU, monitors runtime status of accelerators, and provides an abstraction interface for selecting an appropriate compute device.

### • Automotive API Gateway (ara::aag)

* **Purpose:** Provides standardized access to vehicle data for external (non-AUTOSAR) clients.
* **Key functions:** Implements VISS (Vehicle Information Service Specification) based on VSS (Vehicle Signal Specification) and maps external VISS requests to internal ara::com service interfaces.

### • Sensor Interfaces (ara::adi)

* **Purpose:** Provides standardized service interfaces for vehicle sensors (Camera, Lidar, Radar, Ultrasonic) to support automated driving functions.
* **Key functions:** Ensures the interchangeability of service-compatible applications. Supports sensor types and signals defined by ISO-23150.

