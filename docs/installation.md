(how-to-install)=
# DeepLabCut 安装指南

- **只要您安装了 Python 3.10，DeepLabCut 就可以在 Windows、Linux 或 MacOS 上运行**
  - （另请参阅 [技术注意事项](tech-considerations-during-install) 和安装遇到问题时，请查看 [安装技巧](https://deeplabcut.github.io/DeepLabCut/docs/recipes/installTips.html) 页面）。
- 🚧 请注意，有几种安装模式：
  - 请决定使用基于 [**conda 环境**](https://deeplabcut.github.io/DeepLabCut/docs/installation.html#conda-the-installation-process-is-as-easy-as-this-figure) 的安装（**推荐**），
  - 或使用提供的 [**Docker 容器**](docker-containers)（推荐给 Ubuntu 高级用户）。
- 🚀 请注意，使用 **GPU** 将获得最佳性能！
  - 请参阅 [GPU 支持](https://deeplabcut.github.io/DeepLabCut/docs/installation.html#gpu-support) 部分以安装您的 GPU 驱动程序和 CUDA。

```{Hint} 熟悉 python 包和 conda 吗？快速安装指南：

这假设您已经安装了 `conda`/`mamba`，并且这将在一个全新的环境中安装 DeepLabCut。如果您有 NVIDIA GPU，请按照[他们的说明](https://pytorch.org/get-started/locally/)（使用您想要的 CUDA 版本）安装 PyTorch——您只需要安装好 GPU 驱动程序即可。

```bash
conda create -n DEEPLABCUT python=3.12
conda activate DEEPLABCUT
conda install -c conda-forge pytables==3.8.0

# 使用您想要的 CUDA 版本安装 PyTorch（或仅用于 CPU） - 查看[他们的](https://pytorch.org/get-started/locally/)网站：
# 用于 CUDA 11.3 的 GPU 版本 PyTorch
conda install pytorch cudatoolkit=11.3 -c pytorch


# 安装最新版本的 DeepLabCut
pip install --pre deeplabcut
# 如果你想使用 GUI
pip install --pre deeplabcut[gui]

# **仅当您有 CUDA GPU 时** - 检查 PyTorch 是否可以访问您的 GPU；这应该打印 `True`
python -c "import torch; print(torch.cuda.is_available())"
```

- 为什么我们使用 `conda` 而不是 `pip` 来安装 [pytables](https://www.pytables.org/usersguide/installation.html)？ 因为它需要一些并非所有用户都已安装的库，而 conda 会确保这些库也已安装。

- 如果您熟悉命令行并希望获得 TensorFlow 支持，请查看[下方](deeplabcut-with-tf-install)的全新安装说明，这对我们（在 Linux 上）有效，并且可以使 GPU 同时用于 PyTorch 和 TensorFlow。


## CONDA：安装过程就像这张图一样简单！ -->

 <img src="https://images.squarespace-cdn.com/content/v1/57f6d51c9f74566f55ecf271/71e5d954-75a0-4534-9fa6-7ecc4bf1b76d/installDLC.png?format=1500w" width="250" title="DLC" alt="DLC" align="right" vspace = "50">

### 🚨 在使用我们的 conda 文件之前，您有 GPU 吗？
````{admonition} 🚨 点击此处获取更多信息！
:class: dropdown
- 如果可能，我们建议使用 GPU！
- 您**需要决定是使用 CPU 还是 GPU 来处理您的模型**：（注意，您也可以仅使用 CPU 进行项目管理和数据标注！然后，例如，免费使用 Google Colaboratory GPU（在此[处](https://github.com/DeepLabCut/DeepLabCut/tree/master/examples#demo-4-deeplabcut-training-and-analysis-on-google-colaboratory-with-googles-gpus)阅读更多信息，并且在[我们的 YouTube 频道](https://www.youtube.com/playlist?list=PLjpMSEOb9vRFwwgIkLLN1NmJxFprkO_zi)上有很多辅助视频）。

  - **CPU？** 很好，请跳到下面的下一部分！

  - **NVIDIA GPU？** 如果您想使用自己的 GPU（即工作站中有 GPU），那么您需要确保安装了兼容 CUDA 的 GPU、CUDA 和 cuDNN。请注意，您安装哪个 CUDA 版本取决于您想使用哪个 PyTorch 版本。因此，请仔细查看下面的“GPU 支持”。**注意，DeepLabCut 紧跟最新的 CUDA 和 PyTorch！**
  
  - **Apple M 芯片 GPU？** 确保安装 miniconda3，系统将默认使用您的 GPU。
````

### 步骤 1：通过 Anaconda 安装 Python

### 安装 [anaconda](https://docs.conda.io/projects/conda/en/latest/user-guide/install/index.html#)，或为 MacOS 用户使用 miniconda3（见下文）

- Anaconda 是在各种操作系统上安装 Python 和其他软件包的便捷方法。使用 Anaconda，您可以在计算机上的一个[环境](https://conda.io/docs/user-guide/tasks/manage-environments.html)中创建所有依赖项。

```{Hint}
下载适用于您操作系统的 anaconda：[anaconda.com/download/
](https://www.anaconda.com/download/)
```

- 如果您在 MacBook 上使用 M1 或 M2 芯片，并安装了 v12.5+（通常是 2020 年或更新的设备），我们推荐使用 **miniconda3**，它的原理与 anaconda 相同。这非常直接，并且在 [https://docs.conda.io/projects/conda/en/latest/user-guide/install/macos.html](https://docs.conda.io/projects/conda/en/latest/user-guide/install/macos.html) 中有详细说明。简而言之，打开“终端”程序，复制/粘贴并运行下面提供的代码。

### 💡 Mac 用户的 miniconda
````{admonition} 点击按钮查看 Mac 版 miniconda 的代码
:class: dropdown
wget https://repo.anaconda.com/miniconda/Miniconda3-py310_4.12.0-MacOSX-arm64.sh -O ~/miniconda.sh
bash ~/miniconda.sh -b -p $HOME/miniconda
source ~/miniconda/bin/activate
conda init zsh
````

### 步骤 2：使用我们的 Conda 文件构建一个环境！

您只需将此 `.yaml` 文件保存在本地计算机的任何位置即可。所以，让我们下载它！

```{Hint}
Windows 用户：请确保您已安装 `git` 以及 anaconda：https://gitforwindows.org/
```

- **直接下载 CONDA 文件的方法：**

  - 点击 ➡️ [CONDA 文件](https://github.com/DeepLabCut/DeepLabCut/blob/main/conda-environments/DEEPLABCUT.yaml#:~:text=Raw%20file%20content-,Download,-%E2%8C%98)，然后点击“...”并选择“下载”
    <img width="274" alt="Screen Shot 2023-09-13 at 10 33 32 PM" src="https://github.com/DeepLabCut/DeepLabCut/assets/28102185/ec4295a5-e85c-4ce7-8c16-e6517a2cfa22">

- **现在，在终端（或 Windows 用户的 Anaconda 命令提示符）中，如果点击了下载，请转到您的下载文件夹。**

```{Hint}
Windows 用户：请确保使用**右键单击**、“以管理员身份运行”打开终端/cmd/anaconda 提示符
```

```{Hint}
:class: dropdown
如果您将仓库克隆到了桌面上，该命令可能如下所示：
``cd C:\Users\YourUserName\Desktop\DeepLabCut\conda-environments``
在 (Windows) 中，您可以按住 SHIFT 并右键单击 > 复制为路径；在 (Mac) 中，右键单击并在菜单中按住 OPTION 键以显示“复制为路径名”。
```
确保您位于包含 `.yaml` 文件的文件夹中，然后运行：

``conda env create -f DEEPLABCUT.yaml``


- 现在您可以在计算机上的任何位置使用此环境（即无需返回 conda- 文件夹）。只需运行以下命令进入您的环境：
     - Ubuntu/MacOS: ``source/conda activate nameoftheenv``（例如在 Mac 上：``conda activate DEEPLABCUT``）
     - Windows: ``activate nameoftheenv``（即 ``activate DEEPLABCUT``）

现在，您应该在终端屏幕左侧看到 (`nameofenv`)，例如 ``(DEEPLABCUT) YourName-MacBook...``
注意：无需运行 `pip install deeplabcut`，因为它已安装！！！ :)

(deeplabcut-with-tf-install)=
### 💡 注意：DeepLabCut 中的 PyTorch 和 TensorFlow 支持

````{admonition} DeepLabCut TensorFlow 支持
:class: dropdown
截至 2024 年 6 月，我们有一个 PyTorch 引擎后端，我们将在 2024 年底弃用 TensorFlow 后端。目前，如果您想使用 TensorFlow，您需要在 conda 环境中运行 `pip install deeplabcut[tf]` 来安装正确版本的 TensorFlow。请注意，我们将提供错误修复，但对于超过 2.10 (Windows) 和其他操作系统 2.12 版本的 TensorFlow，我们将不再支持新版本。

安装 TensorFlow 并使其能够访问 GPU 有点棘手。请查看 TensorFlow 的[兼容性矩阵](https://www.tensorflow.org/install/source#gpu)以了解您应该安装哪个版本的 CUDA 和 cuDNN。

我们发现在 Linux 用户中，使用以下命令安装 DeepLabCut 可以很好地在 Conda 环境中安装 PyTorch 2.3.1、TensorFlow 2.12、CUDA 11.8 和 cuDNN 8：

```bash
conda create -n deeplabcut-with-tf "python=3.10"
conda activate deeplabcut-with-tf

# 安装所需的 TensorFlow 版本，版本基于 CUDA 11.8 和 cuDNN 8
pip install "tensorflow==2.12" "tensorpack>=0.11" "tf_slim>=1.1.0"

# 使用基于 CUDA 11.8 和 cuDNN 8 的版本安装 PyTorch
pip install "torch==2.3.1" torchvision --index-url https://download.pytorch.org/whl/cu118

# 为 TensorFlow 创建到 NVIDIA 共享库的符号链接
# -> 如其安装文档中所述：
#      https://www.tensorflow.org/install/pip#step-by-step_instructions

pushd $(dirname $(python -c 'print(__import__("tensorflow").__file__)'))
ln -svf ../nvidia/*/lib/*.so* .
popd

pip install  --pre deeplabcut
```
````

**太棒了，就是这样！DeepLabCut 已安装！** 🎉💜


### 步骤 3：真的，就是这样！让我们运行 DeepLabCut

请前往[用户指南概述](https://deeplabcut.github.io/DeepLabCut/docs/UseOverviewGuide.html)了解相关信息。

🎉 在新环境中启动 DeepLabCut，方法是运行 `python -m deeplabcut`

## 安装 DeepLabCut 的其他方式和附加提示

### 另一种方法是 git 克隆此仓库并从源安装！
即，如果下载失败或您只是想方便地获取源代码！

- **Windows/Linux/MacBooks:** 在终端/cmd 程序中，在您想要放置 DeepLabCut 的**文件夹中**克隆此仓库（输入：``git clone https://github.com/DeepLabCut/DeepLabCut.git``）。请注意，这可以在任何地方，甚至下载文件夹也可以。）
- 然后按照步骤 2 中的相同步骤进行操作，但要根据文件现在位于下载的文件夹中进行调整。

### PIP：

- 要在 DeepLabCut 中构建自定义模型所需的一切（即使用我们的源代码和我们的依赖项），可以通过 `pip install 'deeplabcut[gui]'`（用于带 PyTorch 的 GUI 支持）或不带 GUI 的方式安装：`pip install 'deeplabcut'`。
- 如果您想使用 SuperAnimal 模型，请使用 `pip install 'deeplabcut[gui,modelzoo]'`。

## DOCKER：

- 我们也有 docker 容器。Docker 是使用和部署代码最可重现的方式。请参阅我们的专用 docker 包和页面 [此处](https://deeplabcut.github.io/DeepLabCut/docs/docker.html)。

## 专业提示：

更多 [安装专业技巧](installation-tips) 也可供参考。

如果您想更新您的 DLC，一旦进入您的环境，只需运行 `pip install --upgrade deeplabcut`。如果您想使用特定的版本，则需要指定您想要的 [版本](https://deeplabcut.github.io/DeepLabCut/docs/installation.html#how-to-update-deeplabcut)，例如 `pip install deeplabcut==3.0`。安装后，您可以通过运行 `import deeplabcut` `deeplabcut.__version__` 来检查版本。不要害怕更新，DLC 与您的 2.0+ 项目向后兼容，并且性能每月都在提高并添加新功能。

**您在 2.X 版本中标记的所有数据也与 3+ 版本和 PyTorch 引擎兼容**！工作流程或标签处理方式没有变化：主要的更改发生在底层！如果您一直在使用 DeepLabCut 2.X 并想了解有关迁移到 PyTorch 引擎的更多信息，请查看我们关于[从 TensorFlow 迁移到 PyTorch](dlc3-user-guide) 的文档。

有关 conda 环境管理技巧，请参阅：[kapeli.com: Conda 备忘单](
https://kapeli.com/cheat_sheets/Conda.docset/Contents/Resources/Documents/index)

**专业提示：** 如果您想修改代码然后进行测试，您可以使用我们提供的测试脚本。这意味着您需要使用最新的基于 GitHub 的代码！请参阅[此处](installation-tips)了解如何获取最新的 GitHub 代码，以及如何通过观看此视频来测试您的安装：[https://www.youtube.com/watch?v=IOWtKn3l33s](https://www.youtube.com/watch?v=IOWtKn3l33s)。

## 创建您自己的定制 conda 环境（Linux 推荐的路线：Ubuntu, CentOS, Mint 等）

*注意：在全新的 Ubuntu 安装中，您通常需要运行：``sudo apt-get install gcc python3-dev`` 来安装 GNU 编译器集合和 python 开发环境。

有些用户可能希望创建自己的定制环境。- 这是一个示例。

在终端中输入：

`conda create -n DLC python=3.10`

**当前版本：** 您之后需要添加到环境中的唯一一项是 deeplabcut（`pip install deeplabcut`）或 `pip install 'deeplabcut[gui]'`，它有一个基于 napari 的 GUI。


## **GPU 支持：**

如果您有 NVIDIA GPU 和匹配的 NVIDIA CUDA+驱动程序，您**首先**需要做的**就是**这一件事。
- CUDA：https://developer.nvidia.com/cuda-downloads（只需按照这里的提示操作！）
- 驱动程序：https://www.nvidia.com/Download/index.aspx

### 新用户最常见的障碍是安装和使用 GPU，所以不要灰心！

**关键：** 如果您有 GPU，您应该首先**安装适用于您特定 GPU 的相应驱动程序**，然后您才能使用提供的 conda 文件。您需要一个兼容 CUDA 的 NVIDIA GPU。要查看支持 CUDA 的 NVIDIA GPU 列表，请[访问他们的网站](https://developer.nvidia.com/cuda-gpus)。

- 在这里，我们提供了有关如何安装和检查您的 GPU 如何与 TensorFlow 配合使用的说明（TensorFlow 被 DeepLabCut 使用，并且已与上述 Anaconda 文件一起安装）。因此，您无需单独安装 tensorflow。

**首先**，为您的 GPU 安装驱动程序。在以下位置查找驱动程序：
https://www.nvidia.com/download/index.aspx

- 通过在终端中输入以下内容来检查已安装的驱动程序：``nvidia-smi``。

**其次**，安装 CUDA：https://developer.nvidia.com/（注意 cuDNN，[https://developer.nvidia.com/cudnn](https://developer.nvidia.com/cudnn)，包含在 anaconda 环境文件中，因此您无需再次安装它）。

**第三：** 按照上述步骤获取 `DEEPLABCUT` conda 文件并进行安装！

### 注意事项：

- **从 3.0+ 版本开始，我们已迁移到 PyTorch。支持的最后一个 TensorFlow 版本是 2.10（Windows 用户）和其他系统上的 2.12（我们未测试超过此版本）。**
- 请注意，不同版本的 TensorFlow 需要不同的 CUDA 版本。
- 由于 TensorFlow 和 CUDA 的组合很重要，我们强烈建议您在[此 StackOverflow 帖子](https://stackoverflow.com/questions/30820513/what-is-version-of-cuda-for-nvidia-304-125/30820690#30820690)中**检查您的驱动程序/cuDNN/CUDA/TensorFlow 版本**。
- 要检查您的 GPU 是否正常工作，请在终端中运行：

`nvcc -V` 以检查您已安装的版本。

- 最佳实践是运行提供的 `testscript_pytorch_single_animal.py`（或 TensorFlow 引擎的 `testscript.py`）；它位于您通过 git 克隆仓库时获得的 `examples` 文件夹中。这里有更多信息/一个关于运行测试脚本的简短[视频](https://www.youtube.com/watch?v=IOWtKn3l33s)。
- 此外，如果您想使用最新的尖端代码，通过 git 克隆，您也获得了最新的代码。在主 DeepLabCut 文件夹中时，您可以运行 `./reinstall.sh` 以确保它已安装（更多信息见[此处](installation-tips)）
- 您可以通过这些附加[提示](https://www.tensorflow.org/programmers_guide/using_gpu)检查您的 GPU 是否被正确使用。
- Ubuntu 用户可能会发现此[安装指南](https://deeplabcut.github.io/DeepLabCut/docs/recipes/installTips.html#installation-on-ubuntu-20-04-lts)对全新的 Ubuntu 安装也很有帮助。

## 故障排除：

TensorFlow：
以下是一些用户认为有帮助的额外资源（未经认可发布）：

- https://stackoverflow.com/questions/30820513/what-is-the-correct-version-of-cuda-for-my-nvidia-driver/30820690

<p align="center">
<img src="https://static1.squarespace.com/static/57f6d51c9f74566f55ecf271/t/5c3e46ca1ae6cfbb5c5d1ee0/1547585235033/cuda_driver.png?format=750w" width="50%">
</p>

- https://www.tensorflow.org/install/source#gpu

- http://blog.nitishmutha.com/tensorflow/2017/01/22/TensorFlow-with-gpu-for-windows.html

- https://developer.nvidia.com/cuda-toolkit-archive

- http://www.python36.com/install-tensorflow-gpu-windows/


FFMPEG：

- 一些 Windows 用户报告需要根据此处所述内容重新安装 ffmpeg（在 Windows 更新后）：https://video.stackexchange.com/questions/20495/how-do-i-set-up-and-use-ffmpeg-in-windows（创建新视频时可能会出现错误）。在 Ubuntu 上，命令是：`sudo apt install ffmpeg`

DEEPLABCUT：

- 如果您 git 克隆或下载了此文件夹，并且位于其中，则 ``import deeplabcut`` 将从该位置而不是来自 PyPi 上的最新版本导入包！

(system-wide-considerations-during-install)=
## 系统范围的注意事项：

如果您执行系统范围的安装，并且计算机已安装其他 Python 包或 TensorFlow 版本发生冲突，这将覆盖它们。如果您有一台专用于 DeepLabCut 的计算机，这没问题。如果其他应用程序需要不同版本的库，可能会破坏那些应用程序。解决此问题的方法是创建一个虚拟环境，即包含特定版本 Python 安装和其他附加包的自包含目录。管理虚拟环境的一种方法是使用 conda 环境（这需要安装 Anaconda）。

(tech-considerations-during-install)=
## 技术注意事项：

- 计算机：

     - 供参考，我们使用例如 Dell 工作站（79xx 系列），配备 **Ubuntu 16.04 LTS, 18.04 LTS, 20.04 LTS, 22.04 LTS**；对于早于 2.2 的版本，我们运行一个安装了 TensorFlow 等的 Docker 容器（[https://github.com/DeepLabCut/Docker4DeepLabCut2.0](https://github.com/DeepLabCut/Docker4DeepLabCut2.0)）。现在我们使用此仓库中提供的新的 Docker 容器（仅限 Linux 支持），也可以通过 [DockerHub](https://hub.docker.com/r/deeplabcut/deeplabcut) 或 [`deeplabcut-docker`](https://pypi.org/project/deeplabcut-docker/) 辅助脚本获得。

- 计算机硬件：
     - 理想情况下，您将使用具有*至少* 8GB 内存的强大 NVIDIA GPU。不需要 GPU，但在 CPU 上（训练和评估）代码对于 ResNets 来说要慢得多（慢 10 倍），但 MobileNets 较快（参见 WIKI）。您也可以考虑使用云服务，如 [Google cloud/Amazon Web Services](https://github.com/DeepLabCut/DeepLabCut/issues/47) 或 Google Colaboratory。

- 摄像头硬件：
     - 该软件对来自任何摄像头的跟踪数据（手机摄像头、灰度、彩色；在红外光下拍摄、不同制造商等）非常稳健。请参阅[我们网站](https://www.mousemotorlab.org/deeplabcut/)上的演示。

- 软件：
     - 操作系统：Linux (Ubuntu)、MacOS* 或 Windows 10。但是，作者强烈建议使用 Ubuntu！*MacOS 不（容易）支持 NVIDIA GPU，因此我们仅建议将此选项用于 CPU 使用，或者用户希望标记数据、精炼数据等，然后将项目推送到云资源进行 GPU 计算步骤，或使用 MobileNets。
     - Anaconda/Python3：Anaconda：Python 编程语言的免费和开源发行版（从 [https://www.anaconda.com/](https://www.anaconda.com/) 下载）。DeepLabCut 使用 Python 3 编写（[https://www.python.org/](https://www.python.org/)），与 Python 2 不兼容。
     - `pip install deeplabcut`
     - TensorFlow
       - 如果您想使用预 3.0 的版本，您将需要 [TensorFlow](https://www.tensorflow.org/)（我们在 Nature Neuroscience 论文中使用了 1.0 版本，后续版本也与提供的代码兼容（我们测试了 **TensorFlow 版本 1.0 到 1.15，以及 2.0 到 2.10**；我们现在推荐 TF2.10）用于支持 GPU 的 Python 3.8、3.9、3.10。
        - 请注意，可以在 CPU 上运行 DeepLabCut，但会**非常慢**（参见：[Mathis & Warren](https://www.biorxiv.org/content/early/2018/10/30/457242)）。然而，如果您想在购买 GPU 之前在自己的计算机/数据上测试 DeepLabCut，这是首选方式，并且安装过程简单！否则，请使用我们的 COLAB 笔记本电脑进行 GPU 访问以进行测试。
     - Docker：我们强烈建议高级用户使用提供的 [Docker 容器](docker-containers)。