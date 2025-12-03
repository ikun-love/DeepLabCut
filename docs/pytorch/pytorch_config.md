```yaml
(dlc3-pytorch-config)=
# PyTorch 配置文件

`pytorch_config.yaml` 文件定义了 PyTorch 姿态模型的配置，范围涵盖模型架构、训练使用的优化器、训练运行的日志记录方式、应用的数据增强，以及用于保存“最佳”模型快照所依据的评估指标。

您可以使用 `deeplabcut.create_training_set` 或 `deeplabcut.create_training_model_comparison` 为 Shuffle 创建默认配置。这将为您选择的网络类型生成一个 `pytorch_config.yaml` 文件。该文件的基本结构如下：

```yaml
data:  # 将使用哪些数据增强
  ...
device: auto # 训练和评估使用的默认设备
inference:  # 配置与推理相关的参数（多线程、不同的 torch 选项）
metadata:  # 项目的元数据（身体部位、个体、路径等）- 自动填充
  ...
method: bu # 指示姿态预测的制作方式（自底向上 (`bu`) 或自顶向下 (`td`)）
model:  # 配置模型架构（使用哪个骨干网络、头部网络等）
  ...
net_type: resnet_50 # 文件中配置的神经网类型
runner:  # 配置用于训练的运行器 (runner)
  ...
train_settings:  # 通用训练设置，例如批次大小和最大训练周期数
  ...
logger:  # 可选：如果您需要日志记录器，这里是其配置
resume_training_from:  # 可选：从特定检查点重启训练
```

## 章节 (Sections)

### 单例参数 (Singleton Parameters)

PyTorch 配置文件中定义了几个单例参数：

- `device`: 用于训练/推理的设备。默认值为 `auto`，如果存在 NVIDIA GPU，则设置为 `cuda`，否则设置为 `cpu`。对于在配备 M1/M2/M3 芯片的 macOS 上运行模型的用户，特定模型的此值设置为 `mps`（并非所有操作都支持 Apple GPU，因此像 HRNets 这样的模型需要在 CPU 上训练，而 ResNets 等模型可以利用 GPU）。
- `method`: `bu`（用于自底向上模型）或 `td`（用于自顶向下模型）。
- `net_type`: 配置文件中配置的姿态模型类型（例如 `resnet_50`）。

### 数据 (Data)

`data` 部分配置以下内容：

- `bbox_margin`: 在生成边界框时，围绕真实姿态增加的边距（以像素为单位）。有关更多信息，请参阅 [从姿态生成边界框]( #bbox-from-pose)。
- `colormode`: 图像提供给模型的格式（例如，`RGB`，`BGR`）。
- `inference`: 在运行评估或推理时应用于图像的转换。
- `train`: 在训练时应用于图像的转换。

姿态模型的默认配置如下：

```yaml
data:
  bbox_margin: 20
  colormode: RGB  # 永远不应更改
  inference:  # 在推理期间应用于图像的增强
    normalize_images: true  # 此项应始终设置为 true
  train:
    affine:
      p: 0.5
      rotation: 30
      scaling: [0.5, 1.25]
      translation: 0
    covering: true
    crop_sampling:
      width: 448   # 如果您的图像非常小或非常大，您可能需要编辑！
      height: 448  # 有关 crop_sampling 的更多信息，请参阅下文！
      max_shift: 0.1
      method: hybrid
    gaussian_noise: 12.75
    motion_blur: true
    normalize_images: true  # 此项应始终设置为 true
```

以下转换可用于 `train` 和 `inference` 键。

**Affine**: 对图像应用仿射（旋转、平移、缩放）变换。

```yaml
affine:
  p: 0.9  # float: 应用仿射变换的概率
  rotation: 30  # int: 应用于图像的最大旋转角度（以度为单位）
  scaling: [ 0.5, 1.25 ]  # [float, float]: 用于调整图像大小的（最小, 最大）缩放比例
  translation: 40  # int: 应用于图像的最大平移量（以像素为单位）
