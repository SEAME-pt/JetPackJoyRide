# ADR-0002: Using ROS2 with Fast DDS Middleware

## **Status**
Accepted  

## **Context**
The JetRacer project requires a software framework for handling communication between components. Key considerations include:  

- **Modularity**: The system needs to support multiple independent nodes (e.g., speed sensor, instrument cluster display).  
- **Scalability**: The framework should handle increasing complexity without significant architectural changes.  
- **Middleware**: Publish-subscribe communication model is suitable for distributed systems.

### Why ROS2?  
1. **Modularity**: ROS2 provides a node-based architecture, allowing independent development and testing of components.  
2. **Industry Standard**: Widely used in robotics and autonomous vehicle applications.  
3. **Middleware Flexibility**: Supports multiple middleware implementations, including Fast DDS.  

### Why Fast DDS?  
1. **Performance**: Low latency and high throughput make it suitable for real-time applications.  
2. **Interoperability**: Allows seamless integration with ROS2 and Jetson Nano.  
3. **Scalability**: Supports distributed systems with publish-subscribe models.

## **Decision**
Adopt ROS2 as the robotics framework with Fast DDS as the middleware.

## **Consequences**
- Facilitates modular and scalable architecture for JetRacer.  
- Requires additional setup and learning curve for team members unfamiliar with ROS2 and DDS.  

