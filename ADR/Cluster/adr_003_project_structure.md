# ADR-0003: Project Structure and Layered Abstraction for Jetracer

## **Status**
Accepted  

## **Context**
The Jetracer project requires a well-structured software architecture to facilitate modularity, scalability, and separation of concerns. Key requirements include:  

- **Layered Abstraction**: To isolate hardware interactions, device-specific logic, and high-level functionality.  
- **ROS2 Compatibility**: As a node-based framework, ROS2 naturally supports modular development.  
- **Ease of Debugging and Testing**: Logical separation improves maintainability.  

### Key Architectural Layers  
1. **Bus Interfaces**: Directly interact with hardware data buses (e.g., CAN, I2C).  
2. **Peripherals**: Handle device-specific logic, leveraging bus interface nodes for communication.  
3. **Head Unit**: Implements high-level coordination and user interaction functionalities.

### Benefits of This Structure  
- **Scalability**: Additional peripherals or interfaces can be added without disrupting the overall architecture.  
- **Modularity**: Independent nodes encapsulate specific functionalities, making debugging and testing easier.  
- **Clear Dependency Hierarchy**: Lower layers (e.g., bus interfaces) provide services to higher layers, avoiding circular dependencies.

## **Decision**
Adopt a layered project structure for Jetracer, as illustrated below:
```
JetRacer/
├── src/
│   ├── bus_interfaces/
│   │   ├── can_interface/
│   │   │   ├── CMakeLists.txt
│   │   │   ├── package.xml
│   │   │   ├── src/
│   │   │   └── include/
│   │   ├── i2c_interface/
│   │       ├── CMakeLists.txt
│   │       ├── package.xml
│   │       ├── src/
│   │       └── include/
│   ├── peripherals/
│   │   ├── oled_display/
│   │   │   ├── CMakeLists.txt
│   │   │   ├── package.xml
│   │   │   ├── src/
│   │   │   └── include/
│   │   ├── servo/
│   │   │   ├── CMakeLists.txt
│   │   │   ├── package.xml
│   │   │   ├── src/
│   │   │   └── include/
│   │   ├── dc_motors/
│   │       ├── CMakeLists.txt
│   │       ├── package.xml
│   │       ├── src/
│   │       └── include/
│   ├── head_unit/
│   │   ├── coordinator/
│   │   │   ├── CMakeLists.txt
│   │   │   ├── package.xml
│   │   │   ├── src/
│   │   │   └── include/
│   │   ├── infotainment/
│   │       ├── CMakeLists.txt
│   │       ├── package.xml
│   │       ├── src/
│   │       └── include/
├── CMakeLists.txt
└── colcon.meta
```

## **Consequences**
- **Separation of Concerns**:  
   - **Bus Interfaces** (e.g., `can_interface`, `i2c_interface`): Abstract hardware communication to provide reusable services for higher layers.  
   - **Peripherals** (e.g., `oled_display`, `servo`, `dc_motors`): Encapsulate device-specific logic.  
   - **Head Unit** (e.g., `coordinator`, `infotainment`): Manage overall system behavior and user interaction.  
- **Collaboration**: Enables parallel development by different team members across layers.  
- **Dependency Management**: Lower-layer services are accessed via ROS2 topics or services, ensuring a clear and modular dependency structure.  

