# ADR-003: Machine Learning Model

## U-Net - Machine Learning Model Approach

## Context

For this module, our system requires a robust machine learning model for lane detection on embedded hardware. The implementation must process real-time camera feed data and accurately segment lanes to assist the vehicle's control system. Given the constraints of running on the  **Jetson Nano** , choosing an efficient model is critical to balancing performance and computational limitations.

The key considerations are:

* **Accuracy:** The model must reliably segment lanes under different lighting and road conditions.
* **Computational Efficiency:** The solution must run efficiently on a resource-constrained system without significant latency.
* **Integration:** The model should seamlessly integrate with **ROS** for real-time processing and communication with other modules.
* **Scalability:** The chosen approach should allow for future improvements, such as detecting additional road features beyond lanes.
* **Optimization Potential:** The model should support techniques like quantization or pruning to further optimize performance for embedded deployment.

## Development Phases:

* **Phase 1: Select the Most Suitable ML Algorithm**
  * Evaluate and choose the best machine learning algorithm based on system requirements and constraints (e.g., performance, efficiency, accuracy).
* **Phase 2: Develop and Train U-Net Model**
  * Collect and preprocess data from **CARLA** and **Jetson Mat** environments for training.
  * Develop a U-Net model tailored for lane detection tasks.
* **Phase 3: Optimize model for Jetson Nano** 
  * Convert the model to **ONNX/TensorRT** format.
  * Perform inference using C++ instead of Python for better compatibility with the Jetson Nano.
* **Phase 4: Integrate ML Node**
  * Incorporate the node into the rest of the system architecture, transforming it into a ROS node capable of receiving camera feeds and publishing to the motion control node.
  * Optimize the model’s speed and accuracy based on real-world testing and performance results.

## Why U-Net?

1.  **Efficient Semantic Segmentation** :

* **U-Net** is specifically designed for semantic segmentation tasks, which makes it highly effective for pixel-wise classification. In lane detection, this means U-Net can accurately classify each pixel as part of the lane or not, which is essential for detecting lanes from images. Its ability to segment road areas, including complex lanes in different lighting conditions, is a major advantage.

2.  **Lightweight and Optimized for Embedded Systems** :

* The **Jetson Nano** is a resource-constrained device, but U-Net can be adapted to work efficiently even on such low-power hardware. The model can be optimized for performance using quantization or pruning techniques, ensuring real-time lane detection without heavy computational overhead, which is crucial for embedded systems like the Jetson Nano.

3.  **ROS Integration** :

* U-Net can easily be integrated into **ROS** (Robot Operating System), which is widely used in robotics and autonomous vehicle systems. ROS supports modular software development, making it simple to connect the lane detection model with other systems (e.g., control systems, sensors, or planning algorithms) via ROS nodes. U-Net’s output can be seamlessly passed as input for other processes like steering or speed control in autonomous vehicles.

4.  **Robust to Variations in Road Conditions** :

* U-Net's architecture, with its use of skip connections, enables it to capture both high-level context and fine-grained details, making it highly robust in various conditions. This capability is useful for lane detection as it can handle complex scenes with curves, intersections, varying road textures, or partial occlusions (e.g., from other vehicles). It can effectively detect lanes in dynamic environments like highways or city streets.

5.  **Adaptable to Custom Datasets** :

* U-Net is highly customizable and can be fine-tuned on specific datasets. This allows you to train the model on your own lane detection dataset, improving performance for specific road conditions, weather, or geographical regions. The flexibility to adapt the model to custom data makes it suitable for deployment in real-world lane detection tasks, ensuring high accuracy in different environments.

## Consequences
**Positive:**
* The model efficiently detects lanes, generating masks from a variety of images in different environments.
* Valuable opportunity to work with machine learning models.

**Negative:**

* Some steps in the process took longer than expected, particularly model conversion and C++ inference.
* The model may be too heavy for the Jetson Nano.
* The model is currently limited to lane detection and may not support object detection.

## Related ADRs

- [ADR-001: Solving conversion to RT engine](https://github.com/SEAME-pt/JetPackJoyRide/blob/main/docs/ADR/lane_keeping/adr_001_RT_conversion.md)
