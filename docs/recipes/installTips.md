(installation-tips)=
# 安装技巧

## 如何直接从 GitHub 使用最新更新

我们通常会在 GitHub 上更新 `deeplabcut` 的主分支代码库，大约每月会推送一个稳定版本到 PyPI。这是大多数用户日常使用的版本（例如，你通过 `pip install deeplabcut` 安装的就是 PyPI 上的代码！）。但有时，我们会在代码库中添加尚未集成的功能，或者您可能希望自己编辑代码。这里我们将介绍如何实现这一目标。

### 方法 1：

如果您只是想*使用*最新的代码，您可以使用 `pip` 并通过修改和运行以下命令来添加特定的标签（tags），例如 `gui` 等：

```
pip install --upgrade 'git+https://github.com/deeplabcut/deeplabcut.git#egg=deeplabcut[gui]'
```

这将下载并更新 `deeplabcut` 及其任何与新版本不匹配的依赖项。如果您想强制将所有依赖项也升级到最新可用版本，请使用额外的 `--upgrade-strategy eager` 标志，如下所示：

```
pip install --upgrade --upgrade-strategy eager 'git+https://github.com/deeplabcut/deeplabcut.git#egg=deeplabcut[gui]'
```

### 方法 2：

如果您希望能够*编辑* DeepLabCut 的源代码，例如添加一个新功能或修复一个 🐛（bug），那么您需要“克隆”（clone）源代码：

**步骤 1：**

- 将代码库克隆（clone）到您计算机上的一个文件夹中：

- 点击此绿色按钮并复制链接：