```

**Auto-Padding**: 将图像填充到所需的形状（例如，最小高度/宽度，或使高度/宽度可被给定数字整除）。某些骨干网络（如 HRNets）要求图像的高度和宽度是 32 的倍数。通过将 `pad_height_divisor: 32` 和 `pad_width_divisor: 32` 设置自动填充，可以确保这种情况。请注意，**并非所有键都需要设置**！显示的值是默认值。'min_height' 和 'pad_height_divisor' 参数中**只能设置一个**，'min_width' 和 'pad_width_divisor' 参数中**只能设置一个**。

```yaml
auto_padding:
  min_height: null  # int: 如果不为 None，则为图像的最小高度
  min_width: null  # int: 如果不为 None，则为图像的最小宽度
  pad_height_divisor: null  # int: 如果不为 None，则确保图像高度可被此参数值整除。
  pad_width_divisor: null  # int: 如果不为 None，则确保图像宽度可被此参数值整除。
  position: random  # str: 图像位置，'A.PadIfNeeded.Position' 中的一个
  border_mode: reflect_101  # str: 'constant' 或 'reflect_101' (参见 cv2.BORDER 模式)
  border_value: null  # str: 如果 border_mode 是 'constant' 时的填充值
  border_mask_value: null  # str: 如果 border_mode 是 'constant' 时的蒙版填充值
```

**Covering**: 基于 Albumentations 的 [CoarseDropout](
https://albumentations.ai/docs/api_reference/augmentations/dropout/coarse_dropout/#albumentations.augmentations.dropout.coarse_dropout) 增强，此项会从图像中“剪切”掉区域。如 [Improved Regularization of Convolutional Neural Networks with Cutout](
https://arxiv.org/abs/1708.04552) 中定义。

```yaml
covering: true  # bool: 如果为 true，则以 50% 的概率应用粗略的 dropout
```

**Gaussian Noise**: 对输入图像应用高斯噪声。可以是浮点数（噪声的标准差）或简单布尔值（噪声的标准差将设置为 12.75）。

```yaml
gaussian_noise: 12.75  # bool, float: 添加高斯噪声
```

**Horizontal Flips**（水平翻转）：此操作会围绕 y 轴水平翻转图像。由于产生的图像被镜像，因此它不保留标签（左手会变成右手，反之亦然）。如果您的姿态模型具有对称关键点，则**不应**将此增强用于姿态模型！但是，它可安全地用于训练检测器。如果您想在具有对称关键点时使用水平翻转，则需要通过 `symmetries` 参数明确指定它们！

```yaml
# 用于目标检测器或没有对称（左右）关键点的情况的增强：
hflip: true

# 如果您的身体部位是 [snout, eye_L, eye_R, ear_L, ear_R] 时的增强
hflip:
  p: 0.5  # 以 50% 的概率应用水平翻转
  symmetries: [[1, 2], [3, 4]]  # 对称关键点的索引
