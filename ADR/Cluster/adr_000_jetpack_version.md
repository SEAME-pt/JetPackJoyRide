# ADR-0001: JetPack OS Version

## **Status**
Accepted  

## **Context**
The Jetson Nano board requires an operating system for software development and deployment. JetPack is NVIDIA's official software suite for Jetson devices, providing drivers, libraries, and tools for AI and robotics applications.

- JetPack 4.6 is the latest version compatible with the Jetson Nano.  
- Newer versions of JetPack do not support the Jetson Nano.  

Other considerations:  
- Compatibility with ROS2 and Fast DDS middleware.  
- Stability for long-term project maintenance.

## **Decision**
Install JetPack 4.6 on the Jetson Nano.

## **Consequences**
- Using the latest compatible version ensures stability and access to updated features.  
- Future migration to newer hardware may require transitioning to newer JetPack versions.  

