# ADR-004: ML U-Net Model Optimization

## U-Net - Machine Learning Optimization

## Context

The current U-Net model is being used for lane detection, but its performance on the embedded system (Jetson Nano) needs improvement to meet real-time processing requirements. To ensure efficient operation, we will explore several optimization techniques (e.g., quantization, pruning, and other adjustments) and document the alterations and results from each change. These optimizations aim to reduce computational overhead while maintaining or improving accuracy for lane detection in real-world conditions.

The key considerations are:

__Real-Time Performance:__ Achieve the ability to run the model in real time on the Jetson Nano.

__Memory and Computational Efficiency:__ Reduce the model's memory footprint and computational load without sacrificing detection accuracy.

__Adaptability:__ Ensure the optimizations do not compromise the model’s ability to generalize to different road conditions.

__Scalability:__ Allow for future updates and optimizations to maintain long-term system performance.

Below is an index of various attempts made to optimize the models, including the considerations taken, model specifications, modifications to inference using C++, and the results measured in frames per second.

## Attempt Index

- [Attempt #1: 3.7 FPS](#attempt-1-37-fps)
- [Attempt #2: 3.63 FPS - reduced dataset](#attempt-2-363-fps---reduced-dataset)
- [Attempt #3: 8.98 FPS - reduced dataset + halved filters](#attempt-3-898-fps---reduced-dataset--halved-filters)
- [Attempt #4: 10.01 FPS - reduced dataset + halved filters + less layers](#attempt-4-1001-fps---reduced-dataset--halved-filters--less-layers)

## Attempt #1: 3.7 FPS

### Description:

Original model, pre optimization attempts.

#### Model Specifications:

- **Input / Output:**

  * Input: `(256, 256, 3)`
  * Output: `(256, 256, 1)`

* **Number of Layers** :

| Layer Type      | Count        |
| --------------- | ------------ |
| Conv2D          | 15           |
| Conv2DTranspose | 4            |
| MaxPooling2D    | 4            |
| Concatenate     | 4            |
| **Total** | **27** |

- **Dataset Size:**
  * Jetracer mat: `185`
  * TuSimple: `3626`
  * Total: `3811`

* **Training settings**:
  * Epochs: 20
  * Batch size: 4

- **Model Size:** 360 Mb
- **ONNX Size:** 120 Mb
- **TensorRT Size:** 310 Mb

## Attempt #2: 3.63 FPS - reduced dataset

### Description:

Original model, only with a significantly smaller dataset.

#### Model Specifications:

- **Input / Output:**

  * Input: `(256, 256, 3)`
  * Output: `(256, 256, 1)`

* **Number of Layers** :

| Layer Type      | Count        |
| --------------- | ------------ |
| Conv2D          | 15           |
| Conv2DTranspose | 4            |
| MaxPooling2D    | 4            |
| Concatenate     | 4            |
| **Total** | **27** |

- **Dataset Size:**
  * Jetracer mat: `185`
  * Total: 185

* **Training settings**:
  * Epochs: 20
  * Batch size: 4

- **Model Size:** 360 MB
- **ONNX Size:** 119 Mb
- **TensorRT Size:** 155 Mb

## Attempt #3: 8.98 FPS - reduced dataset + halved filters

### Description:

For this attempt, we kept the same number of layers but halved the filters used in them:

64 is now 32

128 is now 64

The bottleneck of the model is now 512 instead of the previous 1024.

#### Model Specifications:

- **Input / Output:**

  * Input: `(256, 256, 3)`
  * Output: `(256, 256, 1)`

* **Number of Layers** :

| Layer Type      | Count        |
| --------------- | ------------ |
| Conv2D          | 15           |
| Conv2DTranspose | 4            |
| MaxPooling2D    | 4            |
| Concatenate     | 4            |
| **Total** | **27** |

- **Dataset Size:**
  * Jetracer mat: `185`
  * Total: 185

* **Training settings**:
  * Epochs: 20
  * Batch size: 4

- **Model Size:** 90 Mb
- **ONNX Size:** 30 Mb
- **TensorRT Size:** 39 Mb

## Attempt #4: 10.01 FPS - reduced dataset + halved filters + less layers

### Description:

For this attempt, we kept the halvetd filters (same has above) and a reduced number of layers. See model specifications. Results are, unfortunatelly, very bad.

#### Model Specifications:

- **Input / Output:**

  * Input: `(256, 256, 3)`
  * Output: `(256, 256, 1)`

* **Number of Layers** :

| Layer Type      | Count        |
| --------------- | ------------ |
| Conv2D          | 9            |
| Conv2DTranspose | 4            |
| MaxPooling2D    | 4            |
| Concatenate     | 4            |
| **Total** | **21** |

- **Dataset Size:**
  * Jetracer mat: `185`
  * Total: 185

* **Training settings**:
  * Epochs: 20
  * Batch size: 4

- **Model Size:** 54 Mb
- **ONNX Size:** 18 Mb
- **TensorRT Size:** 22 Mb