```

**Histogram Equalization**（直方图均衡化）：以 50% 的概率应用直方图均衡化。

```yaml
hist_eq: true  # bool: 是否应用直方图均衡化
```

**Motion Blur**（运动模糊）：以 50% 的概率对图像应用运动模糊。

```yaml
motion_blur: true  # bool: 是否应用运动模糊
```

**Normalization**（归一化）：此项应始终设置为 `true`。

```yaml
normalize_images: true  # 归一化图像
```

### 处理可变图像尺寸

```{NOTE}
当使用批次大小 1 进行训练时（或者如果数据集中所有图像的大小都相同），您无需担心这些！但是，您仍然可以使用 `crop_sampling`，这可能会帮助您的模型泛化。
```

当使用大于 1 的批次大小进行训练时，批次中的所有图像**必须**具有相同的大小。PyTorch 会将所有图像整合到一个形状为 `[b, c, h, w]` 的张量中，其中 `b` 是批次大小，`c` 是图像的通道数，`h` 和 `w` 是批次中图像的高度和宽度。有几种不同的方法可以确保批次中的所有图像大小相同：

1. **裁剪采样 (Crop sampling)**。这是 DeepLabCut 中 PyTorch 引擎的默认行为。裁剪每个图像的一部分（固定大小）并将其提供给模型进行训练。有关更多信息，请参阅下文。
2. **自定义 collate 函数**。Collate 函数定义了如何将不同大小的图像组合成一个张量。这涉及到将图像调整大小和填充到相同的大小和纵横比。可用的 collate 函数定义在 `deeplabcut/pose_estimation_pytorch/data/collate.py` 中。
3. **调整所有图像的大小**。所有图像可以简单地调整到相同的大小。这通常不会带来最佳性能。

**调整大小 - 裁剪采样 (Resizing - Crop Sampling)**：确保批次中所有图像大小相同的另一种方法是通过裁剪。`crop_sampling` 将图像裁剪到最大宽度和高度，并提供选项，根据关键点的位置对裁剪的中心进行采样。采样裁剪中心的以下方法：

- `uniform`: 在图像上随机
- `keypoints`: 在标注的关键点上随机
- `density`: 优先考虑关键点密集区域的权重
- `hybrid`: 在 `uniform` 和 `density` 之间随机交替

```yaml
crop_sampling:
  height: 400  # int: 裁剪的高度
  width: 400  # int: 裁剪的宽度（此处原文有笔误，应为 width）
  max_shift: 0.4  # float: 允许的裁剪中心位置的最大偏移量，以裁剪尺寸的分数表示。
  method: hybrid # str: 中心采样方法（'uniform', 'keypoints', 'density', 'hybrid' 之一）
```

**Collate**：定义如何将图像整合成批次。默认使用的 collate 函数是 `ResizeFromDataSizeCollate`（其他 collate 函数定义在 `deeplabcut/pose_estimation_pytorch/data/collate.py`）。对于要整合的每个批次，此实现执行以下操作：
1. 通过获取批次中第一张图像的大小，并将其乘以从 `(min_scale, max_scale)` 中均匀随机采样的比例，来选择所有图像将调整到的大小。
2. 调整批次中所有图像的大小（同时保持其纵横比），使它们达到最小尺寸，使得目标尺寸完全包含在图像中。
3. 对每张结果图像进行随机裁剪以达到目标尺寸。

```yaml
collate:  # 在将图像放入批次时重新缩放它们
  type: ResizeFromDataSizeCollate  # 您也可以使用 `ResizeFromListCollate`
  max_shift: 10  # 添加到随机裁剪的最大偏移量（以像素为单位）（这意味着图像周围可能存在轻微边框）
  max_size: 1024  #  调整大小后图像长边的最大尺寸。如果最长边大于此值，则调整大小使最长边为此尺寸，而较短边小于目标尺寸。这对于保留具有极端纵横比的图像的一些信息很有用。
  min_scale: 0.4  # 调整图像大小时的最小缩放比例
  max_scale: 1.0  # 调整图像大小时的最大缩放比例
  min_short_side: 128  # 目标短边的最小值
  max_short_side: 1152  # 目标短边的最大值
  multiple_of: 32  # 填充目标高度、宽度，使它们是 32 的倍数
  to_square: false  # 不使用第一张图像的纵横比，而是仅使用第一张图像的短边来采样一个“边”，并将图像裁剪成正方形
```

**Resizing**（调整大小）：在保持纵横比的同时调整图像大小（首先调整到最大可能尺寸，然后为缺失的像素添加填充）。

```yaml
resize:
  height: 640 # int: 所有图像将调整到的目标高度
  width: 480 # int: 所有图像将调整到的目标宽度
  keep_ratio: true  # bool: 调整大小时是否应保持纵横比
