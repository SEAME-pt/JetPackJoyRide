<p align="center">
  <img src="https://github.com/user-attachments/assets/338d4621-d5dc-4136-81c5-252edcbcf5e0" alt="SEA:ME Portugal logo"/>
</p>

# JetpackJoyRide
Main repository of our [SEA:ME](https://seame.space/) team with the objective of building a functioning intelligent mobility system and its documentation.

---

## 📁 Repo Index

This repo contains several submodules, each tackling a specific component of our intelligent mobility stack—from warm-up exercises to advanced autonomous driving systems.

--- 

### 🔰 [SEAME-WarmUp-24-25](https://github.com/Axel-ex/SEAME-WarmUp-24-25)

First module of the course. Includes C++ exercises to refresh key concepts and an introduction to the **Qt framework**, which we use later to design the infotainment system.

---

### 🚘 [SEAME-Cluster-24-25](https://github.com/Xyckens/SEAME-Cluster-24-25)

Development of the **Instrument Cluster** for our JetRacer vehicle.

> Qt-based graphical interface displaying real-time vehicle metrics (speed, battery, etc.), communicating over **CAN** and **I2C** using a **Raspberry Pi 4**, with data gathered by an **Arduino Uno ECU**.

**Key Subfolders:**
- `Jetracer`: ROS2 workspace running on the Raspberry Pi.
- `JetRacer_container`: Docker environment to build and run the stack with correct hardware access.
- `ECU_code`: Code for the Arduino Uno acting as the Electronic Control Unit.

**Documentation:** [JetRacer Documentation](https://xyckens.github.io/SEAME-Cluster-24-25/)

---

### 🤖 [SEAME-ADS-Autonomous-lane-detection-24-25](https://github.com/Axel-ex/SEAME-ADS-Autonomous-lane-detection-24-25)
This project implements an **Autonomous Driving System (ADS)**, integrating **lane detection** with steering control on a JetRacer.

> Combines **computer vision** and **machine learning**, powered by **ROS 2** on a **Jetson Nano** for GPU-accelerated real-time performance.

**Key Features:**
- Modular ROS2 nodes for camera, control, and perception.
- Runs both in **Carla simulator** and in the **real world**.
- Hybrid lane detection (OpenCV + ML).
- Launch tools for real-time or replayed data workflows.

**Documentation**: [Jetracer Documentation](https://axel-ex.github.io/SEAME-ADS-Autonomous-lane-detection-24-25)

---

### ⚙️  [SEAME-MachineLearningModel_24-25](https://github.com/peterbikes/SEAME-MachineLearningModel_24-25)

....

> Used to create / train / convert our model 

**Structure:**


---

### ⚡ [SEAME-ML-Model-Jetson-24-25](https://github.com/Axel-ex/SEAME-ML-Model-Jetson-24-25)

Reference repo for doing inference using TensorRT directly in **C++** on the **Jetson Nano**.

> Used for benchmarking `.engine` files or debugging single inference calls—great for embedded ML deployments.

**Structure:**
- `benchmarking/`: Times multiple engine files and reports inference performance.
- `test_engine/`: Runs inference on a single image (debug-focused).

---

## 🧠 Contributors & Acknowledgements

This repository is maintained by the **SEA:ME JetpackJoyRide team** ([@Axel](https://github.com/Axel-ex), [@Pedro](https://github.com/peterbikes), [@Joao](https://github.com/jpedroegger), [@Francisco](https://github.com/Xyckens)) as part of our coursework in intelligent mobility.
