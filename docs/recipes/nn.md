```rst
(tf-training-tips-and-tricks)=
# 模型训练技巧与窍门

## TensorFlow Engine：限制 GPU 内存占用

默认情况下，TensorFlow 会将所有 GPU 内存分配给训练过程，这会阻止其他 TensorFlow 进程在同一台机器上运行。

一个灵活的解决方案是调用 `deeplabcut.train(..., allow_growth=True)`，它会根据需要动态增长 GPU 内存区域。另一个更严格的选项是显式地将 GPU 用量限制在可用内存的一部分。例如，分配总内存最大 1/4 的方法如下：

```python
import tensorflow as tf

gpu_options = tf.GPUOptions(per_process_gpu_memory_fraction=0.25)
sess = tf.Session(config=tf.ConfigProto(gpu_options=gpu_options))
```

(tf-custom-image-augmentation)=
## 使用自定义图像增强

图像增强是通过对图像应用各种变换（例如旋转或缩放）来人为地扩展训练集的过程，目的是使模型更健壮、更准确（有关更多信息，请阅读我们的[入门指南](https://www.sciencedirect.com/science/article/pii/S0896627320307170)）。尽管 DeepLabCut 会自动完成数据增强，但在训练之前，可以很容易地覆盖默认值。请参阅以下文件中定义的增强变量：

- PyTorch Engine：[`pytorch_config.yaml` 文件的文档](dlc3-pytorch-config)
- TensorFlow Engine：[默认 `pose_cfg.yaml` 文件](
https://github.com/DeepLabCut/DeepLabCut/blob/main/deeplabcut/pose_cfg.yaml#L23-L74)

对于单动物 TensorFlow 模型，在调用 `create_training_dataset` 时，[您在图像增强方面有多种选择](
https://deeplabcut.github.io/DeepLabCut/docs/standardDeepLabCut_UserGuide.html#f-create-training-dataset-s-and-selection-of-your-neural-network)。

有关图像增强和训练超参数的深入教程，请参见[此处](
https://deeplabcut.github.io/DeepLabCut/docs/recipes/pose_cfg_file_breakdown.html)。

## 评估中间（和所有）快照

训练过程中存储的最新快照不一定能产生最高的性能。因此，您应该分析**所有**快照，并选出最好的一个。为此，请在 `config.yaml` 的 `snapshots` 部分中填入 `'all'`。

(what-neural-network-should-i-use)=
## 我应该使用哪种神经网络？（权衡、速度性能和注意事项）

您在创建训练数据集时始终选择网络类型：即标准 dlc：`deeplabcut.create_training_dataset(config, net_type=resnet_50)`，或 maDLC：
`deeplabcut.create_multianimaltraining_dataset(config, net_type=dlcrnet_ms5)`。除此之外，您不应更改任何内容。

### PyTorch Engine

可在[PyTorch 模型架构](dlc3-architectures) 页面中找到不同可用架构的描述。

### TensorFlow Engine

随着更多网络选项的发布，您现在必须决定使用哪一个！这种额外的灵活性可能很有帮助，但我们希望为您提供一些从哪里开始的指导。

**简而言之**——对于大多数情况，您的最佳性能是 ResNet-50；MobileNetV2-1 速度要快得多，训练时需要的 GPU 内存更少，而且准确率也几乎一样高。

***
### ResNets：

在 Mathis 等人的 2018 年论文中，我们对三种网络进行了基准测试：**ResNet-50、ResNet-101 和 ResNet-101ws**。对于**所有**实验室应用，ResNet-50 就足够了。在 [www.deeplabcut.org](http://www.mousemotorlab.org/deeplabcut) 上的所有演示视频中使用的骨干网络都是 ResNet-50。因此，我们建议将其作为数据分析的常用主力模型。这里有一张来自论文的图表，请看“B”面板（在开放场数据集上，它们之间的误差仅相差几像素）：

<p align="center">
<img src="https://images.squarespace-cdn.com/content/v1/57f6d51c9f74566f55ecf271/1548558406678-S32H6T3M3U7BWVS4IGYD/ke17ZwdGBToddI8pDm48kD4CqqHoJgLzZVYacqX5G8QUqsxRUqqbr1mOJYKfIPR7LoDQ9mXPOjoJoqy81S2I8N_N4V1vUb5AoIIIbLZhVYy7Mythp_T-mtop-vrsUOmeInPi9iDjx9w8K4ZfjXt2dqTB9h4P9po3-YSCqzKkit0PccqviqYX7RTAdOBUgXwbCjLISwBs8eEdxAxTptZAUg/SupplFig2-01.png?format=1000w" width="80%">
</p>

这也是主要结果图之一，使用 ResNet-50 生成。蓝色代表训练——红色代表测试——黑色代表我们最好的人工水平性能，10 像素是老鼠鼻子的宽度——因此，对于我们在这个任务上的表现来说，低于这个值的都是不错的性能！

<p align="center">
<img src="https://images.squarespace-cdn.com/content/v1/57f6d51c9f74566f55ecf271/1547585317499-0QWTWL5KVPK8ZWINQ30U/ke17ZwdGBToddI8pDm48kH23KVWagbNOYpajbj_MQLNZw-zPPgdn4jUwVcJE1ZvWQUxwkmyExglNqGp0IvTJZamWLI2zvYWH8K3-s_4yszcp2ryTI0HqTOaaUohrI8PI-4DGGLi3WdhIPQDa6khDzRWGU5SknjCO3Yd6rloU2Zw/ErrorvsTrainingsetSize.png?format=1000w" width="60%">
</p>

这里还有一些使用 ResNet-50 分析视频的速度统计数据，更多详细信息请参阅 https://www.biorxiv.org/content/early/2018/10/30/457242：

<p align="center">
<img src="https://images.squarespace-cdn.com/content/v1/57f6d51c9f74566f55ecf271/1547585393723-8BQ6RGSPUUEQ1NNGUQDZ/ke17ZwdGBToddI8pDm48kCebzxgICDi_Bmgq_409OyxZw-zPPgdn4jUwVcJE1ZvWQUxwkmyExglNqGp0IvTJZamWLI2zvYWH8K3-s_4yszcp2ryTI0HqTOaaUohrI8PICdDqlshOygx3FUsifuoze123Z0BWMsGmyODBJYiFvQc/inferencespeed.png?format=1000w" width="60%">
</p>

**那么，为什么要使用 ResNet-101 甚至 152 呢？** 如果您面临的挑战性问题更复杂，例如多个人跳舞的场景，那么这是一个不错的选择。此时，您还应在相应混洗文件夹（训练前）的 `pose_config.yaml` 中设置 `intermediate_supervision=True` 以获得最佳性能。请注意，对于 ResNet-50，这**没用**，甚至可能带来负面影响。

### 何时应该使用 MobileNet？

MobileNet 运行速度快、训练速度快、内存效率更高，分析（推理）速度也更快——例如，在 CPU 上快 4 倍，在 GPU 上快 2 倍！因此，如果您没有 GPU（或 GPU 内存很少），并且不想使用 Google COLAB 等服务，那么它们是一个很好的起点。

不过，它们的网络结构更小/更浅，因此您不应向其中输入非常大的图像。所以，请务必对数据使用 `deeplabcut.DownSampleVideo`（坦率地说，这总是一个不错的选择）。

此外，对于在“实时”视频上运行也是一个不错的选择，即如果您想在实验中提供实时反馈，您可以让视频围绕一个较小的裁剪区域运行，并以这种较快的方式运行！

**那么，它们有多快呢？**

这里是 4 种 MobileNetV2 变体与 ResNet-50 和 ResNet-101（最深的红色——在此处阅读更多信息：https://arxiv.org/abs/1909.11229）的速度比较：

<p align="center">
<img src="https://images.squarespace-cdn.com/content/v1/57f6d51c9f74566f55ecf271/1570054128042-51HCY1Y9GV7GAQTZ5BMB/ke17ZwdGBToddI8pDm48kKr5oWkDv6XTQOpQfQOqjiAUqsxRUqqbr1mOJYKfIPR7LoDQ9mXPOjoJoqy81S2I8N_N4V1vUb5AoIIIbLZhVYxCRW4BPu10St3TBAUQYVKchrM-h1v5jGhVgANO1xgMJaHKhYxZ0-Cf0LQLHXkOaBUlIOyXFtu3PNQa47ngsqiu/mbnetv2speed.png?format=1000w" width="100%">
</p>

<p align="center">
<img src="https://images.squarespace-cdn.com/content/v1/57f6d51c9f74566f55ecf271/1570054117297-YA8WOYG50EK55WM6Y8ZI/ke17ZwdGBToddI8pDm48kAWg0301pwdoqO-Bo48aILYUqsxRUqqbr1mOJYKfIPR7LoDQ9mXPOjoJoqy81S2I8N_N4V1vUb5AoIIIbLZhVYxCRW4BPu10St3TBAUQYVKcZbh5EzlyubXk7Q3qHw5ayJHISnXwMOq8Pp90__8eMJefaZFcnumpU7B4DHTHEFkQ/speedtables.png?format=1000w" width="100%">
</p>

### 何时应该使用 EfficientNet？

EfficientNet 与 MobileNets 一样使用**反向残差块**构建，但由于其最优的深度/宽度/分辨率缩放，其性能比 ResNets 更强大，[EfficientNet](https://arxiv.org/abs/1905.11946) 是追求速度和性能的绝佳选择。然而，它们需要更仔细的处理！尤其对于小数据集，您需要调整批大小（batch size）和学习率（learning rates）。因此，我们建议更高级的用户或愿意进行实验以找到最佳设置的用户使用它们。以下是速度比较，有关性能信息，请参阅我们的最新研究：http://horse10.deeplabcut.org

<p align="center">
<img src="https://images.squarespace-cdn.com/content/v1/57f6d51c9f74566f55ecf271/1615891029784-87JAZJN1C5S4HS62F752/ke17ZwdGBToddI8pDm48kLId9V2zDiOqQ5EIZz4b_S0UqsxRUqqbr1mOJYKfIPR7LoDQ9mXPOjoJoqy81S2I8N_N4V1vUb5AoIIIbLZhVYxCRW4BPu10St3TBAUQYVKctCpCjeabgTq1Hv_G9BIks_zjAnmEpAaVGioFPvsrieXDegXGHA0z-h8QeHOQDokM/speedTest.png?format=1000w" width="100%">
</p>

### 如何比较它们？

好问题！比较它们最好的方法是使用**相同**的测试/训练划分（在 `create_training_dataset` 中生成）和不同的模型。在 2.1+ 版本中，我们有了一个**新**的函数可以轻松实现这一点。您将运行 `create_training_model_comparison`，而不是使用 `create_training_dataset`（如需帮助，请查看 `deeplabcut.create_training_model_comparison?` 的文档字符串或运行项目管理器 GUI - `deeplabcut.launch_dlc()`）。
```