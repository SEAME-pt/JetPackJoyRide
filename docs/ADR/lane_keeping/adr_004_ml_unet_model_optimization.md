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

## Attempt template (delete after):

### Description:

#### Model Specifications:

- **Input / Output:**
  [Describe the input and output types, e.g., image size, number of channels, etc.]

- **Number of Layers:**
  [Specify the number of layers in the model.]

- **Dataset Size:**
  [Indicate the size of the dataset used for training.]

#### Results Attained:

- **FPS (Frames Per Second):**
  [Provide the FPS measurement after running the model.]

- **Model Size:**
  [Provide the size of the model before conversion, typically in MB.]

- **Size when converted to ONNX:**
  [Provide the model size after conversion to ONNX format.]

- **Size when converted to TensorRT:**
  [Provide the model size after conversion to TensorRT format.]

- **Results Example:**
  [Visual results obtained using the model.]