```

### 模型 (Model)

模型配置进一步细分为 `backbone`（骨干网络），可选的 `neck`（颈部网络）和多个 `heads`（头部网络）。

更改 `model` 配置应仅由资深用户在极少数情况下进行。当更新模型配置（例如向 `HeatmapHead` 添加更多反卷积层）时，必须以一种方式完成，使模型配置对项目仍然有意义（例如，输出的热图数量需要与项目中的身体部位数量相匹配）。

单动物 HRNet 的模型配置示例可能如下所示：

```yaml
model:
  backbone:  # 姿态模型使用的 BaseBackbone
    type: HRNet
    model_name: hrnet_w18  # 创建一个 HRNet W18 骨干网络
  backbone_output_channels: 18
  heads:  # 配置不同头部网络如何做出预测
    bodypart:  # 配置如何为身体部位预测姿态
      type: HeatmapHead
      predictor:  # 用于从头部网络输出制作预测的 BasePredictor
        type: HeatmapPredictor
          ...
      target_generator:  # 用于为头部网络创建目标的 BaseTargetGenerator
        type: HeatmapPlateauGenerator
          ...
      criterion:  # 为头部网络使用的损失判据
        ...
      ...  # 特定于头部的选项，例如 "HeatmapHead" 的 `heatmap_config` 或 `locref_config`
```

`backbone`、`neck` 和 `head` 配置是使用 `deeplabcut.pose_estimation_pytorch.models.backbones.base.BACKBONES`、`deeplabcut.pose_estimation_pytorch.models.necks.base.NECKS` 和 `deeplabcut.pose_estimation_pytorch.models.heads.base.HEADS` 注册表加载的。您通过 `type` 参数指定要加载哪种类型。此后，头部的**任何参数**都可以在配置中使用。

因此，要为模型使用 `HRNet` 骨干网络（如在 `deeplabcut.pose_estimation_pytorch.models.backbones.hrnet.HRNet` 中定义），您可以设置：

```yaml
model:
  backbone:
    type: HRNet
    model_name: hrnet_w32  # 创建一个 HRNet W32
    pretrained: true  # 将从 TIMM（在 ImageNet 上预训练）加载用于训练的骨干网络权重
    interpolate_branches: false  # 不插值和连接所有分支的通道
    increased_channel_count: true  # 使用 TIMM HRNet 中定义的 incre_modules
  backbone_output_channels: 128  # 骨干网络输出的通道数
```

### 运行器 (Runner)

`runner` 包含与所使用的训练运行器相关的元素（包括优化器和学习率调度器）。除非您精通机器学习和模型训练，**否则不建议更改优化器或调度器**。

```yaml
runner:
  type: PoseTrainingRunner  # 不应需要修改此项
  key_metric: "test.mAP"  # 用于选择“最佳快照”的指标
  key_metric_asc: true  # 对于 key_metric，是否“越大越好”
  eval_interval: 1  # 每次遍历评估数据集的间隔
  optimizer:  # 用于训练模型的优化器
    ...
  scheduler:  # 可选：学习率调度器
    ...
  load_scheduler_state_dict: true/false # 在从快照恢复训练时是否加载调度器状态，
  snapshots:  # TorchSnapshotManager 的参数
    max_snapshots: 5  # 要保存的最大快照数（“最佳”模型不计入其中）
    save_epochs: 25  # 每次保存快照的间隔
    save_optimizer_state: false  # 是否将优化器状态与模型快照一起保存（很少有理由设置为 true）
  gpus: # 用于训练网络的 GPU
  - 0
  - 1
```

**关键指标 (Key metric)**：每次在测试集上评估模型时，都会计算指标以查看模型的性能如何。关键指标用于确定当前模型是否是迄今为止“最佳”模型。如果是，则快照将保存为 `...-best.pt`。对于姿态模型，可选择的指标包括 `test.mAP`（使用 `key_metric_asc: true`）或 `test.rmse`（使用 `key_metric_asc: false`）。

**评估间隔 (Evaluation interval)**：评估会减慢训练速度（需要时间遍历所有评估图像、进行预测和记录结果！）。因此，您可以决定每 5 个周期评估一次（通过设置 `eval_interval: 5`），而不是在每个周期后评估。虽然这意味着您获得的关于模型训练情况的信息较为粗略，但它可以加快大型数据集上的训练速度。

**优化器 (Optimizer)**：任何继承 `torch.optim.Optimizer` 的优化器。有关优化器的更多信息，请参阅 [PyTorch 文档](
https://pytorch.org/docs/stable/optim.html)。示例：

```yaml
  # SGD，初始学习率 1e-3，动量 0.9
  #  参见 https://pytorch.org/docs/stable/generated/torch.optim.SGD.html
  optimizer:
    type: SGD
    params:
      lr: 1e-3
      momentum: 0.9

  # AdamW 优化器，初始学习率 1e-4
  #  参见 https://pytorch.org/docs/stable/generated/torch.optim.AdamW.html
  optimizer:
    type: AdamW
    params:
      lr: 1e-4
