```rst
(multi-animal-userguide)=
# DeepLabCut 多动物项目用户指南

本文档旨在作为 maDLC（多动物 DeepLabCut）的用户指南，以支持 [Lauer 等人 2022 年](https://doi.org/10.1038/s41592-022-01443-0)发表的科学进展。

注意：当我们初次使用多动物模式时，我们强烈建议您使用 [项目管理器 GUI](project-manager-gui)。当您创建或加载一个多动物项目时，每个标签页都会针对多动物情况进行定制。只要您遵循 GUI 中的建议，您就可以顺利开始！

````{versionadded} 3.0.0
PyTorch 现在作为姿态估计模型的深度学习引擎可用，并带来了新的模型架构！有关从 TensorFlow 迁移到 PyTorch（如果您已经熟悉 DeepLabCut 和 TensorFlow 引擎）的更多信息，请查看 [PyTorch 用户指南](dlc3-user-guide)。如果您是 DeepLabCut 的新手，我们建议您使用 PyTorch 后端。
````

## 如何思考使用 maDLC：

您应该将 maDLC 视为由**四个**部分组成：
- (1) 策划标注数据，使您能够学习一个模型来跟踪感兴趣的目标/动物。
- (2) 创建高质量的姿态估计模型。
- (3) 在空间和时间上进行跟踪，即，将身体部位组装到检测到的对象/动物上，并在时间上进行链接。此步骤执行组装和跟踪（包括首先进行局部跟踪，然后通过全局推理进行轨迹片段的拼接）。
- (4) 任何您希望对输出数据进行的后处理，无论是在 DLC 内部还是外部。

因此，您应该始终首先标注、训练和评估姿态估计性能。如果且当该性能很高时，然后您才应该进入跟踪步骤（以及视频分析）。如下所示，这里有一个自然的断点。

<img src="https://images.squarespace-cdn.com/content/v1/57f6d51c9f74566f55ecf271/1596370260800-SP2GWKDPJCOIR7LJ31VM/ke17ZwdGBToddI8pDm48kB4fL2ovSQh5dRlH2jCMtpoUqsxRUqqbr1mOJYKfIPR7LoDQ9mXPOjoJoqy81S2I8N_N4V1vUb5AoIIIbLZhVYxCRW4BPu10St3TBAUQYVKcSV94BuD0XUinmig_1P1RJNYVU597j3jgswapL4c_w92BJE9r6UgUperYhWQ2ubQ_/workflow.png?format=2500w" width="550" title="maDLC" alt="maDLC" align="center" vspace = "50">

## 安装：

**快速入门：** 如果您在云端使用 DeepLabCut，或者无法使用 GUI 并且您应该使用以下方式安装：`pip install 'deeplabcut'`；如果您需要 GUI 支持，请使用：`pip install 'deeplabcut[gui]'`。请查看[安装页面](how-to-install)以获取更多信息，包括 GPU 支持。

如果您想使用最新的开发版本来编辑代码，请参阅[此处了解如何安装和测试它](https://deeplabcut.github.io/DeepLabCut/docs/recipes/installTips.html#how-to-use-the-latest-updates-directly-from-github)。

## 在终端或项目 GUI 中入门：

**GUI：** 只需启动您的 conda 环境，然后在终端中输入 `python -m deeplabcut`。
然后遵循各个标签页的指示！但是，阅读以下内容可能会有所帮助，以便您了解每个命令的作用。

**终端：** 要开始，🚨 (Windows) 导航到 Anaconda 提示符并右键单击“以管理员身份打开”，或 (Unix/MacOS) 仅在计算机上启动“终端”。我们假设您已经安装了 DeepLabCut（如果没有，请参阅[安装说明](how-to-install)）。接下来，启动您的 conda 环境（例如 `conda activate DEEPLABCUT`）。

```{Hint}
🚨 如果您使用 Windows，请始终以管理员权限打开终端！右键单击，然后选择“以管理员身份运行”。
```
请在此处阅读[更多信息](https://deeplabcut.github.io/DeepLabCut/docs/docker.html)，以及我们在 Nature Protocols 论文[此处](https://www.nature.com/articles/s41596-019-0176-0)提供的内容。另外，请参阅我们的[故障排除 Wiki](https://github.com/DeepLabCut/DeepLabCut/wiki/Troubleshooting-Tips)。

打开一个 ``ipython`` 会话并在终端中输入以下内容导入包：
```python
ipython
import deeplabcut
```

```{TIP}
对于每个函数，都有一个相关的帮助文档，可以通过在函数名后添加一个 **?** 来查看；例如 ``deeplabcut.create_new_project?``。要退出此帮助屏幕，请键入 ``:q``。
```

### (A) 创建一个新项目

```python
deeplabcut.create_new_project(
    "ProjectName",
    "YourName",
    ["/usr/FullPath/OfVideo1.avi", "/usr/FullPath/OfVideo2.avi", "/usr/FullPath/OfVideo1.avi"],
    copy_videos=True,
    multianimal=True,
)
```

提示：如果您想将项目文件夹放置在特定位置，请同时传递：``working_directory = "FullPathOftheworkingDirectory"``

- 注意，如果您是 Linux/macOS 用户，路径应如下所示：``["/home/username/yourFolder/video1.mp4"]``；如果您是 Windows 用户，则应如下所示：``[r"C:\username\yourFolder\video1.mp4"]``
- 注意，您也可以在上述行前加上 ``config_path=`` 来创建用于下一步骤的 config.yaml 路径，例如 ``config_path=deeplabcut.create_project(...)``)
    - 如果您没有设置，我们建议设置一个变量以便轻松使用！运行此步骤后，当您运行该行时，config_path 会为您打印出来，因此请设置一个变量以方便使用，例如：
```python
config_path = '/thefulloutputpath/config.yaml'
```
 - 请注意 Windows 与 Unix 的格式差异，见上文。

这组参数将在**工作目录**中创建一个名为 **项目名称+实验者姓名+项目创建日期** 的项目目录，并在 **videos** 目录中创建视频的符号链接。项目目录将包含子目录：**dlc-models**、**dlc-models-pytorch**、**labeled-data**、**training-datasets** 和 **videos**。在项目过程中生成的所有输出都将存储在这些子目录中的一个中，从而允许每个项目与其他项目分开管理。子目录的用途如下：

**dlc-models** 和 **dlc-models-pytorch** 具有相似的结构：第一个包含 TensorFlow 引擎的文件，而第二个包含 PyTorch 引擎的文件。在这些目录的顶层，有指向不同标签细化迭代的目录（见下文）：**iteration-0**、**iteration-1** 等。细化迭代目录存储着 shuffle 目录，每个 shuffle 目录存储与特定实验相关的模型数据：在特定的训练和测试集上训练和测试，以及特定的模型架构。每个 shuffle 目录包含子目录 *test* 和 *train*，其中每个子目录都保存着关于特征检测器参数的元信息配置文件。配置文件是 YAML 文件，这是一种常见的人类可读数据序列化语言。这些文件可以用标准文本编辑器打开和编辑。子目录 *train* 将存储模型训练期间的检查点（称为快照）。这些快照允许用户重新加载训练好的模型而无需重新训练，或者在训练中断时从特定的保存检查点继续训练。

**labeled-data:** 此目录将存储用于创建训练数据集的帧。来自不同视频的帧存储在单独的子目录中。每帧都有一个与对应视频中时间索引相关的文件名，这使用户能够将每帧追溯到其来源。

**training-datasets:** 此目录将包含用于训练网络的数据集以及有关如何创建训练数据集的元数据。

**videos:** 视频链接或视频的目录。当 **copy\_videos** 设置为 ``False`` 时，此目录包含视频的符号链接。如果设置为 ``True``，则视频将被复制到此目录。默认为 ``False``。此外，如果用户想在任何阶段向项目中添加新视频，可以使用 **add\_new\_videos** 函数。这将更新项目配置文件中视频的列表。注意：您既不需要为此文件夹使用视频，也不需要它来分析视频（它们可以位于任何位置）。

```python
deeplabcut.add_new_videos(
    "Full path of the project configuration file*",
    ["full path of video 4", "full path of video 5"],
    copy_videos=True/False,
)
```

\*请注意，*项目配置文件路径* 在本方案中将引用为 ``config_path``。

您也可以通过转换这些文件来使用单动物项目的标注数据。
有相关的文档：[将单动物标注数据转换为多动物数据](convert-maDLC)

![Box 1 - Multi Animal Project Configuration File Glossary](images/box1-multi.png)

### API 文档
````{admonition} 点击按钮查看 API 文档
:class: dropdown
```{eval-rst}
.. include:: ./api/deeplabcut.create_new_project.rst
```
````

### (B) 配置项目

接下来，打开在 **create\_new\_project** 期间创建的 **config.yaml** 文件。
您可以使用任何文本编辑器编辑此文件。熟悉一下参数的含义（Box 1）。您可以编辑各种参数，特别是您**必须添加 *个体* 和 *身体部位*（或兴趣点）的列表**。

<img src="https://images.squarespace-cdn.com/content/v1/57f6d51c9f74566f55ecf271/1588892210304-EW7WD46PYAU43WWZS4QZ/ke17ZwdGBToddI8pDm48kAXtGtTuS2U1SVcl-tYMBOAUqsxRUqqbr1mOJYKfIPR7LoDQ9mXPOjoJoqy81S2I8PaoYXhp6HxIwZIk7-Mi3Tsic-L2IOPH3Dwrhl-Ne3Z2YjE9w60pqfeJxDohDRZk1jXSVCSSfcEA7WmgMAGpjTehHAH51QaxKq4KdVMVBxpG/1nktc1kdgq2.jpg?format=1000w" width="175" title="colormaps" alt="DLC Utils" align="right" vspace = "50">

您还可以在此处设置用于所有下游步骤的*颜色映射*（也可随时编辑），例如标注 GUI、视频等。这里任何 [matplotlib 颜色映射](https://matplotlib.org/tutorials/colors/colormaps.html) 都可以使用！

在任何时候以编程方式编辑配置文件的一种简单方法是使用 **edit\_config** 函数，该函数接受配置文件的完整路径以及要覆盖的键值对字典。

```python
import deeplabcut

