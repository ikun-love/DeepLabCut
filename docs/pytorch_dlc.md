```markdown
# DeepLabCut: PyTorch API

## 模块 (Modules)

- [data](https://github.com/nastya236/DLCdev/blob/69005057eeac3c1492712863303f8268cee776e6/deeplabcut/pose_estimation_pytorch/data/project.py#L7):
`deeplabcut.pose_estimations_pytorch.data` 包包含了所有用于 PyTorch 数据集创建以及训练/测试集划分的代码。
  - `Project` 类提供了训练集和测试集的划分功能，并将数据集转换为所需的格式。例如，转换为 [COCO]() 格式。
  - `PoseTrainDataset` 类是一个 [torch.utils.Dataset](https://pytorch.org/docs/stable/data.html) 类，它将原始图像和关键点转换为用于训练和评估的张量数据集。
- [models](https://github.com/nastya236/DLCdev/blob/69005057eeac3c1492712863303f8268cee776e6/deeplabcut/pose_estimation_pytorch/data/models):
`deeplabcut.pose_estimations_pytorch.models` 包包含了所有与使用 `backbone`（骨干网络）、`neck`（颈部网络，可选）和 `head`（头部网络）构建模型相关的代码。
- [train_module](https://github.com/nastya236/DLCdev/blob/69005057eeac3c1492712863303f8268cee776e6/deeplabcut/pose_estimation_pytorch/data/models):
`deeplabcut.pose_estimations_pytorch.train_module` 包含了所有用于模型训练和验证的类。

## API

DeepLabCut 的 PyTorch 实现与 Tensorflow 的多动物（multi-animal）实现非常相似：需要遵循相同的步骤，只是 API 调用方式（以及模型名称）略有不同。

在创建训练数据集之前，创建 PyTorch 或 Tensorflow 项目的方式上没有变化。

### 创建训练数据集 (Creating a Training Dataset)

要为 DeepLabCut PyTorch 模型创建训练数据集，只需调用：
```python
import deeplabcut
deeplabcut.create_training_dataset(
    path_config_file,
    net_type="dekr_32",
)
```

这将以与 Tensorflow 版本相同的方式创建训练数据集的文件夹，并在 `train` 文件夹中增加一个配置文件：`pytorch_config.yaml`。该文件可以被编辑，用于修改模型架构或训练参数。

目前，PyTorch 中实现了两种主要的模型“家族”：DEKR (Geng, Zigang, et al. "Bottom-up human pose estimation via disentangled keypoint regression." Proceedings of the IEEE/CVF conference on computer vision and pattern recognition. 2021.) 和 Tokenpose (Li, Yanjie, et al. "Tokenpose: Learning keypoint tokens for human pose estimation." Proceedings of the IEEE/CVF International conference on computer vision. 2021.)。用于创建 PyTorch 训练集的 `net_type` 选择包括：
- `"dekr_16"`
- `"dekr_32"`
- `"dekr_48"`
- `"token_pose_w16"`
- `"token_pose_w32"`
- `"token_pose_w48"`

请注意，Tokenpose 模型目前不能用于包含唯一关键点（unique keypoints）的项目。

### 训练网络 (Training the network)
训练 PyTorch 模型的方式与 Tensorflow 模型非常相似，尽管目前需要直接调用 PyTorch API：
```python
import deeplabcut.pose_estimation_pytorch.apis as api
api.train_network(config_path, shuffle=1, trainingsetindex=0)
```

**参数 (Parameters)**
```
config : 项目 yaml 配置文件路径
shuffle : 想要训练的 shuffle 的索引
trainingsetindex : 训练集索引
transform: 图像的增强（Augmentation）管道
    如果为 None，则从配置文件构建增强管道
    如果您想使用自定义变换的建议：
        请记住，为了使迁移学习（transfer learning）有效，您的
        数据统计分布应与用于预训练骨干网络 (backbone) 的分布相似
        在大多数情况下（例如，骨干网络在 ImageNet 上预训练），这意味着它应该使用以下方式进行归一化 (Normalized with)：
        A.Normalize(mean = [0.485, 0.456, 0.406], std = [0.229, 0.224, 0.225])
