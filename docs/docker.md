(docker-containers)=
# DeepLabCut Docker 容器

从 DeepLabCut 2.2.0.2 版本及以后，我们在 [DockerHub](https://hub.docker.com/r/deeplabcut/deeplabcut) 上提供了 Docker 容器。使用 Docker 是使用 DeepLabCut 的一种替代方法，它只需要用户在机器上安装 [Docker](https://www.docker.com/)，而不是遵循为 Anaconda 设置分步安装指南。所有运行 DeepLabCut 所需的依赖项（用于在终端中运行或在预装了 DeepLabCut 的 Jupyter Notebook 中运行）都包含在所提供的 Docker 镜像中。

您可以使用 [`napari-deeplabcut` 标注 GUI](https://deeplabcut.github.io/DeepLabCut/docs/gui/napari_GUI.html) 来标注数据，但它**不能**在 Docker 容器中运行：应按照上述链接中的说明将其作为独立程序安装：`pip install napari-deeplabcut`（同时也请查看 [工作流程](https://deeplabcut.github.io/DeepLabCut/docs/gui/napari_GUI.html#workflow)！）。

高级用户可以直接访问 [DockerHub](https://hub.docker.com/r/deeplabcut/deeplabcut) 并使用其中提供的镜像。然而，为了方便入门使用这些镜像，我们提供了一个辅助工具 `deeplabcut-docker`，它可以使迁移到 Docker 镜像的过程更加便捷；要在您的机器上安装该工具，请运行

``` bash
$ pip install deeplabcut-docker
```

（可能在虚拟环境或现有的 Anaconda 环境中）。请注意，这**不会**干扰或安装 Tensorflow 或任何其他 DeepLabCut 依赖项到您的计算机上——Docker 容器与您现有的软件安装是完全隔离的！

## 使用模式

通过 `deeplabcut-docker`，您可以用两种模式使用这些镜像。

- *注意 1：首次运行以下任何命令时，可能需要一些时间才能完成（几分钟，取决于您的网速），因为它在后台下载 Docker 镜像。如果终端中没有出现任何错误，请假定一切正常！后续运行该命令会更快。*
- *注意 2：标注 GUI 不能通过 Docker 镜像使用。但是，您可以在 conda 环境中安装 [`napari-deeplabcut`](https://github.com/DeepLabCut/napari-deeplabcut/tree/main?tab=readme-ov-file#napari-deeplabcut-keypoint-annotation-for-pose-estimation) 来进行标注！*
- *注意 3：对于以下任何模式，您可能需要设置哪个目录作为基础目录，以便您可以进行读/写（或只读访问）。方法如下：
如果您想挂载整个目录，可以传递*

`deeplabcut-docker bash -v /home/mackenzie/DEEPLABCUT:/home/mackenzie/DEEPLABCUT`

（这将以读/写模式将完整目录挂载到容器中）

如果只需要只读访问，请使用 `deeplabcut-docker bash -v /home/mackenzie/DEEPLABCUT:/home/mackenzie/DEEPLABCUT:ro`

### 终端模式

您可以通过运行以下命令来运行轻量级的 DeepLabCut 版本并打开终端

``` bash
$ deeplabcut-docker bash
```

**重要提示：** 如果您的机器上有 GPU 并且想用它们来训练模型，您需要在 `deeplabcut-docker` 命令中添加 `--gpus all` 参数：

``` bash
$ deeplabcut-docker bash --gpus all
```

在终端内部，您可以通过运行以下命令并查看安装的 Python 版本来确认 DeepLabCut 是否正确安装：

``` bash
$ ipython
>>> import deeplabcut
```

### Jupyter Notebook 模式

您可以通过启动一个 jupyter notebook 服务器来运行 DeepLabCut。相应的镜像可以通过运行以下命令来拉取和启动：

``` bash
$ deeplabcut-docker notebook 
```

这将启动一个 Jupyter notebook 服务器。请按照终端的指示，在您喜欢的浏览器中输入 `http://127.0.0.1:8888` 来打开 notebook。当提示输入密码时，请使用容器中预设的选项：`deeplabcut`。

此容器中的 DeepLabCut 版本等同于您使用 `pip install deeplabcut[gui]` 安装的版本。这意味着您可以在 notebook 中使用适当的命令启动 DeepLabCut GUI！

### 高级用法

高级用户和开发者可以访问 GitHub 上 DeepLabCut 代码库中的 [`/docker` 子目录](https://github.com/DeepLabCut/DeepLabCut/tree/main/docker)。我们为所有镜像提供了 Dockerfile 以及构建说明。

## 先决条件（如果您尚未安装 Docker）

**(1)** 安装 Docker。请参阅 https://docs.docker.com/install/ & 对于 Ubuntu: https://docs.docker.com/install/linux/docker-ce/ubuntu/
测试 docker: 

    $ sudo docker run hello-world
    
输出应为: ``Hello from Docker! This message shows that your installation appears to be working correctly.``

*如果您遇到错误 ``docker: Error response from daemon: Unknown runtime specified nvidia.``，只需重新启动 docker 即可：
  
       $ sudo systemctl daemon-reload
       $ sudo systemctl restart docker

    
**(2)** 将您的用户添加到 docker 用户组（https://docs.docker.com/install/linux/linux-postinstall/#manage-docker-as-a-non-root-user）
创建 docker 用户组并将您的用户添加进去的快速指南：
创建 docker 用户组。

    $ sudo groupadd docker
将您的用户添加到 docker 用户组。

    $ sudo usermod -aG docker $USER

（最好重启电脑（推荐），或者（至少）打开一个新的终端，以确保从现在开始已将您添加进去）


## 注意事项和故障排除

由于支持 GUI 存在太多问题，我们在 2.3.5+ 版本中移除了对 GUI 的支持。此外，请注意这些测试是基于 Unix 系统进行的。

在某些系统上运行 Linux 容器时，在通过 `deeplabcut-docker` 启动镜像之前，可能需要先运行 `host +local:docker`。

如果您在使用这些镜像时遇到错误，请在 DeepLabCut 仓库中打开一个 issue——特别是 `deeplabcut-docker` 目前仍处于 Alpha 版本，我们非常欢迎用户反馈，以便使该工具在各种操作系统上都能稳定使用！