![](https://images.squarespace-cdn.com/content/v1/57f6d51c9f74566f55ecf271/1581984907363-G8AFGX4V20Y1XD1PSZAK/ke17ZwdGBToddI8pDm48kGJBV0_F4LE4_UtCip_K_3lZw-zPPgdn4jUwVcJE1ZvWEtT5uBSRWt4vQZAgTJucoTqqXjS3CfNDSuuf31e0tVE0ejQCe16973Pm-pux3j5_Oqt57D2H0YbaJ3tl8vn_eR926scO3xePJoa6uVJa9B4/gitclone.png?format=500w)

- 然后在终端中输入：`git clone https://github.com/DeepLabCut/DeepLabCut.git`

**步骤 2：**

- 现在您将在该克隆（cloned）文件夹内从终端工作：

![](https://images.squarespace-cdn.com/content/v1/57f6d51c9f74566f55ecf271/1581985288123-V8XUAY0C0ZDNJ5WBHB7Y/ke17ZwdGBToddI8pDm48kIsGBOdR9tS_SxF6KQXIcDtZw-zPPgdn4jUwVcJE1ZvWQUxwkmyExglNqGp0IvTJZUJFbgE-7XRK3dMEBRBhUpz3c8X74DzCy4P3pv-ZANOdh-3ZL9iVkcryTbbTskaGvEc42UcRKU-PHxLXKM6ZekE/terminal.png?format=750w)

- 现在，当您启动 `ipython` 并执行 `import deeplabcut` 时，您导入的是 "deeplabcut" 文件夹 —— 因此您所做的任何更改，或者我们在将其添加到 pip 包之前所做的任何更改，都在这里。

- 您还可以通过运行以下命令来检查您正在导入哪个 `deeplabcut`：`deeplabcut.__file__`

![](https://images.squarespace-cdn.com/content/v1/57f6d51c9f74566f55ecf271/1581985466026-94OCSZJ5TL8U52JLB5VU/ke17ZwdGBToddI8pDm48kNdOD5iqmBzHwUaWGKS6qHBZw-zPPgdn4jUwVcJE1ZvWQUxwkmyExglNqGp0IvTJZUJFbgE-7XRK3dMEBRBhUpyQPoegsR7K4odW9xcCi1MIHmvHh95_BFXYdKinJaRhV61R4G3qaUq94yWmtQgdj1A/importlocal.png?format=750w)

如果您对代码进行了更改/首次使用代码，请确保运行 `./resinstall.sh`，您可以在主 DeepLabCut 文件夹中找到它：

![](https://images.squarespace-cdn.com/content/v1/57f6d51c9f74566f55ecf271/1609353210708-FRNREI7HUNS4GLDSJ00G/ke17ZwdGBToddI8pDm48kAya1IcSd32bok4WHvykeicUqsxRUqqbr1mOJYKfIPR7LoDQ9mXPOjoJoqy81S2I8N_N4V1vUb5AoIIIbLZhVYy7Mythp_T-mtop-vrsUOmeInPi9iDjx9w8K4ZfjXt2dq18t0tDkB2HMfL2JGcLHN27k5rSOPIU8nEAZT0p1MiSCjLISwBs8eEdxAxTptZAUg/Screen+Shot+2020-12-30+at+7.33.16+PM.png?format=2500w)

然后，您可以使用 `deeplabcut.__version__` 查看您拥有的版本号。

如果您进行了更改，您还可以利用我们的测试脚本。运行此处找到的所需测试脚本（您需要先进行 git clone）：https://github.com/DeepLabCut/DeepLabCut/blob/master/examples/.

例如：

```
# 使用 PyTorch 引擎进行测试
python testscript_pytorch_multi_animal.py

# 使用 TensorFlow 引擎进行测试
python testscript_multianimal.py
```

## 在 Ubuntu 18.04 LTS 上安装

### 以下是我们建议在全新安装的计算机（Ubuntu 18.04 LTS）上轻松安装的技巧。

安装 gcc：

```bash
sudo apt install gcc
```

然后，从这里下载 CUDA 10：https://developer.nvidia.com/cuda-downloads 并按照说明操作... 例如：

```bash
wget http://developer.download.nvidia.com/compute/cuda/10.1/Prod/local_installers/cuda_10.1.243_418.87.00_linux.run
sudo sh cuda_10.1.243_418.87.00_linux.run
```

但有一个例外，我在之后（`afterwards`）还执行了以下操作：

```bash
sudo add-apt-repository ppa:graphics-drivers/ppa
sudo apt update
sudo ubuntu-drivers autoinstall
```

**然后重启（reboot）**

检查 gcc 版本：

```bash
gcc --version
```

输出：
```
gcc (Ubuntu 7.3.0-27ubuntu1~18.04) 7.3.0
Copyright (C) 2017 Free Software Foundation, Inc.
This is free software; see the source for copying conditions.  There is NO
warranty; not even for MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE
```
然后：
```bash
sudo apt install nvidia-cuda-toolkit gcc-7
```

```bash
nvcc --version
```

然后您可以检查：

```bash
nvidia-smi
```

您将看到驱动程序版本、CUDA 版本以及显卡状态。

## 在 Ubuntu 20.04 LTS 上安装

大家好！这是另一篇关于如何在全新安装的 20.04 LTS 系统上为 DLC（DeepLabCut）配置环境的指南。主要包括 CUDA、驱动程序、Docker 和 Anaconda！

### 让我们从 GPU 的 CUDA 支持开始：

`sudo apt install gcc`

然后：

```python

wget https://developer.download.nvidia.com/compute/cuda/repos/ubuntu2004/x86_64/cuda-ubuntu2004.pin
sudo mv cuda-ubuntu2004.pin /etc/apt/preferences.d/cuda-repository-pin-600
wget https://developer.download.nvidia.com/compute/cuda/11.3.1/local_installers/cuda-repo-ubuntu2004-11-3-local_11.3.1-465.19.01-1_amd64.deb
sudo dpkg -i cuda-repo-ubuntu2004-11-3-local_11.3.1-465.19.01-1_amd64.deb
sudo apt-key add /var/cuda-repo-ubuntu2004-11-3-local/7fa2af80.pub
sudo apt-get update
sudo apt-get -y install cuda

```

然后：
```bash
sudo add-apt-repository ppa:graphics-drivers/ppa
sudo apt update
sudo ubuntu-drivers autoinstall
```

然后：

`reboot` （重启）

重新打开终端并检查 gcc 版本：

`gcc --version`

输出：
```python
gcc --version
gcc (Ubuntu 9.3.0-17ubuntu1~20.04) 9.3.0
Copyright (C) 2019 Free Software Foundation, Inc.
This is free software; see the source for copying conditions.  There is NO
warranty; not even for MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.
```
然后完成安装：

`sudo apt install nvidia-cuda-toolkit gcc-9`

然后检查：

`nvcc --version`

一切就绪！如果出现错误信息，请仔细阅读，因为它们通常会告诉您如何修复，或者需要搜索什么 :D

现在您可以看到 CUDA、驱动程序（DRIVER）、GPU(s)：

`nvidia-smi`

输出：

```python
nvidia-smi
Tue Jun 22 18:46:26 2021
+-----------------------------------------------------------------------------+
| NVIDIA-SMI 465.19.01    Driver Version: 465.19.01    CUDA Version: 11.3     |
|-------------------------------+----------------------+----------------------+
| GPU  Name        Persistence-M| Bus-Id        Disp.A | Volatile Uncorr. ECC |
| Fan  Temp  Perf  Pwr:Usage/Cap|         Memory-Usage | GPU-Util  Compute M. |
|                               |                      |               MIG M. |
|===============================+======================+======================|
|   0  NVIDIA GeForce ...  On   | 00000000:0B:00.0  On |                  N/A |
|  0%   46C    P8    11W / 200W |    252MiB /  8116MiB |      5%      Default |
|                               |                      |                  N/A |
+-------------------------------+----------------------+----------------------+

```

### 接下来是 Docker！

```bash
sudo apt-get install \
    apt-transport-https \
    ca-certificates \
    curl \
    gnupg \
    lsb-release
```
添加密钥：`curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg`

```bash
echo \
  "deb [arch=amd64 signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```
然后：
```bash
sudo apt-get update
sudo apt-get install docker-ce docker-ce-cli containerd.io
```

进行一些清理：
`sudo apt autoremove`

现在您可以运行 `sudo docker run hello-world`

并得到：
```
Hello from Docker!
This message shows that your installation appears to be working correctly.

To generate this message, Docker took the following steps:
 1. The Docker client contacted the Docker daemon.
 2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
    (amd64)
 3. The Docker daemon created a new container from that image which runs the
    executable that produces the output you are currently reading.
 4. The Docker daemon streamed that output to the Docker client, which sent it
    to your terminal.

To try something more ambitious, you can run an Ubuntu container with:
 $ docker run -it ubuntu bash

Share images, automate workflows, and more with a free Docker ID:
 https://hub.docker.com/

For more examples and ideas, visit:
 https://docs.docker.com/get-started/
```

### 接下来是 Anaconda！

点击此处获取 ubuntu/linux 安装包：https://www.anaconda.com/products/individual#linux

这会下载一个文件，将其保存（我保存在 Downloads 文件夹中）

然后 `cd Downloads`:

并运行：

`bash Anaconda3-2021.05-Linux-x86_64.sh`

您将看到：
```python
Welcome to Anaconda3 2021.05

In order to continue the installation process, please review the license
agreement.
Please, press ENTER to continue
>>>
```

遵循提示！

### 接下来是 DeepLabCut！

鉴于这是一个全新安装，我还需要安装一些东西：`sudo apt install libcanberra-gtk-module libcanberra-gtk3-module`

我们强烈建议 Ubuntu 用户使用 Docker (https://hub.docker.com/r/deeplabcut/deeplabcut) —— 这样环境的可复现性要高得多。

如果您想使用我们的 conda 文件，我继续执行了以下操作：

我从 www.deeplabcut.org 网站上获取 conda 文件。只需点击下载。对我来说，它进入了 Downloads 文件夹。

所以我打开一个终端，`cd Downloads`，然后运行：`conda env create -f DEEPLABCUT.yaml`

遵循提示！

## 故障排除：注意，如果您因为 wxPython 导致构建失败（注意，这在 Ubuntu 18、16 等版本上不会发生），例如：

```{warning}
DeepLabCut 不再使用 `wxpython` 来运行其 GUI —— 如果您遇到此类错误，
您很可能正在安装旧版本的 DeepLabCut。
```

```python
ERROR: Command errored out with exit status 1: /home/mackenzie/anaconda3/envs/DLC-GPU/bin/python -u -c 'import io, os, sys, setuptools, tokenize; sys.argv[0] = '"'"'/tmp/pip-install-0jsmkrr1/wxpython_aeff462b2060421a9cf65df55f63f126/setup.py'"'"'; __file__='"'"'/tmp/pip-install-0jsmkrr1/wxpython_aeff462b2060421a9cf65df55f63f126/setup.py'"'"';f = getattr(tokenize, '"'"'open'"'"', open)(__file__) if os.path.exists(__file__) else io.StringIO('"'"'from setuptools import setup; setup()'"'"');code = f.read().replace('"'"'\r\n'"'"', '"'"'\n'"'"');f.close();exec(compile(code, __file__, '"'"'exec'"'"'))' install --record /tmp/pip-record-pzy9q5u2/install-record.txt --single-version-externally-managed --compile --install-headers /home/mackenzie/anaconda3/envs/DLC-GPU/include/python3.7m/wxpython Check the logs for full command output.

failed

CondaEnvException: Pip failed
```
您可以选择：删除 conda 环境：`conda remove --name DEEPLABCUT --all`，打开 DLC-GPU.yaml 文件（使用任何文本编辑器！），并将 `deeplabcut[gui]` 更改为 `deeplabcut`。然后再次运行：`conda env create -f DEEPLABCUT.yaml`...

然后您将得到：
```python

 Successfully uninstalled decorator-5.0.9
Successfully installed PyWavelets-1.1.1 absl-py-0.13.0 astor-0.8.1 bayesian-optimization-1.2.0 chardet-4.0.0 click-8.0.1 cycler-0.10.0 cython-0.29.23 decorator-4.4.2 deeplabcut-2.1.10.4 filterpy-1.4.5 gast-0.2.2 google-pasta-0.2.0 grpcio-1.38.1 h5py-2.10.0 idna-2.10 imageio-2.9.0 imageio-ffmpeg-0.4.4 imgaug-0.4.0 intel-openmp-2021.2.0 joblib-1.0.1 keras-applications-1.0.8 keras-preprocessing-1.1.2 kiwisolver-1.3.1 llvmlite-0.34.0 markdown-3.3.4 matplotlib-3.1.3 moviepy-1.0.1 msgpack-1.0.2 msgpack-numpy-0.4.7.1 networkx-2.5.1 numba-0.51.1 numexpr-2.7.3 numpy-1.17.5 opencv-python-4.5.2.54 opencv-python-headless-3.4.9.33 opt-einsum-3.3.0 pandas-1.2.5 patsy-0.5.1 pillow-8.2.0 proglog-0.1.9 protobuf-3.17.3 psutil-5.8.0 pytz-2021.1 pyyaml-5.4.1 requests-2.25.1 ruamel.yaml-0.17.9 ruamel.yaml.clib-0.2.2 scikit-image-0.18.1 scikit-learn-0.24.2 scipy-1.7.0 statsmodels-0.12.2 tables-3.6.1 tabulate-0.8.9 tensorboard-1.15.0 tensorflow-estimator-1.15.1 tensorflow-gpu-1.15.5 tensorpack-0.9.8 termcolor-1.1.0 threadpoolctl-2.1.0 tifffile-2021.6.14 tqdm-4.61.1 urllib3-1.26.5 werkzeug-2.0.1 wrapt-1.12.1

done
#
# To activate this environment, use
#
#     $ conda activate DEEPLABCUT
#
# To deactivate an active environment, use
#
#     $ conda deactivate
```

激活！`conda activate DEEPLABCUT` 然后运行：`conda install -c conda-forge wxpython`。

然后运行 `python -m deeplabcut` 启动 DLC GUI。

## DeepLabCut MacOS M 芯片安装环境说明：

这仅假设您已经安装了 Anaconda。如果您使用的是较新的 MacBook（配备 M1、M2、M3、M4 芯片或更新型号），请使用 `DEEPLABCUT_M1.yaml` conda 文件，并遵循以下步骤：

(1) 克隆（git clone）deeplabcut 代码库：

```bash
git clone https://github.com/DeepLabCut/DeepLabCut.git
```

(2) 在程序终端中运行：`cd DeepLabCut/conda-environments`

(3) 然后，运行：

```bash
conda env create -f DEEPLABCUT.yaml
```

(4) 最后，激活您的环境，并启动带有 GUI 的 DLC：

```bash
conda activate DEEPLABCUT
python -m deeplabcut
```

GUI 将打开。当然，您也可以在无头模式（headless mode）下运行 DeepLabCut。

如果您**想使用 TensorFlow 引擎**，您需要使用 DeepLabCut 安装 `apple_mchips` 附加组件。您可以通过运行以下命令来实现：

```bash
pip install deeplabcut[apple_mchips]
```

## 如何确认您的 GPU 被 DeepLabCut 使用

在训练和分析步骤中，DeepLabCut 不会大量使用 GPU 处理器。要确认 DeepLabCut 正在正确使用您的 GPU：

**在 Windows 上**：

(1) 打开任务管理器。如果看起来像下图所示，请点击“更多详细信息 (More Details)”

![](https://images.squarespace-cdn.com/content/v1/57f6d51c9f74566f55ecf271/a0db3157-2228-4444-8084-36801659f272/installBrandon1.png?format=500w)

(2) 这将显示以下内容，这仍然没有帮助，并引起了用户的困惑。%GPU 不反映 DeepLabCut 的使用情况。

![](https://images.squarespace-cdn.com/content/v1/57f6d51c9f74566f55ecf271/117e3573-60bb-4599-b00b-c75276b24173/installBrandon2.png?format=500w)

(3) 点击**“性能 (Performance)”** 选项卡。在该页面上，点击 GPU 下方的小箭头（它可能显示为 **“3D”**），并将其更改为 **“CUDA”**。

(4) 在训练期间，您应该会看到**“专用 GPU 内存使用量 (Dedicated GPU memory usage)”** 增加到接近最大值，并且您应该在 **“CUDA”** 图表中看到活动。下图是在运行 `testscript.py` 时的活动情况。

![](https://images.squarespace-cdn.com/content/v1/57f6d51c9f74566f55ecf271/b1d03ca0-f8ba-4a31-a399-6e86856c81b0/installBrandon3.png?format=500w)

(5) 如果在训练期间没有看到该处的活动，那么您的 GPU 可能未为 DeepLabCut 正确安装。请返回安装说明，并确保您安装了 CUDA 11+，并在安装 DeepLabCut 后运行了 `conda install cudnn -c conda-forge`。

## 如何在 Windows 上为 TensorFlow 引擎安装 Intel 和 AMD GPU 上的 DeepLabCut

如果您使用的是 Windows 10/11，并且拥有任何供应商（AMD、Intel 或 Nvidia）的 DirectX 12 兼容 GPU，您可以使用 GPU 加速进行推理，并且安装在设备之间是一致的。此方法使用 [Tensorflow-directml](https://github.com/microsoft/tensorflow-directml)，它使用 DirectML 而不是 Cuda 进行 ML 训练和推理。

要检查已安装 GPU 的 DirectX 版本，请在 Windows 搜索中键入 `dxdiag` 并选择运行命令。在系统信息中，列表的最底部显示您的 DirectX 版本。除此之外，请确保您的标准 GPU 驱动程序是最新的。通过任何官方方式（Nvidia Geforce Experience、AMD Radeon 软件、直接从供应商网站）更新驱动程序都是可以的。

以下说明使用 conda 和 pip 进行环境管理，在安装 Anaconda Python 时安装的 Anaconda 提示程序（Prompt）中执行。`#` 行不应输入，它们仅用于指导：

```shell
conda create --name dlc_dml python=3.7
conda activate dlc_dml
# 验证版本（以 DLC 2.2.0.6 为准，但其他版本也可能有效）：
pip install 'deeplabcut[gui]'==2.2.0.6
pip install tensorflow-directml==1.15.5
pip install pip install imageio==2.9.0
conda install ffmpeg==4.2.2
```

注意：请注意，执行这些命令的顺序很重要，因为如果顺序错误，pip 的依赖项管理器可能会将软件包版本更改为不正确的版本。