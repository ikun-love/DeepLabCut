(dlc3-user-guide)=
# DeepLabCut 3.0 - PyTorch 用户指南

## 使用 DeepLabCut 3.0

**DeepLabCut 3.0 保留了您熟悉的顶级 API，但拥有了一个全新的 PyTorch 后端。**
**此外，它经过了重写，更易于开发人员使用、功能更强大，并且是为现代基于深度学习的计算机视觉应用而构建的。**

**注意**🔥: 我们建议，如果您刚开始使用 DeepLabCut，就从 PyTorch 后端开始。通过查看主 `config.yaml` 文件或 GUI 右上角，您可以轻松知道您正在使用哪个“引擎”。如果您有 TensorFlow 格式的 DeepLabCut 项目，我们也为您做好了准备：您只需切换引擎（从而可以比较性能），即可无缝切换来训练您已经标记过的数据。简而言之，期待性能的提升 🔥。

简而言之，PyTorch 模型可以在任何 DeepLabCut 项目中进行训练。如果您已经有了一个项目，只需在项目 `config.yaml` 文件中添加一个新键，指定 `engine: pytorch` 即可。然后创建的任何新训练数据集都将是一个 PyTorch 模型（请参阅 [创建 Shuffle 和模型配置](
#Creating-Shuffles-and-Model-Configuration) 以了解更多关于训练 PyTorch 模型的信息）。要再次训练 TensorFlow 模型，您可以设置 `engine: tensorflow`。

### 安装

要查看 DeepLabCut 3.0 的安装指南，请查阅 [安装文档](how-to-install)。

### 使用 GUI

您可以使用 GUI 来训练 DeepLabCut 项目。您可以从右上角的下拉菜单中切换 PyTorch 和 TensorFlow 引擎。

### 快速指南（标准 API）

DLC 的标准用法没有改变（通过高级 API），如标准指南中所示：针对 [单一个体模型](https://deeplabcut.github.io/DeepLabCut/docs/standardDeepLabCut_UserGuide) 和 [多个个体模型](https://deeplabcut.github.io/DeepLabCut/docs/maDLC_UserGuide)。

也可以查看几个 COLAB 笔记本，了解如何使用这些代码。

对于

## 主要变更

### 从迭代次数（iterations）到周期（epochs）

DeepLabCut 3.0 中的 PyTorch 模型是根据设定的 `epochs`（周期）数量进行训练的，而不是最大 `iterations`（迭代次数）。一个 epoch 是对整个训练数据集的**一次完整遍历**，这意味着您的模型恰好看到过每一张训练图像一次。

- 因此，如果您的网络有 64 张训练图像，一个 epoch 就是 64 次迭代（当 Batch Size 为 1 时），或者 32 次迭代（当 Batch Size 为 2 时），16 次迭代（当 Batch Size 为 4 时），依此类推。

## API

### 创建 Shuffle 和模型配置

您可以使用 `pytorch_config.yaml` 文件来配置模型，如 [此处](dlc3-pytorch-config) 所述。在 DeepLabCut 3.0 中，您可以使用与 TensorFlow 模型相同的方法来创建新的 Shuffle（例如 `deeplabcut.create_training_dataset` 和 `deeplabcut.create_training_model_comparison`）。

关于 DeepLabCut 中可用的不同 PyTorch 模型架构的更多信息，请参阅 [此处](architectures)。您可以使用以下命令查看支持的架构/变体的列表：

```python
from deeplabcut.pose_estimation_pytorch import available_models
print(available_models())
```



### 开发状态和路线图 🚧

下表描述了已为 PyTorch 引擎实现的 DeepLabCut API 方法，以及哪些选项尚未实现，哪些参数对 DLC 3.0 PyTorch API 无效的指示。


| API 方法 | 实现情况 | 尚未实现的参数 | 对 pytorch 无效的参数 |
|--------------------------------|:-----------:|-----------------------------------------------------------------------------------------------------|-----------------------------------------------------|
| `train_network` | 🟢 | | `maxiters`, `saveiters`, `allow_growth`, `autotune` |
| `return_train_network_path` | 🟢 | | |
| `evaluate_network` | 🟢 | | |
| `return_evaluate_network_data` | 🔴 | | `TFGPUinference`, `allow_growth` |
| `analyze_videos` | 🟠 | `greedy`, `calibrate`, `window_size` | |
| `create_tracking_dataset` | 🟢 | | |
| `analyze_time_lapse_frames` | 🟢 | 名称已更改为 `analyze_images`，以更好地反映其实际功能（不需要视频） | |
| `convert_detections2tracklets` | 🟠 | `greedy`, `calibrate`, `window_size` | |
| `extract_maps` | 🟢 | | |
| `visualize_scoremaps` | 🟢 | | |
| `visualize_locrefs` | 🟢 | | |
| `visualize_paf` | 🟢 | | |
| `extract_save_all_maps` | 🟢 | | |
| `export_model` | 🟢 | | |