```

**调度器 (Scheduler)**：您可以使用定义在 `torch.optim.lr_scheduler` 中的[任何调度器](
https://pytorch.org/docs/stable/optim.html#how-to-adjust-learning-rate)，其中给定的参数是调度器的参数。默认调度器是 `LRListScheduler`，它在每个里程碑将学习率更改为 `lr_list` 中的相应值。示例：

```yaml
  # 在 epoch 160 减小到 1e-5，在 epoch 190 减小到 1e-6
  scheduler:
    type: LRListScheduler
    params:
      lr_list: [ [ 1e-5 ], [ 1e-6 ] ]
      milestones: [ 160, 190 ]

  # 每隔 step_size 个周期，将每个参数组的学习率按 gamma 衰减
  #   参见 https://pytorch.org/docs/stable/generated/torch.optim.lr_scheduler.StepLR.html
  scheduler:
    type: StepLR
    params:
      step_size: 100
      gamma: 0.1
```

您还可以使用将其他调度器作为参数的调度器，例如 [`ChainedScheduler`](
https://pytorch.org/docs/stable/generated/torch.optim.lr_scheduler.ChainedScheduler.html)
或 [`SequentialLR`](
https://pytorch.org/docs/stable/generated/torch.optim.lr_scheduler.SequentialLR.html)。

`SequentialLR` 特别有用，例如，在一些预热周期中使用第一个调度器，然后在后期使用第二个调度器。一个使用示例是：

```yaml
  # 在前 `total_iters` 个周期内，将学习率乘以 `factor`
  # 在 5 个周期后，每隔 `step_size` 个周期开始按 `gamma` 衰减学习率
  # 如果初始学习率设置为 1，则学习率如下：
  #   epoch 0: 0.01  - 使用 ConstantLR
  #   epoch 1: 0.01  - 使用 ConstantLR
  #   epoch 2: 1.0   - 使用 ConstantLR
  #   epoch 3: 1.0   - 使用 ConstantLR
  #   epoch 4: 1.0   - 使用 ConstantLR
  #   epoch 5: 1.0   - 使用 StepLR
  #   epoch 6: 1.0   - 使用 StepLR
  #   epoch 7: 0.1   - 使用 StepLR
  #   epoch 8: 0.1   - 使用 StepLR
  scheduler:
    type: SequentialLR
    params:
      schedulers:
      - type: ConstantLR
        params:
          factor: 0.01
          total_iters: 2
      - type: StepLR
        params:
          step_size: 2
          gamma: 0.1
      milestones:
      - 5
```

### 训练设置 (Train Settings)

`train_settings` 键包含特定于训练的参数。有关 `dataloader_workers` 和 `dataloader_pin_memory` 设置的更多信息，请参阅 [单进程和多进程数据加载](
https://pytorch.org/docs/stable/data.html#single-and-multi-process-data-loading) 和 [内存固定 (memory pinning)](
https://pytorch.org/docs/stable/data.html#memory-pinning)。设置 `dataloader_workers: 0` 使用单进程数据加载，而设置为 1 或更多则使用多进程数据加载。在 NVIDIA GPU 上训练时，应始终将 `dataloader_pin_memory: true`。

```yaml
train_settings:
  batch_size: 1  # 用于训练的批次大小
  dataloader_workers: 0  # PyTorch Dataloader 的工作线程数
  dataloader_pin_memory: true  # 固定 DataLoader 内存
  display_iters: 500  # 每次日志打印之间的迭代（步数）数
  epochs: 200  # 模型训练的最大周期数
  seed: 42  # 用于可重现性的随机种子
