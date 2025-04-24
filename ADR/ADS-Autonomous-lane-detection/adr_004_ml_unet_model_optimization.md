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
- **Visual Results:**
