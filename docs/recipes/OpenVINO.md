# Intel OpenVINO 后端

::::{warning}
此功能目前仅针对基于 TensorFlow 的模型实现。
::::

DeepLabCut 提供了一个选项，可以使用 [OpenVINO](https://github.com/openvinotoolkit/openvino) 后端运行深度学习模型。

要在您的流程中启用 OpenVINO，请在 `analyze_videos` 方法中使用 `use_openvino` 标志，并提供一个表示设备的字符串值：

* ```"CPU"``` - 使用 CPU。这是默认值。
* ```"GPU"``` - 使用 GPU（需要安装 OpenCL）。首次启动可能需要一些时间进行内核初始化。
* ```"MULTI:CPU,GPU"``` - 同时使用 CPU 和 GPU。在大多数情况下，此选项能提供最佳的效率。

```python
def analyze_videos(
    ...
    use_openvino="MULTI:CPU,GPU",
)
```

OpenVINO 是一个可选依赖项。您可以使用以下命令在安装 DeepLabCut 时一起安装它：

```bash
pip install deeplabcut[openvino]
```