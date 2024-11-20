# ADR-000: Choosing NVidea Jetson Nano

## **Status**
Accepted  

## **Context**
The Jetson Nano brings a lot of advantages when compared
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
