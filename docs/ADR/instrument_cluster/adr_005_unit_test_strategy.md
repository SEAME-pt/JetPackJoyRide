# ADR-005: I2C Interface Abstraction and Testing Strategy

## Context
The JetRacer project requires reliable I2C communication for various peripherals, including the OLED display. Testing I2C-dependent components traditionally requires physical hardware, making unit testing difficult and potentially unreliable. Additionally, direct system calls for I2C communication create tight coupling between the business logic and hardware implementation.

The key considerations are:
- **Testability**: Need to test I2C-dependent components without physical hardware
- **Dependency Injection**: Allow for different I2C implementations (real hardware vs. mock)
- **Error Handling**: Properly handle I2C communication errors and retries
- **Code Reusability**: Create a consistent interface for all I2C communications
- **Maintainability**: Separate hardware-specific code from business logic

## Decision
The solution involves creating an abstraction layer for I2C communication through the following components:

1. **Abstract I2C Interface**:
   Create an abstract class (`AI2cDevice`) that defines the interface for I2C operations:
   - Virtual methods for basic I2C operations (open, read, write, setAddress)
   - Clear documentation of expected behavior and error conditions
   - Protected member for device file descriptor

2. **Concrete Implementation**:
   Implement a concrete class (`I2cDevice`) that handles actual hardware communication:
   - Proper buffer handling for read/write operations
   - Retry mechanism for failed operations
   - Error reporting through return values
   - Resource cleanup in destructor

3. **ROS2 Service Interface**:
   Create a ROS2 service node (`I2cInterface`) that:
   - Accepts I2C read/write requests through a custom service
   - Uses dependency injection to accept any `AI2cDevice` implementation
   - Handles service requests and responses with proper error reporting

4. **Mock Implementation**:
   Create a mock I2C device (`MockI2cDevice`) for testing that:
   - Implements the `AI2cDevice` interface
   - Allows setting expected responses and verifying calls
   - Simulates common I2C errors and edge cases

5. **Error Handling Strategy**:
   Implement comprehensive error handling:
   ```cpp
   if (i2c_device_.setAddress(request->device_address) != 0) {
       std::string error_msg = fmt::format(
           "Fail to set the device at address 0x{:02X}",
           request->device_address);
       RCLCPP_ERROR(this->get_logger(), "%s", error_msg.c_str());
       response->set__success(false);
       response->set__message(error_msg);
       return;
   }
   ```

## Benefits
- **Improved Testability**: Unit tests can run without physical hardware
- **Separation of Concerns**: Hardware implementation details are isolated
- **Consistent Interface**: All I2C communications follow the same pattern
- **Better Error Handling**: Centralized error handling and retry logic
- **Maintainable Code**: Easy to modify or replace I2C implementation
- **Reusable Components**: Abstract interface can be used across different projects

## Consequences

### Positive
- Unit tests can be written and run without hardware dependencies
- Easier to implement and test error handling
- Clear separation between hardware and business logic
- Consistent error handling across all I2C operations

### Negative
- Additional complexity in the codebase
- Need to maintain mock implementations
- Possible overhead from abstraction layer

## References
- Linux I2C documentation
- ROS2 service interface documentation
- Google Test/Mock documentation
