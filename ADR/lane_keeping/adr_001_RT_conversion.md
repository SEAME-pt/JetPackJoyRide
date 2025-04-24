# ADR-001: Solving conversion to RT engine
## Context
The model needs to run on the Jetson nano:

- Ubuntu 18.04

- TensorRT 8.2.1.8

- CUDA 10.2

The development environment uses:

- Ubuntu 22.04

- TensorFlow 2.19.0

- Keras 3.9.1

- CUDA 11.8

## Potential problem
The version mismatch might create several incompatibilities:

- TensorRT Limitations: TensorRT 8.2 on Jetson doesn't support newer operations from TF 2.19/Keras 3.9 (typically some activation functions were introduced after TRT 8.2)

- ONNX Conversion Issues: The default opset for onnx conversion using tf2onnx is 15 and it might introduce unrecognised symbols 

## Current state
In the current state of the repo, the following operation fail when trying to convert to RT engine with trtexec:
```
[Reshape -> "conv2d_14"]:
input: "StatefulPartitionedCall/model/conv2d_14/Sigmoid:0"
input: "new_shape__206"
output: "conv2d_14"
name: "StatefulPartitionedCall/model/conv2d_14/BiasAdd__156"
op_type: "Reshape"

Attribute not found: allowzero
```
This typically happens because the model was converted with the default flags, using opset 15. After converting it using the right opset (11) we still get some errors due to the nature of our model. Namely during upsampling we get errors about a specific operations:

```
Error[10]: [optimizer.cpp::computeCosts::2011] Error Code 10: Internal Error 
(Could not find any implementation for node StatefulPartitionedCall/model/conv2d_transpose_3/BiasAdd.)
```
this error indicate that during upsampling (optimisation of the transpose convolution operation) we try to perform an operation that isn't supported :

```
up6 = layers.Conv2DTranspose(512, (3, 3), strides=(2, 2), padding='same')(conv5)

```
This was solved by increasing memory buffer size, see command below.

## Commands
Conversion to onnx with right opset:
```
 python3 -m tf2onnx.convert --saved-model saved_model --output model.onnx --opset 11
```
Conversion setting resolution and avoiding memory shortage
```
sudo /usr/src/tensorrt/bin/trtexec \
                                   --onnx=model_opset_11.onnx \
                                   --saveEngine=correct.engine \
                                   --best \
                                   --fp16 \
                                   --verbose \
                                   --workspace=2048
```

## Conclusion
There is basically a bunch of operations / activation functions / layers we need to avoid. Overall we need to make sure the operations we perform are compatible with our TensorRT version. Most importantly we need to use the right opset for our TensorRT version.

## References
- [CUDA / TensorRT / TensorFlow Compatibility](https://stackoverflow.com/questions/50622525/which-tensorflow-and-cuda-version-combinations-are-compatible)
- [tf2onnx opset flag](https://github.com/onnx/tensorflow-onnx)
