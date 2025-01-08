# ADR-005: Dependency Injection and Testing Strategy for ROS2 Nodes

## Context
The JetRacer project requires reliable interaction with hardware components, such as motor drivers and sensors. Testing components that depend on hardware traditionally requires physical devices, which can make unit testing difficult, time-consuming, and unreliable. Additionally, direct system calls or tightly coupled hardware dependencies in business logic can hinder maintainability and reusability.

The key considerations are:
- **Testability**: Ability to test hardware-dependent components without physical hardware
- **Dependency Injection**: Flexibility to use mock objects for testing
- **Error Handling**: Proper handling of errors in hardware interactions
- **Maintainability**: Decouple business logic from hardware implementations

## Decision
The solution involves designing a dependency injection strategy for ROS2 nodes and creating mock implementations to facilitate unit testing. This is achieved through the following components:

1. **Abstract Interface**:
   Create abstract classes for hardware interfaces (e.g., `APCA9685Driver`, `AI2cDevice`) that define methods for interacting with hardware components:
   - Virtual methods for core operations (e.g., initialization, reading, writing, setting parameters)
   - Enables polymorphic use of real or mock implementations

2. **Concrete Implementation**:
   Provide concrete classes for actual hardware implementations, including:
   - Proper buffer management and resource cleanup
   - Error handling and retry mechanisms for hardware communication
   - Adherence to the abstract interface

3. **Dependency Injection in Nodes**:
   Design ROS2 nodes to accept dependencies as constructor arguments:
   - Accept instances of the abstract interface (e.g., `std::shared_ptr<IPCA9685Driver>`) for flexibility
   - Decouple the ROS2 node logic from hardware-specific details
   - Enable testing by injecting mock implementations

4. **Mock Implementations for Testing**:
   Create mock classes for hardware dependencies using frameworks like Google Mock:
   - Implement the abstract interface
   - Allow setting expectations and simulating various hardware responses
   - Test edge cases and error scenarios without physical hardware
