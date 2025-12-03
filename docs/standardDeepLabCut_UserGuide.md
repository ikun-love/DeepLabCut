(single-animal-userguide)=
# DeepLabCut 用户指南（针对单动物项目）

本文档涵盖单次/标准的 DeepLabCut 用法。如果您有复杂的多动物场景（即它们看起来很相似），请参阅我们的 [maDLC 用户指南](multi-animal-userguide)。

要开始使用，您可以使用 GUI（图形用户界面）或终端。请参阅下文。

## DeepLabCut 项目管理器 GUI（推荐给初学者）

**GUI：**

要开始，请导航到 Anaconda Prompt 终端，然后右键单击选择“以管理员身份运行”（Windows），或直接启动计算机上的“终端”（Unix/MacOS）。我们假设您已经安装了 DeepLabCut（如果没有，请参阅 [安装文档](how-to-install)！）。接下来，激活您的 conda 环境（例如 `conda activate DEEPLABCUT`）。然后，只需运行 `python -m deeplabcut`。您可以在易于使用的图形用户界面中找到以下功能。虽然大多数功能都可用，但高级用户可能更喜欢命令行界面提供的额外灵活性。请在下面阅读更多内容。
```{Hint}
🚨 如果您使用的是 Windows，请务必使用管理员权限打开终端！右键单击并选择“以管理员身份运行”。
```

<p align="center">
<img src="https://images.squarespace-cdn.com/content/v1/57f6d51c9f74566f55ecf271/1572824438905-QY9XQKZ8LAJZG6BLPWOQ/ke17ZwdGBToddI8pDm48kIIa76w436aRzIF_cdFnEbEUqsxRUqqbr1mOJYKfIPR7LoDQ9mXPOjoJoqy81S2I8N_N4V1vUb5AoIIIbLZhVYxCRW4BPu10St3TBAUQYVKcLthF_aOEGVRewCT7qiippiAuU5PSJ9SSYal26FEts0MmqyMIhpMOn8vJAUvOV4MI/guilaunch.jpg?format=1000w" width="60%">
</p>