config_path = "/path/to/project-dlc-2025-01-01/config.yaml"
edits = {
    "colormap": "summer",
    "individuals": ["mickey", "minnie", "bianca"],
    "skeleton": [["snout", "tailbase"], ["snout", "rightear"]]
}
deeplabcut.auxiliaryfunctions.edit_config(config_path, edits)
```

请**不要**在身体部位、uniquebodyparts、个体等的名称中使用空格。

**注意：** 您需要编辑 config.yaml 文件以**修改以下项**，这些项指定动物 ID、身体部位和任何唯一的标签。请注意，我们还强烈建议您使用比您可能关心的**更多的身体部位**，即标注脊柱/尾巴沿线使用 8 个身体部位会比 4 个要好。这将有助于提高性能。

修改 `config.yaml` 至关重要：

```python
individuals:
- m1
- m2
- m3

uniquebodyparts:
- topleftcornerofBox
- toprightcornerofBox

multianimalbodyparts:
- snout
- leftear
- rightear
- tailbase

identity: True/False
```

**Individuals (个体):** 是标注数据集中“个体”的名称。这些可以是通用的（例如 mouse1、mouse2 等）。这些个体由 `multianimalbodyparts` 定义的相同身体部位组成。对于 GUI 中的标注和训练，重要的是每一帧中的所有个体都被标注。因此，请记住，您需要将个体数设置为标注数据集中出现的最大数量，即，如果（即使只有一帧）有 17 只动物，那么列表就应该是 `- indv1` 到 `- indv17`。请注意，一旦训练完成，如果您有更多或更少的动物的视频，那也没关系——在视频分析过程中可以有更多或更少的动物！

**Identity (身份):** 如果您能分辨动物，例如，一只可能有项圈，或者老鼠尾巴上有黑色标记，那么您应该一致地标注这些个体（例如，总是将带有黑色标记的老鼠标注为“indv1”等）。如果您有 4 只黑老鼠，并且您确实无法分辨它们，那么请将其保留为 `false`。

**Multianimalbodyparts (多动物身体部位):** 是每个个体所属的身体部位（在上面的列表中）。

**Uniquebodyparts (唯一身体部位):** 是您想要跟踪的点，但每个帧内只出现一次，即它们是“唯一的”。通常是像独特的物体、地标、工具等。它们也可以是动物，例如，当一只德国牧羊犬照看许多绵羊时，绵羊的身体部位将是 `multianimalbodyparts`，牧羊犬的部分将是 `uniquebodyparts`，而个体将是绵羊的列表（例如 Polly、Molly、Dolly，...）。

### (C) 选择要标注的帧

**关键：** 良好的训练数据集应包含捕获行为广度的足够数量的帧。理想情况下，这意味着要从不同的（行为）会话、不同的光照和不同的动物中选择帧，如果这些变化很大（以便训练一个不变的、鲁棒的特征检测器）。因此，为了创建可重复用于实验室的鲁棒网络，良好的训练数据集应反映行为在姿势、亮度条件、背景条件、动物身份等方面的多样性，这些将用于分析数据。对于简单的实验室行为，如老鼠伸手、开放性行为和果蝇行为，100-200 帧即可获得良好的效果 [Mathis 等人, 2018](https://www.nature.com/articles/s41593-018-0209-y)。然而，根据所需的精度、行为的性质、视频质量（例如运动模糊、不良照明）和环境，可能需要更多或更少的帧来创建良好的网络。最终，为了将分析扩展到可能包含意外条件的**大量视频集合**，可以在自适应的基础上细化数据集（参见下文的细化）。**对于 maDLC，请确保您已标注了动物紧密互动的帧！**

`extract_frames` 函数从项目配置文件中所有视频中提取帧，以创建训练数据集。从所有视频中提取的帧存储在项目“labeled-data”下以视频文件名命名的单独子目录中。此函数还有各种参数可能根据用户的需求有所帮助。

```python
deeplabcut.extract_frames(
    config_path,
    mode='automatic/manual',
    algo='uniform/kmeans',
    userfeedback=False,
    crop=True/False,
)
```

**关键点：** 建议保持帧尺寸较小，因为大帧会增加训练和推理时间，或者您可能没有足够大的 GPU 来处理。当运行 `extract_frames` 函数时，如果参数 `crop=True`，系统将询问您在 GUI 中绘制一个框（这会写入 config.yaml 文件）。

`userfeedback` 允许用户检查希望从中提取帧的视频。通过这种方式，如果您向 config.yaml 文件添加了更多视频，默认情况下它不会（再次）从每个视频中提取帧。如果您希望禁用此问题，请将 `userfeedback = True`。

提供的函数以随机和时间均匀分布的方式（uniform）选择视频中的帧，通过基于视觉外观的聚类（k-means），或者通过手动选择。对于姿势在整个视频中变化的行为，随机选择帧效果最好。然而，一些行为可能是稀疏的，例如在伸手够物时（reach），伸手和拉回的动作非常快，而在试验之间老鼠的移动并不多（因此，我们默认设置为 True，因为这对我们遇到的大多数用例都是最好的）。在这种情况下，允许基于 k-means 派生量化选择帧的函数会很有用。如果用户选择使用 k-means 作为聚类帧​​的方法，该函数将对视频进行下采样，并使用 k-means 对帧进行聚类，其中每帧都被视为一个向量。然后选择来自不同簇的帧。此过程确保了帧看起来不同。但是，对于大型和长时间的视频，由于计算复杂度，此代码运行缓慢。

**关键点：** 建议从包含有趣行为的视频时间段中提取帧，而不是提取整个视频中的帧。这可以通过在 config.yaml 文件中使用 start 和 stop 参数来实现。此外，用户可以使用 config.yaml 文件中的 numframes2extract 参数更改从每个视频中提取的帧数。

```{TIP}
对于 maDLC，**请确保您已标注了动物紧密互动的帧**！ 因此，手动选择一些帧是个好主意，如果互动在视频中不频繁的话。
```

然而，选择帧在很大程度上取决于所研究的数据和行为。因此，很难提供通用的代码来为每种行为和动物提取帧以创建良好的训练数据集。如果用户觉得缺少特定的帧，他们可以使用工具箱附带的交互式 GUI 使用感兴趣的手动选择的帧。可以通过使用以下命令启动它：

```python
deeplabcut.extract_frames(config_path, 'manual')
```

// FIXME(niels) - 添加一个 napari 帧提取器描述。用户可以使用 *Load Video* 按钮加载项目配置文件中的一个视频，使用滚动条浏览视频并 *Grab a Frame*。用户还可以查看提取的帧，例如在重新加载集合之前删除过于相似的帧（来自目录），然后手动标注它们。

````{admonition} 点击按钮查看 API 文档
:class: dropdown
```{eval-rst}
.. include:: ./api/deeplabcut.extract_frames.rst
```
````

### (D) 标注帧

```python
deeplabcut.label_frames(config_path)
```

工具箱提供了一个 **label\_frames** 函数，它可以帮助用户使用交互式图形用户界面 (GUI) 轻松标注所有提取的帧。用户应该已经在项目的配置文件中通过提供一个列表来命名要标注（兴趣点）的身体部位。以下命令调用 napari-deeplabcut 标注 GUI。

[🎥 演示](https://youtu.be/hsA9IB5r73E)

标注 GUI 中的热键（另请参阅 GUI 中的“帮助”）：

```
Ctrl + C: 从上一帧复制标签。
键盘箭头键：推进帧。
Delete 键：删除标签。
```

![hot keys](https://images.squarespace-cdn.com/content/v1/57f6d51c9f74566f55ecf271/192345a5-e411-4d56-b718-ef52f91e195e/Qwerty.png?format=2500w)

**关键点：** 建议**一致地标注相似的位置**（例如，在一个很大的手腕上，尝试标注相同的位置）。一般来说，**不应**由用户标注不可见或被遮挡的点，除非您想训练网络来“猜测”——这是可能的，但可能会影响准确性。如果您不想标注或看不到身体部位，只需不在帧上应用任何标签即可跳过它们。

可选：在向现有标注数据集添加更多标签的情况下，用户需要在 config.yaml 文件中将新标签追加到身体部位列表中。之后，用户可以调用 **label\_frames** 函数。将弹出一个框，询问用户是希望显示所有部分，还是只添加新标签。在所有图像都标注后保存标签将把新标签追加到现有的标注数据集中。

**maDeepLabCut 关键点：** 对于多动物标注，除非您能分辨动物，否则您无需担心每只动物的“ID”。例如：如果您有一只白鼠和一只黑鼠，请在所有帧中将白鼠标注为动物 1，将黑鼠标注为动物 2。如果有两只黑鼠，那么 ID 标签 1 或 2 可以在帧之间切换——您不需要尝试识别它们（但始终在一个帧内保持一致地标注）。如果您有 2 只黑鼠，但其中一只总是带有光纤（例如），那么**应该**将它们一致地标注为 animal1 和 animal\_fiber（例如）。多动物 DLC 的要点是训练能够首先将正确的身体部位分组到个体中，然后在给定视频中将这些点与特定个体关联起来，这进而利用时间信息将它们链接到视频帧中。

请注意，我们还强烈建议您使用比您可能拥有的更多的身体部位（见下面的示例）。

有关更多信息，请查看 [napari-deeplabcut 文档](napari-gui) 以获取有关标注工作流程的更多信息。

### (E) 检查标注的帧

检查标签是否已正确创建和存储对训练有益，因为标注是创建训练数据集最关键的部分之一。DeepLabCut 工具箱提供了一个 `check_labels` 函数来做到这一点。其用法如下：

```python
deeplabcut.check_labels(config_path, visualizeindividuals=True/False)
 ```

**maDeepLabCut：** 您可以检查并为每个个体或每个身体部位绘制颜色，只需设置标志 `visualizeindividuals=True/False`。请注意，您可以以两种状态运行此两次，以查看两个图像。

<p align="center">
<img src="https://images.squarespace-cdn.com/content/v1/57f6d51c9f74566f55ecf271/1586203062876-D9ZL5Q7NZ464FUQN95NA/ke17ZwdGBToddI8pDm48kKmw982fUOZVIQXHUCR1F55Zw-zPPgdn4jUwVcJE1ZvWQUxwkmyExglNqGp0IvTJZUJFbgE-7XRK3dMEBRBhUpx7krGdD6VO1HGZR3BdeCbrijc_yIxzfnirMo-szZRSL5-VIQGAVcQr6HuuQP1evvE/img1068_individuals.png?format=750w" width="50%">
</p>

对于 labeled-data 中每个视频目录，此函数将创建一个带有 **labeled** 后缀的子目录。这些目录包含用标注的身体部位绘制的帧。用户可以仔细检查身体部位是否已正确标注。如果它们不正确，用户可以重新加载帧（即 `deeplabcut.label_frames`），移动它们，然后再次单击保存。

````{admonition} 点击按钮查看 API 文档
:class: dropdown
```{eval-rst}
.. include:: ./api/deeplabcut.check_labels.rst
```
````

### (F) 创建训练数据集

此时，您需要选择您的神经网络类型。

对于 **PyTorch 引擎**，请参阅 [PyTorch 模型架构](dlc3-architectures) 查看选项。

对于 **TensorFlow 引擎**，请参阅 Lauer 等人 2021 年的选项。多动物模型将使用 `imgaug`、ADAM 优化、我们新的 DLCRNet 和批次训练。我们建议此时保留这些默认设置。在此步骤中，将下载 ImageNet 预训练网络的（例如 ResNet-50）权重。如果它们没有下载（您会在终端中看到下载过程），那么您可能没有权限执行此操作（这是我们看到某些 Windows 用户遇到的问题——请参阅**[
WIKI 故障排除以获取更多帮助！](
https://github.com/DeepLabCut/DeepLabCut/wiki/Troubleshooting-Tips)**）。

然后运行：

```python
deeplabcut.create_training_dataset(config_path)
```

- 函数中的参数集将对合并的标注数据集进行混洗，并将其拆分以创建训练集和测试集。**training-datasets** 目录下带有 ``iteration#`` 后缀的子目录存储数据集和元信息，其中 ``#`` 是项目配置文件中存储的 ``iteration`` 变量的值（此编号记录了数据集被细化的次数）。

- 可选：如果用户希望对 DeepLabCut 的性能进行基准测试，他们可以通过向 `num_shuffles` 指定一个整数值来创建多个训练数据集；有关更多详细信息，请参阅文档字符串。

- 每次创建训练数据集的迭代都会创建几个文件，供特征检测器使用，以及一个包含训练数据集元信息的 ``.pickle`` 文件。这也将在 **dlc-models-pytorch**（TensorFlow 引擎为 **dlc-models**）中创建两个子目录，称为 ``test`` 和 ``train``，它们各自包含一个名为 pose\_cfg.yaml 的配置文件。具体来说，用户可以在开始训练之前编辑 **train** 子目录中（TensorFlow 引擎为 **pose\_cfg.yaml**）的 **pytorch\_config.yaml** 文件。这些配置文件包含有关特征检测器参数的元信息。关键参数在 Box 2 中列出。

**数据增强 (DATA AUGMENTATION)：** 在此阶段，您还可以决定使用哪种类型的增强。调用 `create_training_dataset` 后，您可以编辑创建的 [**pytorch\_config.yaml**](dlc3-pytorch-config) 文件（或者 TensorFlow 引擎的 [**pose\_cfg.yaml**](
https://github.com/DeepLabCut/DeepLabCut/blob/master/deeplabcut/pose_cfg.yaml) 文件）。

- PyTorch 引擎：[Albumentations](https://albumentations.ai/docs/) 用于数据增强。有关图像增强选项的更多信息，请查看 [**pytorch\_config.yaml**](dlc3-pytorch-config)。
- TensorFlow 引擎：默认增强适用于大多数任务（如在 www.deeplabcut.org 上所示），但有许多选项，更多数据增强、中间监督等。仅 `imgaug` 增强可用于多动物项目。

[A Primer on Motion Capture with Deep Learning: Principles, Pitfalls, and Perspectives](
https://www.cell.com/neuron/pdf/S0896-6273(20)30717-0.pdf) 详细介绍了增强对于一个已办实例（见图 8）的优势。简而言之：使用 imgaug 并利用数据的对称性！

重要的是，如前通过 `deeplabcut.cropimagesandlabels` 在多动物项目中完成的图像裁剪现在是增强管道的一部分。换句话说，图像裁剪不再存储在 labeled-data/...\_cropped 文件夹中。裁剪大小仍默认为 (400, 400)；如果您的图像非常大（例如 2k、4k 像素），请考虑增加裁剪大小，但请注意，除非您有强大的 GPU（24 GB 内存或更多），否则可能会遇到内存错误。您可以减小批次大小，但这可能会影响性能。

此外，您可以指定一种裁剪采样策略：裁剪中心可以随机取自图像（`uniform`）或注定关键点（`keypoints`）；专注于具有高身体部位密度的场景区域（`density`）；最后，结合 `uniform` 和 `density` 形成一个 `hybrid` 平衡策略（这是默认策略）。请注意，这两个参数都可以在训练前在 **pose\_cfg.yaml** 配置文件中轻松编辑。提醒一下，将图像裁剪成更小的块是一种数据增强形式，它同时允许在无法容纳较大图像+较大批次大小的小 GPU 上使用批处理（这通常会提高性能并减少训练时间）。

**模型比较 (MODEL COMPARISON)：** 您还可以通过为不同网络创建相同的训练/测试分割来测试多个模型。您可以在项目管理器 GUI 中轻松完成此操作（通过选择“使用现有数据分割”选项），该选项还允许您比较 PyTorch 和 TensorFlow 模型。

````{versionadded} 3.0.0
您现在可以使用 `create_training_dataset_from_existing_split` 使用与现有 shuffle 相同的训练/测试分割来创建新的 shuffle。这允许您比较模型性能（在不同架构之间或使用不同训练超参数时），因为 shuffle 是在相同数据上训练和在相同测试数据上评估的！

示例用法——为 ResNet 50 姿态估计模型创建 3 个新的 shuffle（索引为 10、11 和 12），使用与 shuffle 0 相同的数据分割：

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

```python
deeplabcut.train_network(config_path, shuffle=1)
```

函数中的参数集启动网络训练，针对为一个特定 shuffle 创建的数据集。请注意，您可以在要训练的模型的 [**pytorch\_config.yaml**](dlc3-pytorch-config) 文件（或 TensorFlow 模型的 **pose\_cfg.yaml**）中更改训练参数（在开始训练之前）。

在用户指定的迭代期间，检查点存储在相应迭代和 shuffle 目录下的 *train* 子目录中。

````{admonition} 关于使用 PyTorch 引擎训练模型的技巧
:class: dropdown

可调用的示例参数：

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

DeepLabCut 3.0 中的 Pytorch 模型是为固定数量的 epochs 训练的，而不是像 TensorFlow 模型那样使用最大迭代次数。一个 epoch 是对训练数据集的单次遍历，这意味着您的模型正好看到了一次每张训练图像。因此，如果您有 64 张用于网络的训练图像，一个 epoch 是 64 次迭代，批次大小为 1（或者批次大小为 2 时为 32 次迭代，批次大小为 4 时为 16 次迭代，依此类推）。

默认情况下，预训练网络不在 DeepLabCut 工具箱中（因为它们可能超过 100MB），但在您训练之前会自动下载。

如果用户希望从特定检查点重新开始训练，可以在 *train* 子目录下的 [
**pytorch\_config.yaml**](
dlc3-pytorch-config) 文件中 ``resume_training_from`` 变量中指定检查点的完整路径（查看文档的“在特定检查点重新开始训练”部分）。

**关键点：** 建议**训练网络直到损失平稳**（根据数据集、模型架构和训练超参数，这发生在 100 到 250 个 epoch 训练之后）。

[**pytorch\_config.yaml**](dlc3-pytorch-config) 文件中的变量 ``display_iters`` 和 ``save_epochs`` 允许用户更改损失显示的频率和权重存储的频率。我们建议每 5 到 25 个 epoch 保存一次。
````

````{admonition} 关于使用 TensorFlow 引擎训练模型的技巧
:class: dropdown

可调用的示例参数：

```python
deeplabcut.train_network(
    config_path,
    shuffle=1,
    trainingsetindex=0,
    gputouse=None,
    max_snapshots_to_keep=5,
    autotune=False,
    displayiters=100,
    saveiters=15000,
    maxiters=30000,
    allow_growth=True,
)
```

默认情况下，预训练网络不在 DeepLabCut 工具箱中（因为它们大约 100MB/个），但在您训练之前会下载它们。但是，如果尚未从 TensorFlow 模型权重下载，它将被下载并存储在 *Pose\_Estimation\_Tensorflow* 的 *models* 子目录下的 *pre-trained* 子目录中。在用户指定的迭代期间，检查点存储在相应迭代目录下的 *train* 子目录中。

如果用户希望从特定检查点重新开始训练，他们可以在 *train* 子目录下的 **pose\_cfg.yaml** 文件中将检查点的完整路径指定给 ``init_weights`` 变量（参见 Box 2）。

**关键点：** 建议网络训练数千次迭代，直到损失平稳（通常是**500,000**次迭代，如果使用批次大小 1，以及使用默认批次大小 8 时为 **50-100K**）。

如果您使用的是 **maDeepLabCut**，推荐的训练迭代次数是 **20K-100K**（它会自动在 200K 停止！），因为我们使用 Adam 和批次大小 8；如果您因为内存原因必须减小批次大小，则需要增加迭代次数。

**pose\_cfg.yaml** 文件中的变量 ``display_iters`` 和 ``save_iters`` 允许用户更改损失显示的频率和权重存储的频率。

**maDeepLabCut 关键点：** 对于多动物项目，我们不仅使用不同和新的输出层，还使用新的数据增强、优化、学习率和批次训练默认值。因此，请使用较低的 ``save_iters`` 和 ``maxiters``。即，我们建议每 10K-15K 次迭代保存一次，并且只训练到 50K-100K 次迭代。我们建议密切关注损失，以避免过度拟合您的数据。好处是训练时间大大减少！！！
````

````{admonition} 点击按钮查看 train_network 的 API 文档
:class: dropdown
```{eval-rst}
.. include:: ./api/deeplabcut.train_network.rst
```
````

### (H) 评估训练好的网络

评估训练好的网络的性能非常重要。此性能通过计算两个指标来衡量：

- **平均均方根误差 (RMSE)**：介于手动标签和由您的训练好的 DeepLabCut 模型预测的标签之间的值。RMSE 与手动标签和 DeepLabCut 预测之间的平均欧几里得误差 (MAE) 成正比。MAE 会针对所有配对以及仅可能的配对（>p-cutoff）显示。这有助于排除（例如）被遮挡的身体部位。DeepLabCut 的一个优点是，由于其评分图谱的概率性输出，如果经过充分训练，它还可以可靠地报告给定帧中身体部位是否可见。（参见 Mathis 等人, 2018 年关于伸手动作中指尖和果蝇腿在 3D 行为中的讨论）。
- **平均精度 (mAP)** 和 **平均召回率 (mAR)**：用于由您的训练好的 DeepLabCut 模型预测的个体。该指标描述了模型的精度，基于关于正确检测个体所需的考虑定义。对于单动物模型来说，它不如 RMSE 在该情况下评估模型那样有用。

```{admonition} 关于 mAP 和 mAR 的更详细描述
:class: dropdown

