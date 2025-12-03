(beginners-guide)=
# 使用 DeepLabCut

<img src="https://images.squarespace-cdn.com/content/v1/57f6d51c9f74566f55ecf271/1572296495650-Y4ZTJ2XP2Z9XF1AD74VW/ke17ZwdGBToddI8pDm48kMulEJPOrz9Y8HeI7oJuXxR7gQa3H78H3Y0txjaiv_0fDoOvxcdMmMKkDsyUqMSsMWxHk725yiiHCCLfrh8O1z5QPOohDIaIeljMHgDF5CVlOqpeNLcJ80NK65_fV7S1UZiU3J6AN9rgO1lHw9nGbkYQrCLTag1XBHRgOrY8YAdXW07ycm2Trb21kYhaLJjddA/DLC_logo_blk-01.png?format=1000w" width="150" title="DLC-live" alt="DLC LIVE!" align="right" vspace = "50">

本指南及相关页面，旨在为完全不熟悉 Python 的初学者提供一份关于 DeepLabCut (DLC) 的入门介绍。当您对本材料感到满意后，我们建议您跳转到更详细的用户指南！

- **专业提示 (ProTip):** 如果想获得更“深入”的理解，可以查看 [DeepLabCut 课程](https://deeplabcut.github.io/DeepLabCut/docs/course.html)，它将深入探讨 DeepLabCut 背后的科学原理。

## 安装

在开始之前，请确保您的系统已安装 DeepLabCut。

- **专业提示 (ProTip):** 对于面向更高级用户、更详细的安装说明，请参阅 [完整安装指南](https://deeplabcut.github.io/DeepLabCut/docs/installation.html)。

## 初学者用户指南
如果您是 Python 新手，为您的计算机安装 Python 的最佳方法是使用 Anaconda。请[到这里下载最适合您计算机的版本](https://www.anaconda.com/download)。

- “Conda”——正如它通常所称呼的——是一种在计算机上创建“环境 (env)”的非常好的方法。虽然可能会存在一些交叉影响，但总的来说，它允许您隔离完成科学研究所需的各种不同工具 💪。

## 让我们学习一点并创建一个 DeeplabCut 环境 (env):

安装 Anaconda 后，打开新安装的程序（Anaconda Terminal）。默认情况下，您将位于“root”（根）目录中。

**(0) 创建一个干净的 `conda 环境`** 

在终端中输入：

```
conda create -n deeplabcut python=3.10
```
系统将提示您确认安装 (y/n)，然后等待神奇的事情发生。最后，检查终端，它应该会提示您输入：

```
conda activate deeplabcut
```
现在，我们将安装核心依赖项。其工作原理是存在“包管理器”，例如 `conda` 本身以及 Python 的 `pip`。我们将根据我们所知在各种操作系统上可用的组合进行部署。

**(1) 安装 PyTorch**

`PyTorch` 是我们编写 DLC3 的后端深度学习语言。要选择正确的版本，请转到官方 PyTorch 文档中的 ["安装 PyTorch"](https://pytorch.org/get-started/locally/) 说明。选择您想要的 PyTorch 构建版本、操作系统，选择 `conda` 作为您的包管理器，选择 Python 作为语言。然后选择您的计算平台（CUDA 版本或仅 CPU）。最后，使用相应的命令安装 PyTorch 包。以下是一些可能的示例：

- **适用于 CUDA 12.4 的 GPU 版 pytorch**
```
pip install torch torchvision --index-url https://download.pytorch.org/whl/cu124
```
- **使用最新版本的纯 CPU 版 pytorch**
```
pip install torch torchvision --index-url https://download.pytorch.org/whl/cpu
```

**(2) 安装 DeepLabCut** 

太棒了！接下来，我们将安装 `Tables`（也称为 pytables），这是一个用于读取构成 DeepLabCut 数据管理骨干的 HDF5 文件的包，然后我们将安装所有 `deeplabcut` 的源代码 🔥。请决定您想要哪个版本（稳定版还是 Alpha 版），然后输入：

```
conda install -c conda-forge pytables==3.8.0
```

- **Alpha 发布版:**
```
pip install "git+https://github.com/DeepLabCut/DeepLabCut.git@pytorch_dlc#egg=deeplabcut[gui,modelzoo,wandb]"
```
- 或者运行以安装 **稳定发布版 (Stable release):**
```
pip install "deeplabcut[gui,modelzoo,wandb]"
```
- 如果您选择使用，这将为您提供 DeepLabCut、DLC GUI (gui)、我们最新的神经网络 (modelzoo) 以及一个很棒的数据记录器 (wandb)！

## 启动 DeepLabCut

在终端中，输入：
```bash
python -m deeplabcut
```
这将打开 DeepLabCut 应用程序（请注意，默认模式是深色模式，但您可以通过点击“appearance”进行更改：

![DeepLabCut GUI Screenshot](https://images.squarespace-cdn.com/content/v1/57f6d51c9f74566f55ecf271/1717779625875-5UHPC367I293CBSP8CT6/GUI-screenshot.png?format=500w)

> 💡 **注意:** 有关导航 DeepLabCut GUI 的视觉指南，请查看我们的 [YouTube 教程](https://www.youtube.com/watch?v=tr3npnXWoD4)。

## 创建新项目

### 首次启动 GUI 时的导航

当您首次启动 GUI 时，会看到三个主要的选项：

1. **Create New Project (创建新项目):** 专为新的启动工作而设计。如果您是来开始新项目的，这是一个很好的选择。
2. **Load Project (加载项目):** 使用此选项可以恢复您暂停或过去的工作。
3. **Model Zoo (模型库):** 最适合那些希望探索模型库的用户。

### 开始您的工作:

- 对于首次使用或新用户，请点击 **`Start New Project`**。

## 🐾 开始新项目的步骤

1. **启动新项目:**
   - 当您启动一个新项目时，将出现一个空白的项目窗口。在 DLC3+ 中，您会看到一个新的选项 "Engine"（引擎）。
   - 我们推荐使用 PyTorch 引擎：
  
 ![DeepLabCut Engine](https://images.squarespace-cdn.com/content/v1/57f6d51c9f74566f55ecf271/1717780414978-17LOVBUJ8JR102QVSFDY/Screen+Shot+2024-06-07+at+7.13.14+PM.png?format=1500w))

2. **填写项目详细信息:**
   - **命名您的项目:**
     - 为您的项目指定一个具体、定义明确的名称。
      
      > **💡 提示:** 在您的项目名称中避免使用空格。

   - **命名实验者:**
     - 填写实验者的姓名。这部分数据将保持不变（不可变）。

3. **确定项目位置:** 
   - 默认情况下，您的项目将位于 **桌面 (Desktop)**。
   - 如果需要选择其他主目录，请相应地修改路径。

4. **多动物还是单动物项目:**
   - 仅当项目模式匹配时，才在菜单中勾选“Multi-Animal”（多动物）选项。
   - 根据您的实验选择“Number of Cameras”（相机数量）。

5. **添加视频:**
   - 首先，点击窗口右侧的 **`Browse Videos` (浏览视频)** 按钮，以搜索视频内容。
   - 当媒体选择工具打开时，导航到包含视频的文件夹并选择它。
     
     > **💡 提示:** DeepLabCut 支持 **`.mp4`**, **`.avi`**, **`.mkv`** 和 **`.mov`** 文件格式。
   - 系统将创建一个包含该文件夹中所有视频的列表。
   - 取消选择您希望从项目中移除的视频。
     
6. **创建项目:**
   - 点击主窗口右下角的 **`Create` (创建)** 按钮。
   - 在您在上面选择的位置，将创建一个与您的项目名称同名的新文件夹。
     

### 📽 视频教程：在 DeepLabCut 中设置项目

![DeepLabCut Create Project GIF](https://images.squarespace-cdn.com/content/v1/57f6d51c9f74566f55ecf271/1717779616437-30U5RFYV0OY6ACGDG7F4/create-project.gif?format=500w)

## 下一步，请转到[设置要跟踪的关键点](https://deeplabcut.github.io/DeepLabCut/docs/manage-project)的初学者指南