# Adaptive AUTOSAR Overview


Adaptive Platform (AP) is a standardized software platform designed for high-performance Electronic Control Units (ECUs), particularly supporting complex use cases such as advanced driver assistance and higher-level autonomous driving. AP was developed to meet requirements that the Classic Platform (CP) cannot fulfill, especially those demanding higher computational power and flexible software configurability (e.g., over-the-air updates).

<p align="center">
    <img src="Images/Pasted image 20251206212502.png" alt="Image">
</p>

Below is an overview of the key aspects of Adaptive AUTOSAR:

## 1. Architecture and Functional Clusters (FCs)

<p align="center">
    <img src="Images/Pasted image 20251207002458.png" alt="Image">
</p>

AP Machine is AP Software Stack with OS and Functional Clusters and AA.
In Zonal E/E Architecture, this AP Machine will run as Vehicle Computer, Central/ADAS ECUs.

The AP architecture is built on a **Service-Oriented Architecture (SOA)**, where the system consists of independent services communicating with one another.

<p align="center">
    <img src="Images/Pasted image 20251207154059.png" alt="Image">
</p>

* **AUTOSAR Runtime for Adaptive Applications (ARA):**
  Adaptive Applications (AA) run on top of the ARA layer, which provides standardized application interfaces.

* **Functional Clusters (FCs):**
  ARA consists of multiple FCs grouped into core platform functionalities and services:

  * **Foundation FCs:** Provide fundamental platform capabilities, including Communication Management (ara::com), Adaptive Core (ara::core), Execution Management (ara::exec), Platform Health Management (ara::phm), Logging and Tracing (ara::log), Persistence (ara::per), Cryptography (ara::crypto), Firewalling (ara::fw), Time Synchronization (ara::tsync), Network Management (ara::nm), Intrusion Detection System Management (ara::idsm), and Safety Hardware Acceleration (ara::shwa).

  * **Service FCs:** Provide standard services such as Update and Configuration Management (ara::ucm) and State Management (ara::sm).

  * **Vehicle Service FCs:** Include Automotive API Gateway (ara::aag) and Remote Persistence (ara::rper service).

## 2. Main Communication Model (ara::com)

**ara::com** is the primary interface for application communication, supporting intra-process, inter-process, and inter-machine communication.

* **Proxy/Skeleton Architecture:**
  ara::com uses this abstraction model:

  * **Proxy:** Client-side representation of a service.
  * **Skeleton:** Connects the service implementation to the middleware transport layer.

* **Service Model:** Communication follows a service-based model consisting of:

  * **Methods:** Used for remote procedure calls (RPC), either synchronous or asynchronous. “Fire-and-Forget” methods expect no response.
  * **Events & Triggers:** Notification mechanisms from servers to subscribed clients. Events carry data; triggers do not.
  * **Fields:** Combine event behavior (notifications on value change) with method-like Get/Set access.

* **Programming Styles:**
  AP supports both **event-driven (callback-based)** and **polling-based** communication to satisfy different real-time requirements.

## 3. Technical and Development Aspects

* **Programming Language:**
  AP uses **C++** (minimum standard C++17) as the basis for all application interfaces.

* **Error Handling:**
  To meet functional safety requirements (e.g., ASIL), AP adopts an exception-less API design. Recoverable errors are returned using `ara::core::Result` containing an `ara::core::ErrorCode`.

* **Configuration:**
  System configuration is defined through **Manifest** files (ARXML format):

  * **Execution Manifest:** Deployment information for processes/applications.
  * **Service Instance Manifest:** Detailed service communication configuration, e.g., mapping to SOME/IP transport.
  * **Machine Manifest:** Hardware-specific configuration.

* **Updates:**
  **Update and Configuration Management (UCM)** handles installation, update, and removal of software securely—commonly including over-the-air (OTA) updates.

* **Parallelism:**
  AP is designed to leverage heterogeneous computing architectures, allowing applications to offload heavy computations to specialized hardware accelerators (e.g., GPU, FPGA) via the Safety Hardware Acceleration FC (ara::shwa).

---

Adaptive AUTOSAR is a highly flexible but complex platform designed to support the next generation of automotive systems requiring high performance, dynamic deployment, and strict safety/security standards.

---

### *Analogy Example:*

You can imagine Adaptive AUTOSAR as a modern smart city, completely different from a traditional village (Classic Platform). In this city, every function is provided by specialized **services** (ara::com). Instead of sending letters (signals), you make phone calls (Methods) or receive instant notifications (Events). To build or upgrade the city, strict regulations (Manifest files) must be followed, and a central management authority (Execution Management/UCM) ensures safety and reliability. Citizens (Adaptive Applications) can build their own solutions on top of the shared infrastructure (Functional Clusters).