```

### 日志记录器 (Logger)

默认情况下，训练运行会记录到模型文件夹（存储快照的位置）。

此外，您可以通过添加 `WandbLogger` 将结果记录到 [Weights and Biases](https://wandb.ai/site)。在开始训练运行之前（通过在 shell 中使用 `wandb login`），请确保您已登录到您的 `wandb` 账户。有关更多信息，请参阅他们的[教程](
https://docs.wandb.ai/tutorials)以及 [`wandb.init`](https://docs.wandb.ai/ref/python/init) 的文档。

记录到 `wandb` 是跟踪您执行的操作（包括性能和指标）的好方法。

```yaml
logger:
 type: WandbLogger
 project_name: my-dlc3-project  # 应记录运行所属的项目名称
 run_name: dekr-w32-shuffle0  # 要记录的运行名称
 ...  # 您可以传递给 `wandb.init` 的任何其他参数，例如 `tags: ["dekr", "split=0"]`
```

如果您设置了 `WandbLogger`，相应的运行信息（`entity`, `project`, `run_id`）将保存在模型训练目录中的 `wandb_info.yaml` 文件中，以便以后可以轻松恢复 WandB 运行。

您还可以使用 `image_log_interval` 将模型看到的图像记录到 `wandb`。这会记录一个随机的训练图像和一个测试图像，以及该图像的目标和热图。

### 在特定检查点重启训练

如果您希望在特定检查点重启训练，您可以将检查点的完整路径指定给 `resume_training_from` 变量，如下所示。在此示例中，`snapshot-010.pt` 将在训练开始前加载，模型将从第 10 个周期继续训练。

```yaml
# 模型配置
...
# 从中恢复训练的权重
resume_training_from: /Users/john/dlc-project-2021-06-22/dlc-models-pytorch/iteration-0/dlcJun22-trainset95shuffle0/train/snapshot-010.pt
```

在继续训练模型时，您可能希望修改之前使用的学习率调度（通过编辑 `scheduler` 键下的配置）。执行此操作时，您**必须**在 `runner` 配置中将 `load_scheduler_state_dict: false` 设置为 `false`！否则，将从状态字典加载您开始训练时使用的调度器的参数，并且您所做的编辑可能不会被保留！

### 推理 (Inference)

`pytorch_config.yaml` 中的 `inference:` 块允许配置模型的**特定于推理的行为**。它独立于训练设置，可以包含多个子配置，目前支持**多线程 (multithreading)**、**编译 (compile)**、**自动转换 (autocast)** 和**条件 (conditions)**。

**示例**
```yaml
inference:
  multithreading:
    enabled: true
    queue_length: 4
    timeout: 30.0
  compile:
    enabled: false
    backend: "inductor"
  autocast:
    enabled: false
  conditions:
    config_path: /path/to/model-dir/pytorch_config.yaml
    snapshot_path: /path/to/model-dir/snapshot-best-150.pth
