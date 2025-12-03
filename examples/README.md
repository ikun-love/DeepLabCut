# 演示 Jupyter 和 Colaboratory Notebook

<img src="https://images.squarespace-cdn.com/content/v1/57f6d51c9f74566f55ecf271/1572293604382-W6BWA63LZ9J8R7N0QEA5/ke17ZwdGBToddI8pDm48kIw6YkRUEyoge4858uAJfaMUqsxRUqqbr1mOJYKfIPR7LoDQ9mXPOjoJoqy81S2I8N_N4V1vUb5AoIIIbLZhVYwL8IeDg6_3B-BRuF4nNrNcQkVuAT7tdErd0wQFEGFSnH9wUPiI8bGoX-EQadkbLIJwhzjIpw393-uEwSKO7VZIL9gN_Sb5I_dLwvWryjeCJg/dlc_overview-01.png?format=1000w" width="550" title="DLC" alt="DLC" align="right" vspace = "70">

我们提供了一个项目管理器 GUI (图形用户界面)，它将引导您完成 DeepLabCut 工具箱的主要步骤和选项。然而，通过在交互式环境（如 Jupyter*）中运行代码，可以访问更多选项和功能。此外，如果您没有 GPU，可以在任何计算机上创建您的项目，然后将项目迁移到云端以使用 GPU。为此，我们为您提供了 Google Colaboratory Notebook（请参阅[下方的 Google Colaboratory 演示](/examples#demo-deeplabcut-training-and-analysis-on-google-colaboratory-with-googles-gpus)）。

## 演示 1：在我们的开放区域数据 (open-field data) 上运行 DeepLabCut
 - 这将让您熟悉 DeepLabCut 的工作流程。请遵循 Notebook 中的说明！

请注意，包含已标记数据的 Notebook：[抓取数据 (reaching data)](JUPYTER/Demo_labeledexample_MouseReaching.ipynb)，或 [开放区域数据 (open-field data)](JUPYTER/Demo_labeledexample_Openfield.ipynb) 可以在 CPU、GPU 等上运行。其中使用开放区域数据（open-field data）的 Notebook，即使仅在 GPU 上训练半小时也能获得良好/尚可的结果！（请注意，**这并非** Mathis 等人 2018 年论文中使用的完整数据集）

## 演示 2：在[您自己的数据](JUPYTER/Demo_yourowndata.ipynb)上设置 DeepLabCut
- 掌握了这些演示之后，此 Notebook 将引导您完成构建自己的分析流程 (pipeline) 的方法：
  - 创建一个新项目
  - 标记新数据
  - 然后，您可以选择使用 CPU 或 GPU（Notebook 将在此处指导您），来训练、分析并对您的数据执行一些基本分析。

对于基于 GPU 的训练和分析，您需要切换到我们[提供的 Docker 容器](https://deeplabcut.github.io/DeepLabCut/docs/docker.html)，或者您需要在 Anaconda 环境中[安装本地 GPU](https://deeplabcut.github.io/DeepLabCut/docs/recipes/installTips.html?highlight=gpu#how-to-confirm-that-your-gpu-is-being-used-by-deeplabcut)，或者使用 Google Colab（详见下文）：[![Open in Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/DeepLabCut/DeepLabCut/blob/master/examples/COLAB/COLAB_YOURDATA_TrainNetwork_VideoAnalysis.ipynb)

## 演示 3：在 Google Colaboratory 上进行 DeepLabCut 训练和分析（使用 Google 的 GPU！）

我们建议您“Fork”（派生）此仓库，`git clone` 或下载该文件夹到您的 Google Drive 中，然后将您的 Google 帐户链接到您的 GitHub（您将在下面的 Notebook 中看到如何操作）。然后，您也可以编辑 Notebook 以便处理您自己的数据（只需在您自己的仓库的网络地址前加上 `https://colab.research.google.com/` 即可）。

- 您可以使用 Google [Colaboratory](https://colab.research.google.com) 来演示在我们的数据上运行 DeepLabCut。这是一个可供 Colab 使用的 Jupyter Notebook 示例，用于开放区域数据。点击下面的徽章即可启动：[![Open in Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/DeepLabCut/DeepLabCut/blob/master/examples/COLAB/COLAB_DEMO_mouse_openfield.ipynb)

- 在您自己的数据上使用 Colab 进行新视频的训练和分析，即需要 GPU 的部分！
[![Open in Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/DeepLabCut/DeepLabCut/blob/master/examples/COLAB/COLAB_YOURDATA_TrainNetwork_VideoAnalysis.ipynb)

1. 点击“Open in Colab”以启动 Notebook。
2. 点击 Colab 工具栏中的“Connect”，然后点击“Runtime > Change Runtime Type > 并选择 Python3 和 GPU 作为您的硬件”，使 Notebook 保持活动状态。请遵循 Notebook 中的说明。
3. 请注意，他们通常不允许您在免费账户上长时间运行 GPU（>6 小时），因此请确保在此设置下将您的 ``save\_inters`` 变量设置得更低一些。

这是我们使用 Colab Notebook 的演示：https://www.youtube.com/watch?v=qJGs8nxx80A 和 https://www.youtube.com/watch?v=j13aXxysI2E

*警告：Colab 会更新其 CUDA/TensorFlow，速度可能比我们跟进的速度快，因此在未来某个时间点这段代码可能完全无法工作（并且，提醒您一下，此软件包的发布附带的 [LICENSE](/LICENSE) 意味着不承担任何责任，也不提供任何担保）。*

## 使用 3D DeepLabCut：

准备将您的姿态估计提升到一个新的维度了吗？从 2.0.7+ 版本开始，我们在包内支持 3D 功能。请查看上面专门的 `3D_Demo_DeepLabCut.ipynb` 了解更多详情！

## 使用 DLC 模型库 (Model Zoo)：

我们提供了一个 COLAB Notebook，用于使用针对特定动物/场景训练的、数量不断增长的网络。请在此处阅读更多信息：http://www.mousemotorlab.org/dlc-modelzoo。此代码还将创建一个新的项目文件夹，以便您可以改进、添加新的身体部位 (bodyparts) 或标记其他对象，然后重新训练。在此处启动 COLAB：[![Open in Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/DeepLabCut/DeepLabCut/blob/master/examples/COLAB/COLAB_DLC_ModelZoo.ipynb)

## 使用 Python/iPython：

DeepLabCut 的所有功能都可以从程序 **terminal (终端)** 上的 ipython 控制台中运行！请前往 [此处](/docs/UseOverviewGuide.md) 获取详细说明！

我们还提供了一些视频教程，演示我们如何通过终端使用 Anaconda 和 Docker：

 https://www.youtube.com/watch?v=7xwOhUcIGio 和  https://www.youtube.com/watch?v=bgfnz1wtlpo


* 您可以下载 DeepLabCut 和相关文件：

要在您自己的计算机上安装 DeepLabCut，我们建议使用 **Anaconda 来安装 Python 和 Jupyter Notebooks，请参阅 [Installation](/docs/installation.md) 页面**。然后在本地机器上，使用这些 Notebook 进行指导，您可以 (1) 演示我们已标记的数据（或创建您自己的数据），(2) 创建一个项目，提取要标记的帧，使用 GUI 进行标记，并为神经网络创建训练集。

我们建议您“Fork”此仓库，和/或将 DeepLabCut 文件放在一个本地文件夹中：
``git clone https://github.com/DeepLabCut/DeepLabCut``
这样您就可以使用 **Anaconda** 在本地访问它。您也可以点击“download”（下载）按钮，而不是使用 ``git``。然后您可以随意编辑 Notebook！