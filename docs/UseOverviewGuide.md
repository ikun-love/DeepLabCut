(overview)=
# 🥳 DeepLabCut 入门指南：我们的关键建议

下面我们将首先概述开始使用 DeepLabCut 所需的条件，介绍使用 DeepLabCut 的不同方式，然后是完整的工作流程。请注意，我们强烈建议您也阅读并遵循我们的 [Nature Protocols 论文](https://www.nature.com/articles/s41596-019-0176-0)，该论文对于标准的 DeepLabCut 仍然完全适用。

```{Hint}
💡📚 如果您是 Python 和 DeepLabCut 的新手，当您准备好开始使用 DeepLabCut 应用程序时，可以考虑查看我们的 [初学者指南](https://deeplabcut.github.io/DeepLabCut/docs/beginner-guides/beginners-guide.html)！
```

## [如何安装 DeepLabCut](how-to-install)

本页不详细介绍安装过程，如果您正在寻找这方面的信息，请点击上面的链接。有关开始使用 DeepLabCut 的更多详细信息，请参阅下文！

## 我们支持的功能：

我们主要是一个支持基于深度学习的姿态估计的软件包。我们有许多模型和选项，但不要感到不知所措——开发团队已经尽力提供“尽可能最好的默认设置”！

- 确定您的需求：主要有两种**模式，标准 DeepLabCut 或多动物 DeepLabCut**。我们强烈建议您仔细考虑哪一种最适合您的需求。例如，一只白鼠 + 一只黑鼠应使用标准模式，而两只黑鼠则应使用多动物模式。**[关于在不同场景中如何使用 DLC（单动物 vs. 多动物）的重要信息](important-info-regd-usage)** 然后选择一个用户指南：

  - (1) [如何使用标准 DeepLabCut](single-animal-userguide)
  - (2) [如何使用多动物 DeepLabCut](multi-animal-userguide)

- 值得注意的是，从 DLC3+ 开始，单动物和多动物的代码库更加集成，我们支持 **自上而下 (top-down)**、**自下而上 (bottom-up)**，以及一种被称为 **BUCTD**（自下而上条件化自上而下）的、目前最先进的“混合”方法模型。
  - 如果这些术语对您来说是新的，请查看我们的 [深度学习运动捕捉入门指南](https://www.sciencedirect.com/science/article/pii/S0896627320307170)！简而言之，这两种方法都适用于单只或多只动物，并且每种方法在您的数据上表现可能更好或更差。

<p align="center">
<img src= https://ars.els-cdn.com/content/image/1-s2.0-S0896627320307170-gr5_lrg.jpg?format=1000w width="50%">
 </p>

  - 以下是关于 BUCTD 的更多信息：
<p align="center">
<img src= https://github.com/amathislab/BUCTD/raw/main/media/BUCTD_fig1.png?format=1000w width="50%">
 </p>
 
 **额外的学习资源：**

 - [教程 (TUTORIALS)](https://www.youtube.com/channel/UC2HEbWpC_1v6i9RnDMy-dfA?view_as=subscriber)：演示代码库各种使用方面的视频教程。
 - [操作指南 (HOW-TO-GUIDES)](overview)：关于如何在您自己的数据集上使用 DeepLabCut 的分步用户指南（见下文）
 - [解释 (EXPLANATIONS)](https://github.com/DeepLabCut/DeepLabCut-Workshop-Materials)：关于理解 DeepLabCut 如何工作的资源。
 - [参考文献 (REFERENCES)](https://github.com/DeepLabCut/DeepLabCut#references)：阅读 DeepLabCut 背后的科学原理。
 - [GUI 初学者指南](https://deeplabcut.github.io/DeepLabCut/docs/beginners-guide.html)

开始学习：[一个关于如何浏览文档的视频教程！](https://www.youtube.com/watch?v=A9qZidI7tL8)


### 您开始需要准备什么：

 - **一组视频，涵盖您想要跟踪的行为类型。** 包含不同背景、不同个体和不同姿势的 10 个视频，远比 1 或 2 个视频（来自 1 或 2 个不同个体的视频）要好（即，来自 10 个视频的各 10-20 帧，**远比**来自 2 个视频的 50-100 帧要好）。

 - **最少，一台带 CPU 的计算机。** 如果您想在自己的计算机上为大量实验使用 DeepLabCut，那么您应该配备一块 NVIDIA GPU。技术规格请参阅 [此处](https://github.com/DeepLabCut/DeepLabCut/wiki/FAQ)。您也可以使用云端计算资源，包括 COLAB（[查看方法](https://github.com/DeepLabCut/DeepLabCut/blob/master/examples/README.md)）。


### 您开始时**不需要**什么：

 - 不需要特定的相机/视频；彩色、单色等都可以。如果您能看到想要测量的东西，那么它就能对您起作用（假设有足够多的标记数据）。

 - 不需要特定的计算机（但请参阅上面的建议），我们的软件支持 Linux、Windows 和 MacOS。


### 概述：
**DeepLabCut** 是一种用于对执行各种任务的动物进行无标记姿态估计的软件包。该软件可以为各种任务管理多个项目。每个项目由项目名称（例如 TheBehavior）、实验者名称（例如 YourName）以及创建日期来标识。该项目文件夹包含一个 ``config.yaml``（文本文件），文件中包含了各种（项目）参数以及指向项目数据的链接。


<p align="center">
<img src=   https://images.squarespace-cdn.com/content/v1/57f6d51c9f74566f55ecf271/1572293604382-W6BWA63LZ9J8R7N0QEA5/ke17ZwdGBToddI8pDm48kIw6YkRUEyoge4858uAJfaMUqsxRUqqbr1mOJYKfIPR7LoDQ9mXPOjoJoqy81S2I8N_N4V1vUb5AoIIIbLZhVYwL8IeDg6_3B-BRuF4nNrNcQkVuAT7tdErd0wQFEGFSnH9wUPiI8bGoX-EQadkbLIJwhzjIpw393-uEwSKO7VZIL9gN_Sb5I_dLwvWryjeCJg/dlc_overview-01.png?format=1000w width="80%">
 </p>

 <p align="center">
 <img src="https://images.squarespace-cdn.com/content/v1/57f6d51c9f74566f55ecf271/1560124235138-A9VEZB45SQPD5Z0BDEXA/ke17ZwdGBToddI8pDm48kKsvCFNoOAts8bgs5LXY20UUqsxRUqqbr1mOJYKfIPR7LoDQ9mXPOjoJoqy81S2I8N_N4V1vUb5AoIIIbLZhVYxCRW4BPu10St3TBAUQYVKcZaDohTswVrVk6oKw3G03bTl18OXeDyNJsBjNlGiyPYGo9Ewyd5AI5wx6CleNeBtf/dlc_steps.jpg?format=1000w" width="80%">
</p>

### 工作流程概述：
此页面列出了 DeepLabCut 的基本功能以及演示（Demos）。每个功能都有许多可选参数。有关详细的功能文档，请参阅主要用户指南或 API 文档。如需其他帮助，您可以使用 [help](UseOverviewGuide.md#help) 函数来更好地理解每个功能的作用。

<p align="center">
  <img src="https://static1.squarespace.com/static/57f6d51c9f74566f55ecf271/t/5cca272524a69435c3251c40/1556752170424/flowfig.jpg?format=1000w" width=95%>
  <br>
  <em>
   <a href="https://static1.squarespace.com/static/57f6d51c9f74566f55ecf271/t/5cca272524a69435c3251c40/1556752170424/flowfig.jpg?format=1000w">全屏查看</a>
  </em>
</p>

您的计算机上可以拥有任意数量的项目。您可以将 DeepLabCut 安装在 [一个环境中](../conda-environments/README.md)，并随时退出和返回该环境来运行代码。您只需要指向正确的 ``config.yaml`` 文件，就可以 [跳回来继续操作](/docs/UseOverviewGuide.md#tips-for-daily-use)！下面的文档将引导您完成各个步骤。

<p align="center">
<img src=  https://images.squarespace-cdn.com/content/v1/57f6d51c9f74566f55ecf271/1559758477126-B9PU1EFA7L7L1I24Z2EH/ke17ZwdGBToddI8pDm48kH6mtUjqMdETiS6k4kEkCoR7gQa3H78H3Y0txjaiv_0fDoOvxcdMmMKkDsyUqMSsMWxHk725yiiHCCLfrh8O1z5QPOohDIaIeljMHgDF5CVlOqpeNLcJ80NK65_fV7S1UQf4d-kVja3vCG3Q_2S8RPAcZTZ9JxgjXkf3-Un9aT84H3bqxw7fF48mhrq5Ulr0Hg/howtouseDLC2d_3d-01.png?format=500w width="60%">
 </p>


(important-info-regd-usage)=

# 关于使用 DeepLabCut 的具体建议：

## 使用 DeepLabCut 的重要信息：

我们建议您首先使用 **DeepLabCut 进行单动物场景**来理解工作流程——即使只是使用我们的演示数据也可以。多动物跟踪更为复杂——即用户需要做出一些额外的决策。然后，当您准备好后，可以深入研究多动物场景...

### 关于开始使用 maDeepLabCut 的附加信息：

我们强烈建议您首先在项目管理器 GUI（[选项 3](docs/functionDetails.md#deeplabcut-project-manager-gui)）中使用它。这将允许您在引导过程中熟悉额外的步骤。然后，您总可以在您最喜欢的 IDE、Notebooks 等环境中使用所有功能。

### *您属于哪种场景？*

- **我有单动物视频：**
   - 快速入门：当您调用 `create_new_project` 时（并将 `multianimal=` 标志保留为默认的 False）。这是大多数用户的典型工作路径。

- **我有单动物视频，但想使用为多动物项目引入的更新后的网络功能：**
   - 快速入门：当您调用 `create_new_project` 时，只需设置标志 `multianimal=True`。这使得您即使只有一个动物也能使用 maDLC 功能。请注意，这在单动物项目中很少需要，也不是推荐的路径。您可能想要使用此功能的一些场景提示：这对于例如手或老鼠来说是很好的，如果您认为训练期间的“骨架”会提高性能。但**不要**对那些可以被识别为独立物体的特征使用此功能。也就是说，不要将胡须 1、胡须 2、胡须 3 标记为 3 个个体。每根胡须总是有特定的空间位置的，将它们标记为个体会导致性能**比**单动物模式差。

[有视频教程！](https://youtu.be/JDsa8R5J0nQ)

- **我的视频中有多个*外观相同的动物*：**
   - 快速入门：当您调用 `create_new_project` 时，设置标志 `multianimal=True`。如果您无法区分它们，可以在每一帧中将“个体”ID 分配给任何动物。请查看此 [带 2.2 版本的标记视频演示](https://www.youtube.com/watch?v=_qbEqNKApsI)

[有视频教程！](https://www.youtube.com/watch?v=Kp-stcTm77g)

- **我有多个动物，*但我能区分它们*，并想使用 DLC2.2：**
   - 快速入门：当您调用 `create_new_project` 时，设置标志 `multianimal=True`。并且务必将“个体”ID 名称标记为相同；例如，如果您有 mouse1 和 mouse2，但 mouse2 总是带有微型显微镜，那么在每一帧中都要一致地标记 mouse2。请查看此 [带 2.2 版本的标记视频演示](https://www.youtube.com/watch?v=_qbEqNKApsI)。然后，您**必须**在 config.yaml 文件中加入以下内容：`identity: true`

[有视频教程！](https://www.youtube.com/watch?v=Kp-stcTm77g) - 另外，如果您能区分它们，请一致地标记动物！

- **我有一个 2.2 之前的单动物项目，但想升级到 2.2：**

请阅读 [此转换为 maDLC 指南](convert-maDLC)

# 使用 DeepLabCut 的选项：

太好了——现在您了解了总体工作流程，让我们开始吧！这里有几个选项供您选择。

[**选项 1**](using-demo-notebooks)：演示 (DEMOs)：快速通过我们的数据了解 DLC。

[**选项 2**](using-project-manager-gui)：独立 GUI：是初学者希望使用 DeepLabCut 处理自己数据的理想选择。

[**选项 3**](using-the-terminal)：在终端中使用：最适合高级用户，因为通过终端界面，您可以获得最大的灵活性和选项。

(using-demo-notebooks)=
## 选项 1：演示 Notebooks：
[有视频教程！](https://www.youtube.com/watch?v=DRT-Cq2vdWs)

我们提供了 Jupyter 和 COLAB Notebooks，可用于处理预先标记的数据集以及用户自己的数据集。所有演示请参见 [此处](../examples/README.md)！请注意，由于 MacOS 需要编译的 Python 框架，GUI 很难在 Jupyter 中支持。虽然可以通过一些调整启动它们，但我们建议您使用项目管理器 GUI 或终端，因此请遵循以下说明。

(using-project-manager-gui)=
## 选项 2：使用项目管理器 GUI：
[视频教程！](https://www.youtube.com/watch?v=KcXogR-p5Ak)

[视频教程#2！](https://youtu.be/Kp-stcTm77g)

在终端中输入 ``ipython`` 或 ``python`` 来启动 Python（注意：对于 Mac 用户，使用 pythonw 已于 2022 年弃用）。如果您在云端使用 DeepLabCut，则无法使用 GUI。如果您使用 Windows，请始终以管理员身份打开终端。请在我们的 Nature Protocols 论文 [此处](https://www.nature.com/articles/s41596-019-0176-0) 阅读更多信息。另请参阅我们的 [故障排除 Wiki](https://github.com/DeepLabCut/DeepLabCut/wiki/Troubleshooting-Tips)。

只需打开终端并输入：
```python
python -m deeplabcut
```
就是这样！请遵循 GUI 获取详细信息

(using-the-terminal)=
## 选项 3：使用程序终端，启动 iPython*：

[有视频教程！](https://www.youtube.com/watch?v=7xwOhUcIGio)

请决定您想使用 DeepLabCut 的哪种模式，并遵循以下其中一项：

- (1) [如何使用标准 DeepLabCut](single-animal-userguide)
- (2) [如何使用多动物 DeepLabCut](multi-animal-userguide)