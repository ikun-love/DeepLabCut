# 技术（硬件）方面的注意事项

## 快速摘要：
在我们的安装页面 [tech-considerations-during-install] 中，我们强调使用标准安装进行 GPU 计算时，需要一块具有**至少 8 GB 内存**的 NVIDIA GPU。如果您使用的是 Intel 或 AMD GPU 并且在 Windows 系统上，可以选择替代的安装方法，该方法在 [installation tips 页面](installation-tips) 的“如何为 Intel 和 AMD GPU 安装 Deeplabcut”部分中有所说明。
请注意，此处会重复一些信息，这些信息将随着系统和硬件的更新而进行维护。

### 计算机：

作为参考，我们使用的示例是 Dell 工作站（79xx 系列），搭载 **Ubuntu 16.04 LTS、18.04 LTS 或 20.04 LTS**，并运行一个已安装了 TensorFlow 等软件的 Docker 容器（参考：https://github.com/DeepLabCut/Docker4DeepLabCut2.0）。

### 计算机硬件：

理想情况下，您应该使用性能强大的 GPU，其显存*至少*为 8GB，例如 [NVIDIA GeForce 1080 Ti、2080 Ti 或 3090](https://www.nvidia.com/en-us/shop/geforce/?page=1&limit=9&locale=en-us)。虽然 GPU 不是必需的，但在使用 CPU 时，ResNets 模型的（训练和评估）代码会**慢得多**（慢约 10 倍），而 MobileNets 和 EfficientNets 则稍快一些。不过，GPU 仍能为您带来巨大的速度提升。您也可以考虑使用云服务，例如 [Google Cloud/Amazon Web Services](https://github.com/DeepLabCut/DeepLabCut/issues/47) 或 Google Colaboratory。

### 摄像机硬件：

该软件对于跟踪来自任何摄像机（手机摄像头、灰度、彩色；在红外光下拍摄、不同制造商等）的数据都非常健壮。请参阅我们在 [网站](https://www.mousemotorlab.org/deeplabcut/) 上的演示。

### 软件：

**操作系统：** Linux (Ubuntu)、MacOS* (Mojave) 或 Windows 10。但是，作者强烈推荐使用 Ubuntu！*MacOS 不（容易）支持 NVIDIA GPU，因此我们仅建议在以下情况使用此选项：纯粹进行 CPU 使用，或者用户希望标记数据、精炼数据等，然后将项目推送到云资源上进行 GPU 计算步骤，或者使用 MobileNets。

**Anaconda/Python3：** Anaconda：一个免费和开源的 Python 编程语言发行版（可从 https://www.anaconda.com/ 下载）。DeepLabCut 使用 Python 3 编写（https://www.python.org/），与 Python 2 不兼容。

**TensorFlow 引擎：** 您需要 [TensorFlow](https://www.tensorflow.org/)。
我们在论文中使用了 1.0 版本，后续版本也可以与提供的代码一起使用（我们测试了**TensorFlow 版本 1.0 到 1.15，以及 2.0 到 2.12（Windows 上为 2.10）**；对于支持 GPU 的 Python 3.10，我们推荐使用 TF2.12 适用于 MacOS/Ubuntu，推荐使用 2.10 适用于 Windows）。

需要注意的是，虽然可以在 CPU 上运行 DeepLabCut，但它会**非常慢**（参见：[Mathis & Warren](https://www.biorxiv.org/content/early/2018/10/30/457242)）。然而，如果您想在购买 GPU 之前在自己的计算机/数据上测试 DeepLabCut，这是一个更理想的选择，同时安装过程也更直接！否则，请使用我们的 COLAB 笔记本以获得 GPU 测试访问权限。

Docker：我们强烈建议高级用户使用提供的 [Docker 容器](docker-containers)。

注意：[目前 Docker Desktop 中对 GPU 的支持仅在 Windows 且使用 WSL2 后端时可用。](https://docs.docker.com/desktop/features/gpu/)