transform_cropped: 围绕动物裁剪图像的增强管道
    如果为 None，则从配置文件构建增强管道
    如果您想使用自定义变换的建议：
        请记住，为了使迁移学习（transfer learning）有效，您的
        数据统计分布应与用于预训练骨干网络 (backbone) 的分布相似
        在大多数情况下（例如，骨干网络在 ImageNet 上预训练），这意味着它应该使用以下方式进行归一化 (Normalized with)：
        A.Normalize(mean = [0.485, 0.456, 0.406], std = [0.229, 0.224, 0.225])
modelprefix: 包含用于训练网络的 deeplabcut 配置文件的目录（也是快照保存的位置）。默认情况下，假定它们存在于项目文件夹中。
snapshot_path: 如果是恢复训练，则用于指定从哪个快照恢复
detector_path: 如果是恢复训练自上而下的模型（top down model），则用于指定从哪个检测器快照恢复
**kwargs : pytorch_config 字典中的任何条目。例如，请查看项目文件夹中 pytorch_cfg.yaml 文件以获取完整列表
```

### 评估网络 (Evaluating the network)
与训练一样，主要区别在于需要直接调用 API。
```python
import deeplabcut.pose_estimation_pytorch.apis as api
api.evaluate_network(config_path, shuffle=1, trainingsetindex="all")
```

**参数 (Parameters)**
```
config: 项目配置文件路径
shuffles: 一个可迭代对象，包含要评估的 shuffle 索引。
trainingsetindex: 指定使用哪个训练集部分的整数。如果设置为 "all"，则评估所有部分。
snapshotindex: 要加载的快照的索引（从 0 开始）。要评估最后一个快照，请使用 -1。要评估所有快照，请使用 "all"。例如，如果我们保存了 3 个模型
        - snapshot-0.pt
        - snapshot-50.pt
        - snapshot-100.pt
    并且我们想要评估 snapshot-50.pt，snapshotindex 应为 1。如果为 None，则从项目配置中加载 snapshotindex。
plotting: 在训练和测试图像上绘制预测结果。如果提供了该参数，它必须是 ``True``、``False``、``"bodypart"`` 或 ``"individual"`` 中的一个。对于多动物项目，设置为 ``True`` 默认为 ``"bodypart"``。
show_errors: 显示训练和测试误差。
transform: 评估的变换管道
    ** 应以与训练期间归一化数据相同的方式对数据进行归一化 **
modelprefix: 评估网络时使用的 deeplabcut 模型的目录。默认情况下，假定它们存在于项目文件夹中。
batch_size: 评估时使用的批次大小 (batch size)
```

### 分析新视频 (Analyzing novel videos)
PyTorch 和 Tensorflow 实现之间的一个主要区别在于动物组装（animal assembly）的方式（针对多动物模型）。在 Tensorflow 中，组装是与关键点提取分离的一个独立步骤；而在 PyTorch 版本中，它被直接集成到模型中。从 API 的角度来看，这并没有太大变化。

同样，需要直接调用 PyTorch API（它也具有 `auto_track` 选项）。
```python
import deeplabcut.pose_estimation_pytorch.apis as api
api.analyze_videos(config_path, ["/fullpath/project/videos/test.mp4"], videotype=".mp4")
```

PyTorch 检测到的结果需要使用 PyTorch API 转换为轨迹（tracklets），然后才能使用原始的轨迹拼接功能。
```python
import deeplabcut
import deeplabcut.pose_estimation_pytorch.apis as api
api.convert_detections2tracklets(
    config_path,
    videos=['/fullpath/project/videos/test.mp4'],
    videotype=".mp4",
)
deeplabcut.stitch_tracklets(
    config_path,
    videos=['/fullpath/project/videos/test.mp4'],
    videotype=".mp4",
)
```

之后，创建带标签视频（labeled videos）的调用方式与之前完全相同。
```python
import deeplabcut
deeplabcut.create_labeled_video(
    config_path,
    videos=['/fullpath/project/videos/test.mp4'],
    videotype=".mp4",
)
```
```