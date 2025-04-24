# ADR-003: Input Preprocessing and Memory Handling for TensorRT Inference
## Context
During deployment of our lane detection model to Jetson Nano, we encountered two critical implementation issues:
1. Channel Ordering Mismatch between OpenCV (BGR) and model expectations (RGB)
2. Incorrect Memory Buffer Calculations for input/output tensors
## Problem Analysis
### 1. Channel Ordering Issue
**Symptoms**:
- Inverted/incorrect lane predictions
- 1 / 4 of the output we should get / image repeating itself inside of the buffer

**Root Cause**:
```cpp
// Original incorrect BGR order
og_image[(y * cols + x) * 3] = pixel[0]; // B
og_image[(y * cols + x) * 3 + 1] = pixel[1]; // G
og_image[(y * cols + x) * 3 + 2] = pixel[2]; // R
```
### 2. Memory Calculation Issue
**Symptoms**:
- Segmentation faults
- Inconsistent outputs
**Root Cause**:
```cpp
size_t inputSize = 256 * 256 * 3; // Hardcoded dimensions
```
## Solution
### 1. Channel Ordering Fix
```cpp
// Correct RGB order
og_image[(y * cols + x) * 3] = pixel[2] / 255.0f; // R
og_image[(y * cols + x) * 3 + 1] = pixel[1] / 255.0f; // G
og_image[(y * cols + x) * 3 + 2] = pixel[0] / 255.0f; // B
```
### 2. Dynamic Memory Calculation
```cpp
size_t inputSize = 1;
for (int i = 0; i < inputDims.nbDims; i++) {
    inputSize *= inputDims.d[i]; 
}
inputSize *= sizeof(float);
```

## References
1. OpenCV docs
2. TensorRT best practices