提醒一下，核心功能在我们的 [Nature Protocols 论文](https://www.nature.com/articles/s41596-019-0176-0) 中有描述（发表于 2.0.6 版本时）。包中不断添加其他功能和特性。因此，我们建议您阅读该协议，然后查看以下文档和文档字符串。感谢您使用 DeepLabCut！

## DeepLabCut 的终端/命令行界面：

要开始，请导航到 Anaconda Prompt 终端，然后右键单击选择“以管理员身份运行”（Windows），或直接启动计算机上的“终端”（Unix/MacOS）。我们假设您已经安装了 DeepLabCut（如果没有，请参阅 [安装文档](how-to-install)！）。接下来，激活您的 conda 环境（例如 `conda activate DEEPLABCUT`），然后输入 `ipython`。然后输入：
```python
import deeplabcut
```

```{Hint}
🚨 如果您使用的是 Windows，请务必使用管理员权限打开终端！右键单击并选择“以管理员身份运行”。
```

### (A) 创建一个新项目

`create_new_project` 函数会创建一个新的项目目录、必需的子目录以及一个基本的项目配置文件。每个项目都通过项目名称（例如 Reaching）、实验员姓名（例如 YourName）以及创建日期来标识。

因此，此函数要求用户输入项目名称、实验员姓名以及（最初）用于创建训练数据集的视频的完整路径。

可选参数指定项目目录将被创建的工作目录，以及用户是否希望将视频复制（到项目目录）。如果未指定可选参数 `working_directory`，则项目目录将在当前工作目录中创建；如果未指定 `copy_videos`，则在 `videos` 目录中创建视频的符号链接。每个符号链接都创建了到视频的引用，因此无需将整个视频复制到 `videos` 目录中（前提是视频保留在原始位置）。

```python
deeplabcut.create_new_project(
    "项目名称",
    "实验员姓名",
    ["视频1的完整路径", "视频2的完整路径", "视频3的完整路径"],
    working_directory="工作目录的完整路径",
    copy_videos=True/False,
    multianimal=False
)
```

**重要的路径格式说明**

Windows 用户，您必须以下列格式输入路径：`r'C:\Users\computername\Videos\reachingvideo1.avi'` 或 `'C:\\Users\\computername\\Videos\\reachingvideo1.avi'`

提示：您也可以将 `config_path` 放在 `deeplabcut.create_new_project` 前面，以创建一个保存到 config.yaml 文件路径的变量，即 `config_path=deeplabcut.create_new_project(...)`

这组参数将在 **工作目录** 中创建一个名为 **<项目名称>+<实验员姓名>+<项目创建日期>** 的项目目录，并在 **videos** 目录中创建视频的符号链接。项目目录将包含子目录：**dlc-models**、**dlc-models-pytorch**、**labeled-data**、**training-datasets** 和 **videos**。在项目过程中生成的所有输出都将存储在这些子目录之一中，从而允许将每个项目与其他项目分开管理。这些子目录的目的是如下：

**dlc-models** 和 **dlc-models-pytorch** 具有相似的结构；第一个包含 TensorFlow 引擎的文件，而第二个包含 PyTorch 引擎的文件。在这些目录的顶层，有一些目录引用了不同迭代的标签精炼过程（见下文）：**iteration-0**、**iteration-1** 等。迭代目录存储 shuffle（洗牌）目录，每个 shuffle 目录存储与特定实验相关的模型数据：在特定的训练和测试集上进行训练和测试，并使用特定的模型架构。每个 shuffle 目录包含子目录 *test* 和 *train*，每个子目录都保存有关特征检测器参数的元信息配置文件中。配置文件是 YAML 文件，这是一种常见的人类可读数据序列化语言。这些文件可以用标准的文本编辑器打开和编辑。子目录 *train* 将存储模型训练期间的检查点（称为快照）。这些快照允许用户在不重新训练的情况下重新加载训练好的模型，或者在训练中断的情况下从特定的保存检查点继续训练。

**labeled-data:** 此目录存储用于创建训练数据集的帧。来自不同视频的帧存储在以视频文件名命名的单独子目录中。每帧的文件名都与对应视频中的时间索引相关，这使用户能够将每帧追溯到其来源。

**training-datasets:** 此目录将包含用于训练网络的训练数据集和元数据，其中包含有关如何创建训练数据集的信息。

**videos:** 视频链接或视频的目录。当 **copy\_videos** 设置为 `False` 时，此目录包含指向视频的符号链接。如果设置为 `True`，则视频将被复制到此目录。默认为 `False`。此外，如果用户希望在任何阶段向项目中添加新视频，可以使用 **add\_new\_videos** 函数。这将更新项目配置文件中视频列表。

```python
deeplabcut.add_new_videos(
    "项目配置文件路径*",
    ["视频4的完整路径", "视频5的完整路径"],
    copy_videos=True/False
)
```

*请注意，*项目配置文件的完整路径*在整个协议中将引用为 `config_path`。

项目目录还包含名为 *config.yaml* 的主配置文件。*config.yaml* 文件包含项目的许多重要参数。有关参数及其描述的完整列表，请参阅 Box1。

`create_new_project` 步骤将以下参数写入配置文件：*Task*（任务）、*scorer*（评分者）、*date*（日期）、*project\_path* 以及视频列表 *video\_sets*。前三个参数**不应**更改。可以通过添加新视频或手动删除视频来更改视频列表。

![Box 1 - Single Animal Project Configuration File Glossary](images/box1-single.png)

### API 文档
````{admonition} 点击按钮查看 API 文档
:class: dropdown
```{eval-rst}
.. include:: ./api/deeplabcut.create_new_project.rst
```
````

### (B) 配置项目

<img src="https://images.squarespace-cdn.com/content/v1/57f6d51c9f74566f55ecf271/1588892210304-EW7WD46PYAU43WWZS4QZ/ke17ZwdGBToddI8pDm48kAXtGtTuS2U1SVcl-tYMBOAUqsxRUqqbr1mOJYKfIPR7LoDQ9mXPOjoJoqy81S2I8PaoYXhp6HxIwZIk7-Mi3Tsic-L2IOPH3Dwrhl-Ne3Z2YjE9w60pqfeJxDohDRZk1jXSVCSSfcEA7WmgMAGpjTehHAH51QaxKq4KdVMVBxpG/1nktc1kdgq2.jpg?format=1000w" width="175" title="colormaps" alt="DLC Utils" align="right" vspace = "50">

接下来，打开在 **create\_new\_project** 期间创建的 **config.yaml** 文件。您可以使用任何文本编辑器编辑此文件。熟悉参数的含义（Box 1）。您可以编辑各种参数，特别是您**必须添加要跟踪的 *bodyparts*（或兴趣点）的列表**。您还可以在此处设置用于所有下游步骤的 *colormap*（也可以随时编辑），例如标记 GUI、视频等。这里的任何 [matplotlib 颜色映射](https://matplotlib.org/tutorials/colors/colormaps.html) 都可以！请**不要**在 bodypart 名称中使用空格。

**bodyparts:** 是每个个体（在上述列表中）的身体部位。

 ### (C) 选择要标记的帧

**关键：** 良好的训练数据集应包含捕获行为 [**广度**](https://zh.wikipedia.org/wiki/%E5%B9%BF%E5%BA%A6) 的足够数量的帧。理想情况下，这意味着从不同的（行为）会话、不同的光照和不同的动物中选择帧，如果它们有很大差异（以训练出不变的、健壮的特征检测器）。因此，为了创建可重复用于实验室的健壮网络，良好的训练数据集应反映行为在姿势、亮度条件、背景条件、动物身份等方面与要分析的数据的多样性。对于简单的实验室行为，如小鼠伸手（reaching）、开场行为（open-field behavior）和果蝇行为，100-200 帧即可获得良好结果 [Mathis et al, 2018](https://www.nature.com/articles/s41593-018-0209-y)。但是，根据所需的精度、行为的性质、视频质量（例如运动模糊、光照不佳）和环境，可能需要更多或更少的帧来创建良好的网络。最终，为了将分析扩展到可能包含意外情况的大量视频，也可以以自适应方式精炼数据集（参见下文的精炼）。

`extract_frames` 函数从项目配置文件中的所有视频中提取帧，以创建训练数据集。从所有视频中提取的帧存储在 'labeled-data' 下以视频文件名命名的单独子目录中。此函数还具有可能根据用户的需求有用的各种参数。
```python
deeplabcut.extract_frames(
    config_path,
    mode="automatic/manual",
    algo="uniform/kmeans",
    crop=True/False,
    userfeedback=False
)
```
**关键点：** 建议保持帧大小较小，因为大帧会增加训练和推理时间。每个视频的裁剪参数可以在 config.yaml 文件中提供（并在下面查看）。运行 extract_frames 函数时，如果参数 crop=True，系统将要求您在 GUI 中绘制一个框（这将写入 config.yaml 文件）。

`userfeedback` 允许用户指定希望从中提取帧的视频。当设置为 `"True"` 时，将启动一个对话框，系统会询问每个视频是否应提取（额外/任何）帧。例如，如果您已经标记了一些文件夹并希望为新视频提取数据，则使用此选项。

提供的函数要么根据均匀分布（uniform）随机抽样从视频中选择帧，要么根据视觉外观聚类（k-means），要么通过手动选择。对于姿势在整个视频中变化的那些行为，随机均匀选择帧效果最好。然而，某些行为可能是稀疏的，如伸手行为中，伸手和拉拽动作非常快，小鼠在试验之间移动不多。在这种情况下，允许选择基于 k-means 派生量化的帧的函数会很有用。如果用户选择使用 k-means 作为方法对帧进行聚类，那么此函数会下采样视频并使用 k-means 对帧进行聚类，其中每个帧都被视为一个向量。然后选择来自不同簇的帧。此过程确保了帧看起来不同。然而，对于大型和长时间播放的视频，由于计算复杂性，此代码运行缓慢。

**关键点：** 建议从包含有趣行为的视频片段中提取帧，而不是从整个视频中提取。这可以通过在 config.yaml 文件中使用 start 和 stop 参数来实现。此外，用户可以使用 config.yaml 文件中的 numframes2extract 参数更改从每个视频中提取的帧数。

然而，选择帧高度依赖于所研究的数据和行为。因此，很难为每种行为和动物提供通用的代码来提取帧以创建良好的训练数据集。如果用户觉得缺少特定的帧，他们可以使用工具箱提供的交互式 GUI 提取感兴趣的手选帧。通过以下方式启动：
```python
deeplabcut.extract_frames(config_path, "manual")
```
用户可以使用 *Load Video* 按钮加载项目配置文件中的一个视频，使用滚动条导航穿过视频，然后使用 *Grab a Frame*（或从 2.0.5 版本开始的帧范围）来提取帧。用户还可以查看提取的帧，例如在重新加载集合之前删除太相似的帧，然后手动注释它们。

<p align="center">
<img src="https://static1.squarespace.com/static/57f6d51c9f74566f55ecf271/t/5c71bfbc71c10b4a23d20567/1550958540700/cropMANUAL.gif?format=750w" width="70%">
</p>

### API 文档
````{admonition} 点击按钮查看 API 文档
:class: dropdown
```{eval-rst}
.. include:: ./api/deeplabcut.extract_frames.rst
```
````

### (D) 标记帧

工具箱提供了一个 **label\_frames** 函数，它可以帮助用户使用交互式图形用户界面 (GUI) 轻松标记所有提取的帧。用户应该已经在项目配置文件中通过提供列表来命名要标记的 bodyparts（兴趣点）。以下命令会调用 napari-deeplabcut 标记 GUI。有关标记工作流程的更多信息，请查看 [napari-deeplabcut 文档](napari-gui)。

```python
deeplabcut.label_frames(config_path)
```

[🎥 演示](https://youtu.be/hsA9IB5r73E)

标记 GUI 中的热键（也请参阅 GUI 中的“帮助”）：

```
Ctrl + C: 复制上一帧的标签。
键盘箭头键：前进/后退帧。
Delete 键：删除标签。
```

![hot keys](https://images.squarespace-cdn.com/content/v1/57f6d51c9f74566f55ecf271/c192345a5-e411-4d56-b718-ef52f91e195e/Qwerty.png?format=2500w)

**关键点：** 建议**一致地标记相似的位置**（例如，在一个非常大的手腕上，尝试标记相同的位置）。通常，用户不应标记看不见或被遮挡的点。只需在帧上不应用标签即可跳过它们。

可选：如果要向现有标记数据集添加更多标签，用户需要将新标签追加到 config.yaml 文件中的 bodyparts 列表中。之后，用户可以调用 **label\_frames** 函数。从 2.0.5+ 开始：将弹出一个框，询问用户是想显示所有部分，还是只添加新标签。在标记完所有图像后保存标签将把新标签追加到现有的标记数据集中。

有关更多信息，请查看 [napari-deeplabcut 文档](napari-gui)，了解有关标记工作流程的更多信息。

### (E) 检查已注释的帧

可选：检查标签是否已创建并正确存储对于训练非常有益，因为标记是创建训练数据集最关键的部分之一。DeepLabCut 工具箱提供了一个 `check_labels` 函数来完成此操作。其用法如下：
```python
deeplabcut.check_labels(config_path, visualizeindividuals=True/False)
 ```

对于 labeled-data 中每个视频目录，此函数都会创建一个后缀为 **labeled** 的子目录。这些目录包含带有注释的身体部位的帧图。用户可以仔细检查身体部位是否已正确标记。如果它们不正确，用户可以重新加载帧（即 `deeplabcut.label_frames`），移动它们，然后再次单击保存。

### API 文档
````{admonition} 点击按钮查看 API 文档
:class: dropdown
```{eval-rst}
.. include:: ./api/deeplabcut.check_labels.rst
```
````

(create-training-dataset)=
### (F) 创建训练数据集

**关键点：** 仅在您将要训练网络**的位置**运行此步骤。如果您在笔记本电脑上标记，但将项目文件夹移动到 Google Colab 或 AWS、实验室服务器等地，请在该平台上运行以下步骤！如果您在 Windows 机器上标记，但在 Linux 上训练，这是可以的，从 2.0.4 版本开始会自动完成（它会为您同时保存 Linux 和 Windows 的文件集）。

- 如果移动项目文件夹，您只需要更改主 config.yaml 文件中的 `project_path`（这是自动完成的）——仅此而已——无需更改视频路径等！您的项目是完全可移植的。

- 请注意，您在此阶段选择神经网络骨干网。从 DLC3+ 开始，我们支持 PyTorch（也支持 TensorFlow，但它将被淘汰）。

**概述：** 此函数将所有视频中的标记数据集组合起来，并将其拆分以创建 train (训练) 和 test (测试) 数据集。训练数据将用于训练网络，而测试数据集将用于评估网络性能。

```python
deeplabcut.create_training_dataset(config_path)
```

- 可选：如果用户希望对 DeepLabCut 的性能进行基准测试，他们可以通过向 `num_shuffles` 指定一个整数值来创建多个训练数据集；有关更多详细信息，请参阅文档字符串。

该函数在当前 “iteration” 目录中 **dlc-models-pytorch** 目录（如果使用 TensorFlow，则为 **dlc-models**）中创建一个新的 shuffle（洗牌）目录。*train* 和 *test* 目录各有不同的配置文件（对于 Pytorch 模型，*train* 中是 **pytorch\_config.yaml**，*test* 中是 **pose\_cfg.yaml**；对于 Tensorflow 模型，*train* 和 *test* 中都是 **pose\_cfg.yaml**）。具体来说，用户可以在开始训练之前编辑 *train* 子目录中的 **pytorch\_config.yaml**（或 **pose\_cfg.yaml**）。这些配置文件包含有关特征检测器参数的元信息。有关 **pytorch\_config.yaml** 文件的更多信息，请参阅 [此处](dlc3-pytorch-config)（对于基于 TensorFlow 的模型，请参阅关键参数 [此处](https://github.com/DeepLabCut/DeepLabCut/blob/main/deeplabcut/pose_cfg.yaml)）。

**关键点：** 在此步骤中，对于 **create\_training\_dataset**，您选择要使用的网络以及任何额外的数据增强（超出我们的默认设置）。调用函数时，您可以设置 `net_type`、`detector_type`（如果使用检测器）和 `augmenter_type`。

- 网络：将下载 ImageNet 预训练网络或 SuperAnimal 预训练网络的权重。您可以选择进行迁移学习（推荐）或“微调”（fine-tune）骨干网和解码器头。我们建议参阅我们的 [关于模型的专用文档](dlc3-architectures) 以获取更多信息（或 [本页面关于选择模型](what-neural-network-should-i-use) 的 TensorFlow 引擎）。

```{Hint}
🚨 如果它们没有下载（您将在终端中看到正在下载），则您可能没有权限执行此操作——请确保“以管理员身份”打开终端（我们只在某些 Windows 用户中看到过这种情况——请参阅 **[文档以获得更多帮助!](tf-training-tips-and-tricks)**）。
```

**数据增强：** 在此阶段，您还可以决定使用哪种类型的增强。调用 `create_training_dataset` 后，您可以编辑创建的 [**pytorch\_config.yaml**](dlc3-pytorch-config) 文件（对于 TensorFlow 引擎，则是 [**pose\_cfg.yaml**](
https://github.com/DeepLabCut/DeepLabCut/blob/main/deeplabcut/pose_cfg.yaml) 文件）。

- PyTorch 引擎：[Albumentations](https://albumentations.ai/docs/) 用于数据增强。有关图像增强选项，请查看 [**pytorch\_config.yaml**](dlc3-pytorch-config)。
- TensorFlow 引擎：默认增强方案适用于大多数任务（如 www.deeplabcut.org 上所示），但有许多可用选项、更多数据增强、中间监督等。这里是可用的加载器：
  - `imgaug`：许多增强可能性，高效的目标图创建代码和支持的 batch size >1。您可以在训练模型的 `pose_cfg.yaml` 文件中设置 `batch_size` 等参数。这是推荐的默认设置！
  - `crop_scale`：我们在 Nature Protocols 中介绍的 DLC 2.0 标准变体（缩放、自动裁剪增强）。
  - `tensorpack`：许多增强可能性，多 CPU 支持以实现快速处理，目标图的创建效率低于 imgaug，不支持 batch size>1。
  - `deterministic`：仅在测试时有用，冻结 numpy 种子；否则与默认设置相同。

**模型比较**：您还可以通过为不同网络创建相同的 train/test 拆分来测试多个模型。您可以在项目管理器 GUI 中轻松完成此操作（选择“使用现有数据分割”选项），它还允许您比较 PyTorch 和 TensorFlow 模型。

````{versionadded} 3.0.0
您现在可以使用 `create_training_dataset_from_existing_split` 使用与现有 shuffle 相同的数据分割来创建新的 shuffle，这允许您（在不同架构之间或使用不同训练超参数时）比较模型性能，因为 shuffle 是在相同数据上训练的，并在相同测试数据上评估的！

示例用法 - 使用与 shuffle 0 相同的数据分割，为 ResNet 50 姿态估计模型创建 3 个新的 shuffle（索引为 10、11 和 12）：

```python
deeplabcut.create_training_dataset_from_existing_split(
    config_path,
    from_shuffle=0,
    shuffles=[10, 11, 12],
    net_type="resnet_50",
)
```
````

````{admonition} 点击按钮查看 deeplabcut.create_training_dataset 的 API 文档
:class: dropdown
```{eval-rst}
.. include:: ./api/deeplabcut.create_training_dataset.rst
```
````

````{admonition} 点击按钮查看 deeplabcut.create_training_model_comparison 的 API 文档
:class: dropdown
```{eval-rst}
.. include:: ./api/deeplabcut.create_training_model_comparison.rst
```
````

````{admonition} 点击按钮查看 deeplabcut.create_training_dataset_from_existing_split 的 API 文档
:class: dropdown
```{eval-rst}
.. include:: ./api/deeplabcut.create_training_dataset_from_existing_split.rst
```
````

### (G) 训练网络

`train_network` 函数帮助用户训练网络。其用法如下：
```python
deeplabcut.train_network(config_path)
```
函数中的参数集开始为为一个特定的 shuffle 创建的数据集训练网络。请注意，在开始训练之前，您可以更改模型 [**pytorch\_config.yaml**](dlc3-pytorch-config) 文件（或 TensorFlow 模型的 **pose\_cfg.yaml**）中的训练参数。

在用户指定的迭代期间，检查点存储在相应迭代和 shuffle 目录下的 *train* 子目录中。

````{admonition} 关于使用 PyTorch 引擎训练模型的提示
:class: dropdown

示例参数调用：

```python
deeplabcut.train_network(
    config_path,
    shuffle=1,
    trainingsetindex=0,
    device="cuda:0",
    max_snapshots_to_keep=5,
    displayiters=100,
    save_epochs=5,
    epochs=200,
)
```

DeepLabCut 3.0 中的 Pytorch 模型是为固定数量的 epoch 进行训练的，而不是像 TensorFlow 模型那样使用最大迭代次数。一个 epoch 是对训练数据集的单次遍历，这意味着您的模型恰好看到了一次每张训练图像。因此，如果您有 64 张训练图像，一个 epoch 就是 64 次迭代（批次大小为 1，或批次大小为 2 时为 32 次迭代，批次大小为 4 时为 16 次迭代，依此类推）。默认情况下，预训练网络不在 DeepLabCut 工具箱中（因为它们可能超过 100MB），但在您训练之前会自动下载它们。

如果用户希望从特定检查点重新开始训练，他们可以在 *train* 子目录下的 [**pytorch\_config.yaml**](dlc3-pytorch-config) 文件中将检查点的完整路径指定给变量 ``resume\_training\_from``（请查看文档的“从特定检查点重新开始训练”部分）。

**关键点：** 建议**训练网络直到损失平稳**（根据数据集、模型架构和训练超参数的不同，这种情况通常发生在 100 到 250 个 epoch 训练之后）。

[**pytorch\_config.yaml**](dlc3-pytorch-config) 文件中的变量 ``display\_iters`` 和 ``save\_epochs`` 允许用户更改丢失的显示频率和权重的存储频率。我们建议每 5 到 25 个 epoch 保存一次。
````

````{admonition} 关于使用 TensorFlow 引擎训练模型的提示
:class: dropdown

示例参数调用：

```python
deeplabcut.train_network(
    config_path,
    shuffle=1,
    trainingsetindex=0,
    gputouse=None,
    max_snapshots_to_keep=5,
    autotune=False,
    displayiters=100,
    saveiters=25000,
    maxiters=300000,
    allow_growth=True,
)
```

默认情况下，预训练网络不在 DeepLabCut 工具箱中（因为它们大约各 100MB），但在您训练之前它们会被自动下载。但是，如果尚未从 TensorFlow 模型权重下载，它将下载并存储在 *Pose\_Estimation\_Tensorflow* 的 *models* 子目录下的 *pre-trained* 子目录中。在用户指定的迭代期间，检查点将存储在相应迭代目录下的 *train* 子目录中。

如果用户希望从特定检查点重新开始训练，他们可以在 *train* 子目录下的 **pose\_cfg.yaml** 文件中将检查点的完整路径指定给变量 ``init\_weights``（见 Box 2）。

**关键点：** 如果您使用批次大小 1，建议训练网络数千次迭代，直到损失平稳（通常在 **500,000** 次左右）。如果您想进行批次训练，我们推荐使用 Adam，[参见此处](tf-custom-image-augmentation)。

**pose\_cfg.yaml** 文件中的变量 ``display\_iters`` 和 ``save\_iters`` 允许用户更改损失的显示频率和权重的存储频率。

**maDeepLabCut 关键点：** 对于多动物项目，我们不仅使用不同和新的输出层，还使用新的数据增强、优化、学习率和批次训练默认设置。因此，请使用较低的 ``save\_iters`` 和 ``maxiters``。也就是说，我们建议每 10K-15K 次迭代保存一次，并且仅训练到 50K-100K 次迭代。我们建议仔细查看损失，以免在数据上过度拟合。好处是，训练时间会短得多！！！
````

````{admonition} 点击按钮查看 train\_network 的 API 文档
:class: dropdown
```{eval-rst}
.. include:: ./api/deeplabcut.train_network.rst
```
````

### (H) 评估训练好的网络

评估训练好的网络的性能非常重要。此性能通过计算手动标签与 DeepLabCut 预测标签之间的平均均方根误差 (RMSE) 来衡量。RMSE 被保存为一个逗号分隔的文件，并针对所有对（以及仅针对可能的对 > p-cutoff）显示。这有助于排除（例如）被遮挡的身体部位。DeepLabCut 的一个优势在于，由于其得分图的概率性输出，如果训练充分，它还可以可靠地报告身体部位在给定帧中是否可见。（参见 Mathis 等人在 [2018 年的论文](https://www.nature.com/articles/s41593-018-0209-y) 中关于伸手动作中指尖和果蝇腿 3D 行为的讨论）。通过输入以下命令计算评估结果：

```python
deeplabcut.evaluate_network(config_path, Shuffles=[1], plotting=True)
```

将 `plotting` 设置为 `True` 会绘制在训练和测试图像上带有手动和预测标签的结果。用户应在目录 ‘evaluation-results’ 中视觉检查创建的标记的测试（和训练）图像。理想情况下，DeepLabCut 应根据用户要求的精度标记未见过的（测试）图像，并且平均训练和测试误差应具有可比性（良好的泛化能力）。什么（在数值上）构成可接受的 RMSE 取决于许多因素（包括被跟踪身体部分的大小、标记的可变性等）。请注意，测试误差可能大于训练误差，这可能是由于人为可变性（在标记中，参见 Mathis 等人在 Nature Neuroscience 2018 中的图 2）。

**可选参数：**

- `Shuffles: list, optional` - 指定训练数据集的 shuffle 索引的整数列表。默认为 [1]

- `plotting: bool, optional` - 在训练和测试图像上绘制预测结果。默认为 `False`；如果提供，则必须为 `True` 或 `False`

- `show_errors: bool, optional` - 显示训练和测试误差。默认为 `True`

- `comparisonbodyparts: list of bodyparts, Default is all` - 仅对这些身体部位计算平均误差（必须是身体部位的子集）。

- `gputouse: int, optional` - 表示您的 GPU 编号的自然数（参见 nvidia-smi 中的数字）。如果您没有 GPU，请设置为 None。请参阅：https://nvidia.custhelp.com/app/answers/detail/a_id/3751/~/useful-nvidia-smi-queries

- `pcutoff: float | list[float] | dict[str, float], optional`
（仅在 PyTorch 引擎适用时。对于 TensorFlow，请在 `config.yaml` 文件中设置 `pcutoff`。）
指定用于计算评估指标的截止值。
  - 如果为 `None`（默认值），则从项目配置中加载截止值。
  - 要将单个截止值应用于所有 bodypart，请提供一个 `float`。
  - 要为每个 bodypart 指定不同的截止值，请提供以下之一：
    - 一个 `list[float]`: 每个 bodypart 一个值，如果适用，则为每个唯一 bodypart 附加一个值。
    - 一个 `dict[str, float]`: 键是 bodypart 名称，值是相应的截止值。
如果 bodypart 不在提供的字典中，则该 bodypart 将使用默认的 `pcutoff` 值 `0.6`。

可以通过编辑 **config.yaml** 文件来自定义绘图（例如，可以修改标签的颜色映射、比例、标记大小 (dotsize) 和透明度 (alphavalue)）。默认情况下，每个身体部位都以不同的颜色绘制（由颜色映射控制），并且绘图标签指示其来源。请注意，默认情况下，人类标签绘制为加号（‘+’），DeepLabCut 的预测结果绘制为 ‘.’（对于似然度 > p-cutoff 的置信预测）和 ‘x’（对于似然度 <= `pcutoff`）。

每个训练数据集 shuffle 的评估结果存储在项目目录中新创建的目录 ‘evaluation-results-pytorch’（Tensorflow 模型为 ‘evaluation-results’）中的唯一子目录中。用户可以目视检查标记和预测的身体部位之间的距离是否可接受。在与相同训练数据集的不同 shuffle 进行基准测试的情况下，用户可以提供多个 shuffle 索引来评估相应的网络。
请注意，对于多动物项目，还会在此目录中存储额外的、跨动物或身体部位汇总的距离统计数据。这旨在在动物跟踪之前对多动物的预测性能提供更精细的定量评估。如果泛化能力不足，用户可能需要：

• 检查标签是否正确导入；即不可见点未被标记，兴趣点被准确标记

• 确保损失已经收敛

• 考虑标记更多图像并创建另一次迭代的训练数据集

**可选：** 您还可以绘制得分图（scoremaps）、locref 层和 PAF（姿态对齐特征）：

```python
deeplabcut.extract_save_all_maps(config_path, shuffle=shuffle, Indices=[0, 5])
```
您可以删除 "Indices" 以对所有训练/测试图像运行此操作（这很慢！）

### API 文档
````{admonition} 点击按钮查看 API 文档
:class: dropdown
```{eval-rst}
.. include:: ./api/deeplabcut.evaluate_network.rst
```
````

### (I) 分析新视频

训练好的网络可用于分析新视频。新视频**不必**在配置文件中！
您可以通过简单地使用以下代码行随时分析新视频：
```python
deeplabcut.analyze_videos(
    config_path, ["analysis/project/videos/reachingvideo1.avi的完整路径"],
    save_as_csv=True
)
```
还有其他几个可选输入，例如：
```python
deeplabcut.analyze_videos(
    config_path,
    videos,
    videotype="avi",
    shuffle=1,
    trainingsetindex=0,
    gputouse=None,
    save_as_csv=False,
    destfolder=None,
    dynamic=(True, .5, 10)
)
```
用户可以选择用于分析视频的检查点。为此，用户可以将对应检查点的索引输入到 config.yaml 文件中的 `snapshotindex` 变量中。默认情况下，使用最新的检查点（即最后一个）来分析视频。
标签存储在一个多索引 [Pandas](http://pandas.pydata.org) 数组中，其中包含网络名称、身体部位名称、（像素中的 x, y）标签位置以及每帧每个身体部位的似然度。这些数组以高效的 HDF（分层数据格式）存储在视频所在的目录中。但是，如果 `save_as_csv` 标志设置为 `True`，数据也可以导出为逗号分隔值格式 (.csv)，然后可以导入到许多程序中，例如 MATLAB、R、Prism 等；此标志默认设置为 `False`。您还可以通过传递希望写入的文件夹路径来设置目标文件夹（`destfolder`）。

### API 文档
````{admonition} 点击按钮查看 API 文档
:class: dropdown
```{eval-rst}
.. include:: ./api/deeplabcut.analyze_videos.rst
```
````

### 新视频分析：额外功能

### 视频的动态裁剪：

从 2.1+ 开始，我们有了动态裁剪选项。即，如果您有大帧而动物/物体只占据较小的比例，您可以围绕动物/物体裁剪以加快处理速度。例如，如果您有一个大的开场实验但只跟踪小鼠，这将加快您的分析速度（也有助于实时应用）。要使用此功能，只需在调用 `analyze_videos` 时添加 `dynamic=(True,.5,10)`。

```python
dynamic: 包含 (状态, 检测阈值, 边距) 的三元组

    如果状态为 true，则执行动态裁剪。
    这意味着如果检测到一个物体（即任何身体部位 > detectiontreshold），
    则根据所有身体部位的最小/最大 x 位置和最小/最大 y 位置计算物体边界。此窗口会通过 margin 扩展，然后只分析此裁剪窗口内的姿态（直到物体丢失；即 < detectiontreshold）。当前位置用于更新下一个帧的裁剪窗口（这就是为什么 margin 很重要，并且应设置得足够大以适应动物的移动）。
```

### (J) 过滤姿态数据

您还可以使用中值滤波器（默认）或 [SARIMAX 模型](https://www.statsmodels.org/dev/generated/statsmodels.tsa.statespace.sarimax.SARIMAX.html) 过滤预测结果。这会创建一个新的 .h5 文件，其后缀为 *_filtered*，您可以在 create\_labeled\_data 中使用或用于绘制轨迹。
```python
deeplabcut.filterpredictions(
    config_path,
    ["analysis/project/videos/reachingvideo1.avi的完整路径"]
)
```
一个调用示例：
```python
deeplabcut.filterpredictions(
    config_path,
    ["analysis/project/videos"],
    videotype=".mp4",
    filtertype="arima",
    ARdegree=5,
    MAdegree=2
)
```
您可以修改并传入的参数如下：
```python
deeplabcut.filterpredictions(
    config_path,
    ["analysis/project/videos/reachingvideo1.avi的完整路径"],
    shuffle=1,
    trainingsetindex=0,
    filtertype="arima",
    p_bound=0.01,
    ARdegree=3,
    MAdegree=1,
    alpha=0.01
)
```
下面是一个如何将此应用于视频的示例：

 <p align="center">
<img src="https://static1.squarespace.com/static/57f6d51c9f74566f55ecf271/t/5ccc8b8ae6e8df000100a995/1556908943893/filter_example-01.png?format=1000w" width="70%">
</p>

### API 文档
````{admonition} 点击按钮查看 API 文档
:class: dropdown
```{eval-rst}
.. include:: ./api/deeplabcut.filterpredictions.rst
```
````

### (K) 绘制轨迹

该工具箱的绘图组件利用 matplotlib。因此，最终用户可以通过修改这些图表轻松自定义这些图表。我们还提供了一个函数来绘制分析视频中提取的姿态的轨迹，可以通过输入以下命令调用：

```
deeplabcut.plot_trajectories(config_path, [‘analysis/project/videos/reachingvideo1.avi的完整路径’])
```

它会在视频目录中创建一个名为 `plot-poses` 的文件夹。这些图表显示身体部位的坐标随时间的变化、似然度随时间的变化、身体部位的 x-与 y-坐标，以及连续坐标差的直方图。这些图表有助于用户快速评估视频的跟踪性能。理想情况下，似然度应保持高位，并且连续坐标差的直方图值应接近零（即帧间检测到的身体部位没有跳跃）。以下是演示视频（左侧）上的示例绘图输出：

<p align="center">
<img src="https://images.squarespace-cdn.com/content/v1/57f6d51c9f74566f55ecf271/1559946148685-WHDO5IG9MMCHU0T7RC62/ke17ZwdGBToddI8pDm48kEOb1vFO6oRDmR8SXh4iL21Zw-zPPgdn4jUwVcJE1ZvWEtT5uBSRWt4vQZAgTJucoTqqXjS3CfNDSuuf31e0tVG1gXK66ltnjKh4U2immgm7AVAdfOWODmXNLQLqbLRZ2DqWIIaSPh2v08GbKqpiV54/file0289.png?format=500w" height="240">
<img src="https://images.squarespace-cdn.com/content/v1/57f6d51c9f74566f55ecf271/1559939762886-CCB0R107I2HXAHZLHECP/ke17ZwdGBToddI8pDm48kNeA8e5AnyMqj80u4_mB0hV7gQa3H78H3Y0txjaiv_0fDoOvxcdMmMKkDsyUqMSsMWxHk725yiiHCCLfrh8O1z5QPOohDIaIeljMHgDF5CVlOqpeNLcJ80NK65_fV7S1UcpboONgOQYHLzaUWEI1Ir9fXt7Ehyn7DSgU3GCReAA-ZDqXZYzu2fuaodM4POSZ4w/plot_poses-01.png?format=1000w" height="250">
</p>

### API 文档
````{admonition} 点击按钮查看 API 文档
:class: dropdown
```{eval-rst}
.. include:: ./api/deeplabcut.plot_trajectories.rst
```
````

### (L) 创建标记视频

此外，该工具箱还提供了一个函数，用于通过在帧上绘制标签并创建视频来根据提取的姿态创建标记视频。创建视频有两种模式：快速（FAST）和慢速（SLOW，但质量更高！）。可以使用以下命令创建多个标记视频：
```python
deeplabcut.create_labeled_video(
    config_path,
    ["analysis/project/videos/reachingvideo1.avi的完整路径",
     "analysis/project/videos/reachingvideo2.avi的完整路径"],
    save_frames = True/False
)
```
 可选地，如果您想对一个视频或一​​系列过滤后的视频使用过滤后的数据，请传递 `filtered=True`，即：
```python
deeplabcut.create_labeled_video(
    config_path,
    ["一个视频文件夹的完整路径"],
    videotype=".mp4",
    filtered=True
)
```
您还可以选择添加一个骨架（skeleton）来连接点和/或添加一个点的历史记录用于可视化。要设置“拖尾点”（trailing points），您需要传递 `trailpoints`：
```python
deeplabcut.create_labeled_video(
    config_path,
    ["一个视频文件夹的完整路径"],
    videotype=".mp4",
    trailpoints=10
)
```
要绘制骨架，您需要首先在 `config.yaml` 文件中定义连接节点的对，并设置骨架颜色（也在 `config.yaml` 文件中）。还有一个 GUI 可以帮助您完成此操作，通过调用 `deeplabcut.SkeletonBuilder(configpath)` 来使用！

以下是 `config.yaml` 添加/编辑的示例（例如，在我们提供的 Openfield 演示数据上）：
```yaml
# 绘图配置
skeleton:
  - ["snout", "leftear"]
  - ["snout", "rightear"]
  - ["leftear", "tailbase"]
  - ["leftear", "rightear"]
  - ["rightear", "tailbase"]
skeleton_color: white
pcutoff: 0.4
dotsize: 4
alphavalue: 0.5
colormap: jet
```
然后使用命令传递 `draw_skeleton=True`：
```python
deeplabcut.create_labeled_video(
    config_path,
    ["一个视频文件夹的完整路径"],
    videotype=".mp4",
    draw_skeleton=True
)
```

**新功能** (2.2b8)：您可以创建一个只绘制“点”的视频，即采用 [Johansson 的风格](https://link.springer.com/article/10.1007/BF00309043)，通过传递 `keypoints_only=True`：

```python
deeplabcut.create_labeled_video(
    config_path,["一个视频文件夹的完整路径"],
    videotype=".mp4",
    keypoints_only=True
)
```

**专业提示：** 当传递 `fastmode=False` 时，创建的**视频质量最佳**。因此，当使用 `trailpoints` 和 `draw_skeleton` 时，我们**强烈**建议您同时传递 `fastmode=False`！

 <p align="center">
<img src="https://images.squarespace-cdn.com/content/v1/57f6d51c9f74566f55ecf271/1559935526258-KFYZC8BDHK01ZIDPNVIX/ke17ZwdGBToddI8pDm48kJbosy0LGK_KqcAZRQ_Qph1Zw-zPPgdn4jUwVcJE1ZvWQUxwkmyExglNqGp0IvTJZUJFbgE-7XRK3dMEBRBhUpzkC6kmM1CbNgeHQVxASNv0wiXikHv274BIFe4LR7nd1rKmAka4uxYMJ9FupazBoaU/mouse_skel_trail.gif?format=750w" width="40%">
</p>

此函数还有其他各种参数，特别是用户可以在 **config.yaml** 文件中设置标签的 `colormap`、`dotsize` 和 `alphavalue`。

### API 文档
````{admonition} 点击按钮查看 API 文档
:class: dropdown
```{eval-rst}
.. include:: ./api/deeplabcut.create_labeled_video.rst
```
````

### 提取“骨架”特征：

新功能 (2.0.7+): 您可以保存应用于 `create_labeled_videos` 中的“骨架”以进行更多计算。即，它会提取在 **config.yaml** 文件中定义的每个“骨骼”的长度和方向。您可以通过以下方式使用该函数：

```python
deeplabcut.analyzeskeleton(
    config,
    video,
    videotype="avi",
    shuffle=1,
    trainingsetindex=0,
    save_as_csv=False,
    destfolder=None
)
```

### API 文档
````{admonition} 点击按钮查看 API 文档
:class: dropdown
```{eval-rst}
.. include:: ./api/deeplabcut.analyzeskeleton.rst
```
````

(active-learning)=
### (M) 可选的主动学习 -> 网络精炼：提取离群帧

虽然 DeepLabCut 通常在不同数据集上表现良好，但人们可能希望优化其在各种、可能意外情况下的性能。为了泛化到大型数据集，可以提取标记性能不足的图像，通过调整标签来手动更正它们以扩大训练集，并迭代地改进特征检测器。这种主动学习框架可用于以最小的标记成本（Mathis 等人在 2018 年的论文中讨论）达到预定的置信度水平。然后，由于构成特征检测器的神经网络容量很大，可以继续使用这些附加示例训练网络。人们不一定需要更正所有错误，因为常见的错误可以通过重新标记少量示例然后重新训练来消除。理论上，由于分析的视频没有真实情况数据，找到推定的“离群帧”具有挑战性。但是，可以使用启发式方法，例如身体部位轨迹的连续性，来识别解码器可能出现大错误的图像。

对于特定视频，可以通过输入以下命令完成所有这些操作（参见下面的其他可选输入）：

```python
deeplabcut.extract_outlier_frames(config_path, ["videofile_path"])
```

我们为此目的提供了各种帧选择方法。特别是，用户可以设置：

```
outlieralgorithm: "fitting", "jump", 或 "uncertain"
```
• `outlieralgorithm="uncertain"`：如果特定或所有身体部位的似然度低于 `p_bound`，则选择帧（请注意，这也可能是由于遮挡而不是错误）。

• `outlieralgorithm="jump"`：如果特定或所有身体部位比上一帧跳跃了超过 `epsilon` 像素，则选择帧。

• `outlieralgorithm="fitting"`：如果预测的身体部位位置与对各个身体部位时间序列进行拟合的状态空间模型存在偏差，则选择帧。具体来说，此方法将自回归积分移动平均 (ARIMA) 模型拟合到每个身体部位的时间序列。在此过程中，会将每个似然度小于 `p_bound` 的身体部位检测视为缺失数据。然后，将平均身体部位估计值与拟合值相差至少 `epsilon` 像素的时间点识别为推定的离群帧。此方法的参数是 `epsilon`、`p_bound`、ARIMA 参数以及要平均的身体部位列表（也可以是 `all`）。

• `outlieralgorithm="manual"`：根据用户的目视检查手动选择离群帧。

作为一个例子：
```python
deeplabcut.extract_outlier_frames(config_path, ["videofile_path"], outlieralgorithm="manual")
```

通常，根据参数，这些方法可能会返回比用户想要提取的帧多得多的帧 (`numframes2pick`)。因此，此列表随后用于通过从该列表中随机抽样（`extractionalgorithm="uniform"`）或对相应的帧执行 `extractionalgorithm="kmeans"` 聚类来选择离群帧。

在自动配置中，在发生帧选择之前，系统会告知用户满足标准的帧数量，并询问是否应继续选择此选择。此步骤允许用户首先更改帧选择启发式方法的参数（即确保没有太多帧符合条件）。用户可以迭代地运行 `extract_outlier_frames` 方法，甚至从同一视频中提取额外的帧。提取了足够的离群帧后，可以使用精炼 GUI 根据用户反馈调整标签（见下文）。

### API 文档
````{admonition} 点击按钮查看 API 文档
:class: dropdown
```{eval-rst}
.. include:: ./api/deeplabcut.extract_outlier_frames.rst
```
````

 ### (N) 精炼标签：训练数据集的增强

 根据 DeepLabCut 的性能，可能出现四种情况：

(A) 可见的身体部位具有准确的 DeepLabCut 预测。这些标签不需要任何修改。

(B) 可见的身体部位但 DeepLabCut 预测错误。将标签位置移动到身体部位的实际位置。

(C) 不可见、被遮挡的身体部位。使用鼠标中键删除 DeepLabCut 预测的标签。所有预测的标签都会显示，即使 DeepLabCut 不确定。这是必要的，以便用户可以移动预测的标签。但是，为了帮助用户删除所有不可见的身体部位，低似然度的预测显示为实心圆圈（而不是圆盘）。

(D) 无效图像：如果出现无效图像（极不可能），用户应删除该图像及其相应的预测（如果有）。在这里，GUI 会提示用户删除被识别为无效的图像。

可以打开 GUI 来精炼提取的推定的离群帧的标签：
```python
deeplabcut.refine_labels(config_path)
```
这将启动一个 GUI，用户可以在其中精炼标签。

有关标记工作流程的更多信息，请参阅 [napari-deeplabcut 文档](napari-gui)。

修正了每个子目录中所有帧的标签后，用户应使用数据集来创建新数据集。在此步骤中，config.yaml 文件中的 iteration 参数会自动更新。
```python
deeplabcut.merge_datasets(config_path)
```
一旦数据集合并，用户可以通过绘制所有标签（步骤 E）来测试合并过程是否成功。接下来，使用这个扩展后的训练集，用户可以创建一个新的训练集并按步骤 F 和 G 中所述训练网络。训练数据集将存储在与之前相同的位置，但在项目配置文件中存储的 `iteration` 变量的新值（即 ``#``）下（这是自动完成的）。

现在您可以运行 `create_training_dataset`，然后 `train_network`，依此类推。如果您的原始标签进行了任何调整，请从新的（通常是推荐的）权重开始，否则考虑使用已经训练好的网络权重（参见 Box 2）。

如果训练网络后对数据泛化良好，则继续分析新视频。否则，考虑标记更多数据。

### deeplabcut.refine_labels 的 API 文档
````{admonition} 点击按钮查看 API 文档
:class: dropdown
```{eval-rst}
.. include:: ./api/deeplabcut.refine_labels.rst
```
````

### deeplabcut.merge_datasets 的 API 文档
````{admonition} 点击按钮查看 API 文档
:class: dropdown
```{eval-rst}
.. include:: ./api/deeplabcut.merge_datasets.rst
```
````

### 用于演示 DeepLabCut 工作流程的 Jupyter 笔记本

我们还提供了两个 Jupyter 笔记本，一个用于使用预先标记的数据集，另一个用于使用用户自己的数据集。首先，我们准备了一个名为 [Demo\_yourowndata.ipynb](https://github.com/DeepLabCut/DeepLabCut/blob/main/examples/JUPYTER/Demo_yourowndata.ipynb) 的交互式 Jupyter 笔记本，它可以作为用户开发项目的模板。此外，我们提供了一个用于已启动项目的笔记本，其中包含标记数据。示例项目，命名为 [Reaching-Mackenzie-2018-08-30](https://github.com/DeepLabCut/DeepLabCut/tree/main/examples/Reaching-Mackenzie-2018-08-30)，包含一个具有默认参数的项目配置文件和 20 张图像，这些图像根据感兴趣区域进行了裁剪，作为示例数据集。这些图像是从一项研究小鼠熟练运动控制的视频中提取的。还提供了一些这些图像的示例标签。在此处查看更多详细信息 [here](https://github.com/DeepLabCut/DeepLabCut/tree/main/examples)。

## 3D 工具箱

有关使用 DeepLabCut 的 3D 工具箱的信息（从 2.0.7+ 开始），请参阅 [3D 概述](3D-overview)。

## 其他功能，有些尚待记录：

我们建议您 [查看这些额外的辅助函数](helper-functions)，它们可能会很有用（它们都是可选的）。