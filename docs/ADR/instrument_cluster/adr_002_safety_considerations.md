# ADR-0003: Safety Considerations for Using ROS2 with Fast DDS Middleware

## Status
Accepted

## Context
The JetRacer project requires a software framework for handling communication between components. Given the safety-critical nature of the system, it is essential to address concerns about reliability and fault tolerance when using ROS2 with Fast DDS middleware. The key considerations are:

- **Critical Node Failures**: Ensuring uninterrupted operation of safety-critical components like braking and steering.
- **Middleware Failures**: Addressing the risk of Fast DDS or ROS2 failures causing system-wide communication disruption.
- **Real-Time Performance**: Ensuring deterministic behavior for time-sensitive operations.
- **Data Security and Integrity**: Protecting communication from corruption or unauthorized access.
- **Fault Isolation**: Preventing cascading failures caused by non-critical subsystems.
- **System Overload**: Managing resource contention and load distribution effectively.

## Decision
The following safety measures could be adopted to mitigate the risks:

### 1. Redundancy
- **Node Redundancy**: Deploy redundant nodes for safety-critical functions, with a primary-backup setup. Use heartbeats to monitor node health and enable seamless failover.
- **Communication Redundancy**: Implement fallback IPC mechanisms (e.g., shared memory or CAN bus) to bypass middleware failures for critical data.
- **Hardware Redundancy**: Introduce secondary Jetson Nano units or microcontrollers for key safety tasks.

### 2. Lifecycle Nodes
- Use ROS2 Lifecycle Nodes for critical components to manage their states (e.g., activation, deactivation, and fault recovery).
- Add a supervisory node to monitor lifecycle states and automate recovery actions like restarting or switching to redundant nodes.

### 3. Real-Time Capabilities
- **RTOS Integration**: Use an RTOS (e.g., FreeRTOS or Zephyr) for time-sensitive tasks, integrated with ROS2 via micro-ROS.
- **QoS Configuration**:
  - **Reliable QoS**: Ensure critical messages are always delivered.
  - **Deadline QoS**: Set deadlines for time-sensitive topics to detect and address delays.
  - **History QoS**: Optimize history settings (e.g., `KEEP_LAST`) to reduce latency.

### 4. Security and Integrity
- **SROS2**:
  - Encrypt communication and authenticate nodes to secure data.
  - Enforce strict access control policies for joining nodes.
- **Message Integrity**:
  - Implement checksums or hash-based integrity validation for critical messages.

### 5. Fault Isolation
- **Prioritization**:
  - Ensure critical nodes are prioritized for CPU, memory, and network resources.
  - Deactivate non-critical nodes when resource contention occurs.

## Consequences
### Benefits
- Enhanced reliability and fault tolerance for safety-critical components.
- Deterministic behavior for real-time operations.
- Secured and integrity-verified communication between components.
- Modular and scalable architecture that isolates non-critical failures.

### Challenges
- Increased complexity in setup and configuration.
- Higher computational overhead due to redundancy and security features.
- Steeper learning curve for implementing RTOS and advanced ROS2 features.

## Status
Accepted

## Related ADRs
- [ADR-001: Using ROS2 with Fast DDS Middleware](#adr_001_use_ros2.md)

