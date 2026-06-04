# CGRA Mapping

onnx-flow is able to transform ONNX models for mapping to the STRELA CGRA. This process involves two main steps.

The first step is the decomposition of the ONNX model for compatibility with the CGRA architecture. This involves breaking down some operations into simpler ones that are supported by the CGRA and splitting tensors until all operations incide on only 1D tensors (and scalar values). This step can be enabled using the `--decomposeForCgra` flag. Keep in mind that the supported decompositions are currently very limited to just a few operations (`MatMul`, `Add` and `Relu`) in some particular configurations. Therefore, applying this step may still results in an unmappable model.

The second step is the transformation of the decomposed ONNX model into an equivalent DOT format representation. The specialized formatter is only able to generate DOT graphs for models that are compatible with the CGRA architecture. This means that all operations must be decomposed into supported ones and all tensors must be 1D (or scalars). For this export step, use the flags `format=dot --formatter=cgra`.

## Usage

The general case for the transformation of an ONNX model for CGRA mapping is as follows:

```bash
onnx-flow input_model.onnx --output=output_model.dot --format=dot --formatter=cgra --decomposeForCgra --vz=0
```

If you want to visualize the resulting DOT graph, you can change the visualization option (`--vz`) to `1` or `2`.

## Supported Decompositions

| Operation | Supported Configurations |
|-----------|--------------------------|
| MatMul    | Both inputs are 2D tensors (matrices). |
| Add       | Any configuration with both tensors of the same dimensions |
| Relu      | Any configuration |