```

**子配置 (Sub-configs)**
- `multithreading`
  控制推理期间用于预处理和批处理的生产者-消费者线程。
  - `enabled` (`bool`): 启用/禁用多线程。
  - `queue_length` (`int`): 预处理和模型预测之间允许排队的批次的最大数量。
  - `timeout` (`float`): 预处理队列的超时时间（秒）。
- `compile`
  控制推理期间可选的 `torch.compile` 用法。
  **注意：** 使用 `torch.compile` 可能会加快推理速度，但会引入一些初始化开销。
  它也已知在某些设置、环境或架构中会失败（例如 `ctd_coam_*` 模型）。
  风险自负。
  - `enabled` (`bool`): 启用/禁用编译。默认值：`false`。
  - `backend` (`str`): 编译时使用的后端（`"inductor"`、`"aot_eager"` 等）。
- `autocast`
  控制推理期间可选的混合精度。
  - `enabled` (`bool`): 启用/禁用 `torch.autocast`。默认值：`false`。
  注意：启用 autocast 可能会降低推理精度。默认禁用。
- `conditions`
  仅用于**条件自顶向下 (CTD)** 模型，用于指定推理期间应使用哪些条件。

## 训练自顶向下模型

自顶向下模型分为两个主要部分：一个检测器（在图像中定位个体）和一个姿态模型（预测每个个体的姿态；一旦定位完成，获得姿态就像在单动物模型中获得姿态一样！）。

模型的“姿态”部分配置与单动物或自底向上模型完全相同（通过 `data`、`model`、`runner` 和 `train_settings` 配置）。检测器则通过配置该文件顶层的 `detector` 键来配置。

### 检测器配置 (Detector Configuration)

训练自顶向下模型时，您还需要配置检测器将如何训练。所有与检测器相关的信息都放在 `detector` 键下。

```yaml
detector:
  data:  # 使用哪些数据增强，选项与姿态模型相同
    colormode: RGB
    inference:  # 检测器的默认推理配置
      normalize_images: true
    train:  # 检测器的默认训练配置
      affine:
        p: 0.9
        rotation: 30
        scaling: [ 0.5, 1.25 ]
        translation: 40
      hflip: true
      normalize_images: true
  model:  # 要训练的检测器
    type: FasterRCNN
    variant: fasterrcnn_mobilenet_v3_large_fpn
    pretrained: true
  runner:  # 检测器训练运行器配置（与姿态模型相同的键）
    type: DetectorTrainingRunner
    ...
  train_settings: # 检测器训练设置（与姿态模型相同的键）
    ...
  resume_training_from: # 可选：从特定检查点重启训练
```

目前，可用的检测器只有 `FasterRCNN` 和 `SSDLite`。但是，`FasterRCNN` 有多种变体（您可以在 [torchvision 的目标检测页面](
https://pytorch.org/vision/stable/models.html#object-detection) 上查看不同的变体）。建议使用能带来足够性能的最快的检测器。推荐的变体如下（从最快到最强大，摘自 torchvision 文档）：

| 名称 | 框 mAP（越大 = 越强大） | 参数（越大 = 越强大） | GFLOPS（越大 = 越慢） |
|---|---|---|---|
| SSDLite | 21.3 | 3.4M | 0.58 |
| fasterrcnn_mobilenet_v3_large_fpn | 32.8 | 19.4M | 4.49 |
| fasterrcnn_resnet50_fpn | 37 | 41.8M | 134.38 |
| fasterrcnn_resnet50_fpn_v2 | 46.7 | 43.7M | 280.37 |


### 在特定检查点重启目标检测器训练

如果您希望在特定检查点重启检测器的训练，可以将检查点的完整路径指定给检测器的 `resume_training_from` 变量，如下所示。在此示例中，`snapshot-detector-020.pt` 将在训练开始前加载，模型将从第 20 个周期继续训练。

```yaml
detector:
  # 检测器配置
  ...
  # 从中恢复训练的权重
  resume_training_from: /Users/john/dlc-project-2021-06-22/dlc-models-pytorch/iteration-0/dlcJun22-trainset95shuffle0/train/snapshot-detector-020.pt
```

在继续训练检测器时，您可能希望修改之前使用的学习率调度（通过编辑 `scheduler` 键下的配置）。执行此操作时，您**必须**在 `detector`: `runner` 配置中将 `load_scheduler_state_dict: false` 设置为 `false`！否则，将从状态字典加载您开始训练时使用的调度器的参数，并且您所做的编辑可能不会被保留！

(bbox-from-pose)=
### 从姿态生成边界框 (Generating Bounding Boxes from Pose)

要训练目标检测模型（用于自顶向下姿态估计），需要真实边界框。由于它们在 DeepLabCut 中没有被标注，因此它们是从真实姿态生成的：只需获取 x 和 y 轴的最小值和最大值，加上一个小的边距，您就得到了边界框！默认设置在姿态周围增加 20 像素的边距。这在大多数情况下都有效，但在某些情况下（例如，图像非常小或非常大时），您应该更新此值。

您可以通过模型 `data: bbox_margin` 参数在 `pytorch_config.yaml` 中为检测器编辑该值：

```yaml
detector:
  data:
    bbox_margin: 20
    ...
```

![Bounding boxes generated from pose with different margins](assets/bboxes_from_kpts.png)
```