对于多动物姿态估计，可以对每个图像做出多个预测。我们想了解所做预测中有多少是正确的比例。
然而，姿态估计的“正确预测”的概念并不简单：如果所有预测的关键点都在距离地面实况 5 像素内，则预测是正确的吗？距离地面实况 2 像素内？如果所有像素都与地面实况完美匹配，但错误的预测偏离 50 像素该怎么办？平均精度（
和平均召回率）通过设置不同的“正确性阈值”并平均结果来估计模型的精度/召回率。可以通过[物体关键点相似度](https://cocodataset.org/#keypoints-eval)来评估预测的“正确”程度，而不是使用交并比 (IoU)。

斯坦福 CS230 课程 [此处](https://cs230.stanford.edu/section/8/#object-detection-iou-ap-and-map) 是深入了解 mAP 的好资源。虽然它描述了用于对象检测的 mAP（预测*边界框*而不是关键点），但相同的指标可以计算姿态估计，其中预测与地面实况之间的相似性是通过[对象-关键点相似度](https://cocodataset.org/#keypoints-eval)而不是交并比 (IoU) 计算的。
```

通过在调用 `evaluate_network` 时设置 `plotting=True`，可以直观检查单个帧上的预测以评估模型的性能：

```python
deeplabcut.evaluate_network(config_path, Shuffles=[1], plotting=True)
```

🎥 [提供视频教程！](https://www.youtube.com/watch?v=bgfnz1wtlpo)

设置 ``plotting=True`` 会绘制所有测试和训练帧以及手动和预测的标签；默认情况下，这些将按身体部位类型着色。它们也可以通过传递 `plotting="individual"` 按个体着色。用户应该目视检查在‘evaluation-results’目录中创建的标注的测试（和训练）图像。理想情况下，DeepLabCut 会根据用户要求的精度对未见过的（测试图像）进行标注，并且平均训练和测试误差是可比较的（良好的泛化）。（数值上）构成可接受的 MAE 取决于许多因素（包括跟踪的身体部位的大小、标注的可变性等）。请注意，测试误差也可能大于训练误差，因为存在人为变异性（在标注中，参见 Mathis 等人，2018 年 Nature Neuroscience 第 2 期图 2）。

**可选参数：**

- `Shuffles: list, optional` - 指定训练数据集的 shuffle 索引的整数列表。默认为 [1]

- `plotting: bool | str, optional` - 在训练和测试图像上绘制预测。默认为 `False`；如果提供，则必须是 `True`、`False`、`"bodypart"` 或 `"individual"`。

- `show_errors: bool, optional` - 显示训练和测试误差。默认为 `True`

- `comparisonbodyparts: list of bodyparts, Default is all` - 将仅对这些身体部位计算平均误差（必须是身体部位的子集）。

- `gputouse: int, optional` - 表示您的 GPU 编号的自然数（参见 nvidia-smi 中的数字）。如果您没有 GPU，则设置为 None。参见：https://nvidia.custhelp.com/app/answers/detail/a_id/3751/~/useful-nvidia-smi-queries

- `pcutoff: float | list[float] | dict[str, float], optional`
（仅在使用 PyTorch 引擎时适用。对于 TensorFlow，请在 `config.yaml` 文件中设置 `pcutoff`。）
指定用于计算评估指标的截止值。
  - 如果为 `None`（默认值），则从项目配置中加载截止值。
  - 要将单个截止值应用于所有身体部位，请提供一个 `float`。
  - 要为每个身体部位指定不同的截止值，请提供以下之一：
    - 一个 `list[float]`：每个身体部位一个值，如果适用，为每个唯一身体部位提供一个额外的值。
    - 一个 `dict[str, float]`：键是身体部位名称，值是相应的截止值。
如果提供的字典中未包含某个身体部位，则该身体部位的默认 `pcutoff` 设为 `0.6`。

可以通过编辑 **config.yaml** 文件来自定义绘图（即可以修改颜色映射、刻度、标记大小（dotsize）和标签透明度（alpha-value））。默认情况下，每个身体部位都以不同的颜色绘制（由颜色映射控制），绘图标签指示其来源。请注意，默认情况下，人类标签绘制为加号（‘+’），DeepLabCut 的预测要么绘制为“.”（对于似然度 > `pcutoff` 的自信预测），要么绘制为 ‘x’（对于似然度 <= `pcutoff`）。

每个 shuffle 的评估结果都存储在项目目录中新创建的目录 ‘evaluation-results-pytorch’（对于 TensorFlow 模型为 ‘evaluation-results’）中的一个唯一子目录中。用户可以目视检查标注的和预测的身体部位之间的距离是否可接受。在针对同一训练数据集的不同 shuffle 进行基准测试的情况下，用户可以提供多个 shuffle 索引来评估相应的网络。如果泛化不足，用户可能希望：

• 检查标签是否已正确导入；即，未标注不可见点，并且兴趣点已准确标注

• 确保损失已经收敛

• 考虑标注更多图像并创建另一轮训练数据

````{admonition} 点击按钮查看 evaluate_network 的 API 文档
:class: dropdown
```{eval-rst}
.. include:: ./api/deeplabcut.evaluate_network.rst
```
````

**maDeepLabCut：（或在普通项目上！）**

在多动物项目中，模型评估至关重要，因为这是数据驱动地选择*最佳骨架*的步骤。跳过此步骤会导致视频分析默认使用冗余骨架，这不仅速度慢，而且不能保证最佳性能。

您还应该绘制评分图谱、locref 层和 PAFs 以评估性能：

```python
deeplabcut.extract_save_all_maps(config_path, shuffle=shuffle, Indices=[0, 5])
```

您可以删除 "Indices" 以对所有训练/测试图像运行此操作（这非常慢！）

### (I) 分析新视频

````{versionadded} 3.0.0
随着 DeepLabCut 3.0 中条件式自上而下模型的加入，现在可以在**视频分析期间**直接跟踪个体。如果您选择训练任何以 `ctd_` 开头的模型，您将能够使用 `ctd_tracking=True` 调用 `deeplabcut.analyze_videos`。要了解有关使用 CTD 进行跟踪的更多信息，请参阅 [
`COLAB_BUCTD_and_CTD_tracking`](
https://github.com/DeepLabCut/DeepLabCut/blob/main/examples/COLAB/COLAB_BUCTD_and_CTD_tracking.ipynb) COLAB 笔记本。
````

**-------------------- 决策点 -------------------**

**注意！**
**姿态估计和跟踪应被视为独立的步骤。** 如果此时没有良好的姿态估计评估指标，请停止，检查原始标签，添加更多数据等-->不要使用此模型继续前进。如果您认为自己有一个好的模型，请测试“原始”姿态估计性能在一个视频上以验证性能：

请运行：

```python
videos_to_analyze = ['/fullpath/project/videos/testVideo.mp4']
scorername = deeplabcut.analyze_videos(config_path, videos_to_analyze, videotype='.mp4')
deeplabcut.create_video_with_all_detections(config_path, videos_to_analyze, videotype='.mp4')
```

请注意，您**不会**得到通常会得到的 .h5/csv 文件（该文件在跟踪之后出现）。您将得到一个用于 `create_video_with_all_detections` 的 `pickle` 文件。

对于预测部位亲和场（part-affinity fields）的模型，另一个合理的检查是使用 `deeplabcut.utils.plot_edge_affinity_distributions` 检查边缘亲和力成本的分布。易于分离的分布表明模型已经学习了将关键点分组到不同个体中的强链接——这很可能是组件阶段所必需的特征（请注意，重叠的量也将取决于数据集中动物之间互动的多少）。所有 TensorFlow 多动物模型都使用部位亲和场，而仅由骨干名称（例如 `resnet_50`、`resnet_101`）组成的 PyTorch 模型也使用部位亲和场。如果您不确定您的 PyTorch 模型是否具有该字段，请检查 **pytorch\_config.yaml** 中是否有 `DLCRNetHead`。

如果您的输出视频干净且良好，以 `....full.mp4` 结尾（并且评估指标看起来不错，评分图谱看起来不错，绘制的评估图像，并且大多数边缘的亲和力分布相差很大），那么请继续！！！

如果情况不理想，我们建议提取和标注更多帧（甚至来自更多视频）。尽量标注动物的紧密互动以获得最佳性能。标记更多后，您可以创建新的训练集并进行训练。

您可以选择：
1. 从现有或新视频手动提取更多帧，并像最初构建训练数据集时一样进行标注；或者
2. 让 DeepLabCut 找到关键点检测不佳的帧，并自动为您提取它们。您需要做的就是运行：

```python
deeplabcut.find_outliers_in_raw_data(config_path, pickle_file, video_file)
```

其中 pickle\_file 是视频分析后得到的 `_full.pickle` 文件。标记的帧将被添加到相应 labeled-data 文件夹中图像的集合中供您标注。

### 动物组装和跨帧跟踪

姿态估计之后，现在执行组装和跟踪。

````{versionadded} v2.2.0
*新功能* v2.2 中有一个新颖的数据驱动方法来设置最佳骨架和组装指标，因此这不再需要用户输入。如果您确实想编辑指标，它们可以在 `inference_cfg.yaml` 文件中找到。
````

### 优化动物组装 + 视频分析：
请注意，**对于新视频，不需要将它们添加到 config.yaml 文件中**。您可以简单地在计算机的别处放置一个文件夹，然后传递视频文件夹（然后它将分析所有具有指定类型（即 ``videotype='.mp4'``）的视频），或者传递**文件夹**或您希望分析的精确视频的路径：

```python
deeplabcut.analyze_videos(config_path, ['/fullpath/project/videos/'], videotype='.mp4', auto_track=True)
```

### 如果 auto\_track = True：

```{versionadded} v2.2.0.3
向 `deeplabcut.analyze_videos` 添加了一个新参数 `auto_track=True`，将姿态估计、跟踪和拼接链接成一个函数调用，带有我们发现效果良好的默认设置。因此，您现在将获得标准 DLC 中可能习惯获得的 `.h5` 文件。如果 `auto_track=False`，则必须手动运行 `convert_detections2tracklets` 和
`stitch_tracklets`（见下文），从而可以对工作流程的最后一步进行更多控制（非常适合高级用户）。
```

### 如果 auto\_track = False：

您可以验证跟踪参数。特别是，您可以迭代更改参数，运行 `convert_detections2tracklets`，然后在 GUI 中加载它们（`refine_tracklets`），如果您想查看性能。如果您想编辑这些参数，您需要打开 `inference_cfg.yaml` 文件（或单击 GUI 中的按钮）。选项如下：

```python
# Tracking:
#p/m pixels in width and height for increasing bounding boxes.
boundingboxslack : 0
# Intersection over Union (IoU) threshold for linking two bounding boxes
iou_threshold: .2
# maximum duration of a lost tracklet before it's considered a "new animal" (in frames)
max_age: 100
# minimum number of consecutive frames before a detection is tracked
min_hits: 3
```

  - **关于监督身份跟踪的重要说明**

    如果网络已被训练为学习动物的身份（即您在训练前在 config.yaml 中设置了 `identity=True`），则此信息可在以下两方面得到利用：（i）动物组装，其中身体部位根据它们被预测属于的动物进行分组（在这种情况下不再考虑关键点对之间的亲和力）；以及（ii）动物跟踪，其中身份可以替代运动追踪器来形成轨迹片段。

要使用此 ID 信息，只需传递：
```python
deeplabcut.convert_detections2tracklets(..., identity_only=True)
```

- **注意：** 如果只组装和跟踪一个个体，则跳过组装和跟踪，并将检测视为单动物项目中的情况；即，保留并累积最高置信度的关键点以形成一个长轨迹片段。用户无需采取任何操作，这是自动完成的。


可以通过 `deeplabcut.utils.make_labeled_video.create_video_from_pickled_tracks` 评估动物组装和跟踪质量。此函数在移动到细化轨迹片段之前，提供了额外的诊断工具。


如果动物组装看起来不理想，与上述异常值搜索不同的一种替代方法是将 `_assemblies.pickle` 传递给 `find_outliers_in_raw_data`，以替换 `_full.pickle`。这将使异常值搜索集中在不寻常的组装上（即，以奇怪的方式重建的动物骨架）。这在拥挤的场景或动物紧密互动的帧中可能更敏感。请注意，到那时，最好还是继续完成剩余的步骤，并从最终的 h5 文件中提取异常值，就像单动物项目中习惯的那样。


**接下来，使用以下方法将轨迹片段拼接到完整的轨道中：

```python
deeplabcut.stitch_tracklets(
    config_path,
    ['videofile_path'],
    videotype='mp4',
    shuffle=1,
    trainingsetindex=0,
)
```

请注意，函数的基线签名与 `analyze_videos` 和 `convert_detections2tracklets` 相同。如果需要重建的轨道数量与 config.yaml 中最初定义的个体数量不同，则可以直接指定 `n_tracks`（即视频中的动物数量），如下所示：

```python
deeplabcut.stitch_tracklets(..., n_tracks=n)
```

在这种情况下，文件列将默认为虚拟动物名称（ind1、ind2，...，最多到 indn）。

### API 文档

````{admonition} 点击按钮查看 analyze_videos 的 API 文档
:class: dropdown
```{eval-rst}
.. include:: ./api/deeplabcut.analyze_videos.rst
```
````

````{admonition} 点击按钮查看 convert_detections2tracklets 的 API 文档
:class: dropdown
```{eval-rst}
.. include:: ./api/deeplabcut.convert_detections2tracklets.rst
```
````

````{admonition} 点击按钮查看 stitch_tracklets 的 API 文档
:class: dropdown
```{eval-rst}
.. include:: ./api/deeplabcut.stitch_tracklets.rst
```
````

### 使用无监督身份跟踪：

在 Lauer 等人 2022 年的论文中，我们引入了一种新的方法来进行动物的无监督重新识别（reID）。在这里，您可以使用轨迹片段来学习动物的身份，以提高您的跟踪性能。要使用代码：

```python
deeplabcut.transformer_reID(config, videos_to_analyze, n_tracks=None, videotype="mp4")
```

请注意，您应该传递您期望在视频中看到的动物数量 `n_tracks`。

### 细化轨迹片段：

您还可以选择**细化轨迹片段**。您可以修复“主要”ID 交换（即当动物交叉时）以及微调个体身上的关键点。您将加载上面创建的 `...trackertype.pickle` 或 `.h5` 文件，然后您可以启动一个 GUI 来交互式地细化数据。这也有几个选项，所以请查看文档字符串。保存细化后的轨道后，您将获得一个 `.h5` 文件（类似于标准 DLC 中可能习惯使用的文件）。您还可以加载 (1) 以消除小的抖动，和 (2) 再次加载此 `.h5` 以进行进一步细化（如果发现其他问题），等等！

```python
deeplabcut.refine_tracklets(config_path, pickle_or_h5_file, videofile_path, max_gap=0, min_swap_len=2, min_tracklet_len=2, trail_len=50)
```

如果您使用 GUI（或其他方式），请考虑以下设置：

<img src="https://images.squarespace-cdn.com/content/v1/57f6d51c9f74566f55ecf271/1619628014395-BQ09VLLTKCLQQGRB5T9A/ke17ZwdGBToddI8pDm48kLMj_XrWI9gi4tVeBdgcB8p7gQa3H78H3Y0txjaiv_0fDoOvxcdMmMKkDsyUqMSsMWxHk725yiiHCCLfrh8O1z4YTzHvnKhyp6Da-NYroOW3ZGjoBKy3azqku80C789l0lt53wR20brczws2A6XSGt3kSTbW7uM0ncVKHWPvgHR4kN5Ka1TcK96ljy4ji9jPkQ/TrackletGUI.png?format=1000w" width="950" title="maDLCtrack" alt="maDLC" align="center" vspace = "50">

\*注意，设置 `max_gap=0` 可用于填充视频中的所有帧；否则，1-n 是您想要填充的帧数，即您可能想填充 5 帧的短间隙，但 15 帧表示其他问题，依此类推。您可以通过编辑该值并在 GUI 中重新启动弹出窗口来非常轻松地在 GUI 中测试此项。

如果您填补了间隙，它们将被关联到一个极低的概率，0.01，因此您会知道这不是网络的最佳估计，这是人工覆盖！因此，如果您创建视频，则需要将 pcutoff 设置为 0 才能“看到”填补的片段。

[在此处阅读更多信息！](functionDetails.md#madeeplabcut-critical-point---assemble--refine-tracklets)

简短演示：
 <p align="center">
<img src="https://images.squarespace-cdn.com/content/v1/57f6d51c9f74566f55ecf271/1588690928000-90ZMRIM8SN6QE20ZOMNX/ke17ZwdGBToddI8pDm48kJ1oJoOIxBAgRD2ClXVCmKFZw-zPPgdn4jUwVcJE1ZvWQUxwkmyExglNqGp0IvTJZUJFbgE-7XRK3dMEBRBhUpxBw7VlGKDQO2xTcc51Yv6DahHgScLwHgvMZoEtbzk_9vMJY_JknNFgVzVQ2g0FD_s/refineDEMO.gif?format=750w" width="70%">
</p>

### (J) 过滤姿态数据

首先，这里有一些关于扩大视频分析规模的技巧，包括对许多文件夹进行循环以进行批量处理：https://github.com/DeepLabCut/DeepLabCut/wiki/Batch-Processing-your-Analysis

您也可以过滤预测的身体部位：
```python
deeplabcut.filterpredictions(config_path,['/fullpath/project/videos/reachingvideo1.avi'])
```
注意，这会创建一个以 filtered.h5 结尾的文件，可用于进一步分析。此过滤步骤有许多参数，因此请参阅键入时的完整文档字符串：``deeplabcut.filterpredictions?``

````{admonition} 点击按钮查看 API 文档
:class: dropdown
```{eval-rst}
.. include:: ./api/deeplabcut.filterpredictions.rst
```
````

### (K) 绘制轨迹，(L) 创建标注视频

- **注意 :bulb::mega::** 在创建视频之前，您应该设置用于绘图的阈值。这在 `config.yaml` 文件中设置为 `pcutoff`——如果您有一个训练良好的网络，这个值应该很高，例如设置为 `0.8` 或更高！如果您**已填充间隙**，则需要将其设置为 `0` 才能“看到”填充的部分。


- 您还可以通过查看 `plot_trajectories` 期间创建的似然度图来确定一个好的 `pcutoff` 值：

绘制输出：
```python
  deeplabcut.plot_trajectories(config_path,['/fullpath/project/videos/reachingvideo1.avi'],filtered = True)
```

创建视频：
```python
  deeplabcut.create_labeled_video(config_path, [videos], videotype='avi', shuffle=1, trainingsetindex=0, filtered=False, fastmode=True, save_frames=False, keypoints_only=False, Frames2plot=None, displayedbodyparts='all', displayedindividuals='all', codec='mp4v', outputframerate=None, destfolder=None, draw_skeleton=False, trailpoints=0, displaycropped=False, color_by='bodypart', track_method='')
```
- **注意 :bulb::mega::** 您在视频绘图方面有很多选项（质量、显示类型等）。我们建议查看文档字符串！

（更多详情 [此处](functionDetails.md#i-video-analysis-and-plotting-results)）

````{admonition} 点击按钮查看 plot_trajectories 的 API 文档
:class: dropdown
```{eval-rst}
.. include:: ./api/deeplabcut.plot_trajectories.rst
```
````

````{admonition} 点击按钮查看 create_labeled_video 的 API 文档
:class: dropdown
```{eval-rst}
.. include:: ./api/deeplabcut.create_labeled_video.rst
```
````

### 帮助：

在 ipython/Jupyter Notebook 中：

```
deeplabcut.nameofthefunction?
```

在 python 或 pythonw 中：

```
help(deeplabcut.nameofthefunction)
```

## “日常”使用提示：

<p align="center">
<img src= https://static1.squarespace.com/static/57f6d51c9f74566f55ecf271/t/5ccc5abe0d9297405a428522/1556896461304/howtouseDLC-01.png?format=1000w width="80%">
 </p>

您始终可以退出 conda 环境，只需通过以下方式轻松返回项目：

Linux/MacOS 格式示例：
```
source activate yourdeeplabcutEnvName
ipython or pythonw
import deeplabcut
config_path ='/home/yourprojectfolder/config.yaml'
```
Windows 格式示例：
```
activate yourdeeplabcutEnvName
ipython
import deeplabcut
config_path = r'C:\home\yourprojectfolder\config.yaml'
```

现在，您可以运行本文档中描述的任何函数。

# 获取 maDLC 的帮助：

- 如果您有关于如何使用代码的详细问题，或者遇到了不是“错误”但需要代码帮助的问题，请在 [![Image.sc forum](https://img.shields.io/badge/dynamic/json.svg?label=forum&amp;url=https%3A%2F%2Fforum.image.sc%2Ftags%2Fdeeplabcut.json&amp;query=%24.topic_list.tags.0.topic_count&amp;colorB=brightgreen&amp;&amp;suffix=%20topics&amp;logo=data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAA4AAAAOCAYAAAAfSC3RAAABPklEQVR42m3SyyqFURTA8Y2BER0TDyExZ+aSPIKUlPIITFzKeQWXwhBlQrmFgUzMMFLKZeguBu5y+//17dP3nc5vuPdee6299gohUYYaDGOyyACq4JmQVoFujOMR77hNfOAGM+hBOQqB9TjHD36xhAa04RCuuXeKOvwHVWIKL9jCK2bRiV284QgL8MwEjAneeo9VNOEaBhzALGtoRy02cIcWhE34jj5YxgW+E5Z4iTPkMYpPLCNY3hdOYEfNbKYdmNngZ1jyEzw7h7AIb3fRTQ95OAZ6yQpGYHMMtOTgouktYwxuXsHgWLLl+4x++Kx1FJrjLTagA77bTPvYgw1rRqY56e+w7GNYsqX6JfPwi7aR+Y5SA+BXtKIRfkfJAYgj14tpOF6+I46c4/cAM3UhM3JxyKsxiOIhH0IO6SH/A1Kb1WBeUjbkAAAAAElFTkSuQmCC)](https://forum.image.sc/tags/deeplabcut) 上发帖

- 如果您有一个简短的问题，适合“聊天”格式：
[![Gitter](https://badges.gitter.im/DeepLabCut/community.svg)](https://gitter.im/DeepLabCut/community?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge)

- 如果您想分享一些结果，或看到别人的：
[![Twitter Follow](https://img.shields.io/twitter/follow/DeepLabCut.svg?label=DeepLabCut&style=social)](https://twitter.com/DeepLabCut)

- 如果你有代码错误报告，请创建一个 issue 并展示最小代码以重现错误：https://github.com/DeepLabCut/DeepLabCut/issues

- 如果您正在寻找资源来增加您对软件的理解和一般指南，我们有一个开源的免费课程：http://DLCcourse.deeplabcut.org。

**请注意：** 我们不能提供实验设计和数据分析的帮助或支持。这方面的请求太多，无法在我们的收件箱中持续提供。我们很高兴在论坛上以社区化的、可扩展的方式回答此类问题。我们希望并相信我们已经提供了足够的工具和资源来入门并加速您的研究项目，这一点得到了使用 DLC 的 >700 次引用、其他人的 2 项临床试验以及无数次应用的支持。因此，我们相信此代码有效、易于访问，并且只需有限的编程知识即可使用。请阅读我们的 [使命与价值观声明](mission-and-values) 以了解我们希望为您提供什么的更多信息。
```