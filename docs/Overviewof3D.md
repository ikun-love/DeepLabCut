```rst
(3D-overview)=
# 3D DeepLabCut

在此仓库中，我们直接支持双摄像机设置的 3D 姿态估计。如果您需要支持 $n$ 个摄像机以及更优美的优化方法，请参阅我们发表在 [ICRA 2021 关于强大基线 3D 模型（及 3D 数据集）](https://github.com/African-Robotics-Unit/AcinoSet) 上的工作。在链接中，您将找到我们如何针对猎豹优化 6 个以上摄像头的 DLC 输出数据（并在下方查看更多内容）。

<img src="https://images.squarespace-cdn.com/content/v1/57f6d51c9f74566f55ecf271/1589578632599-HQENUYUIBI9KYTZA2WXV/ke17ZwdGBToddI8pDm48kBgERiRoVg6XJpnbAnG076FZw-zPPgdn4jUwVcJE1ZvWhcwhEtWJXoshNdA9f1qD7Y5_KuY_fkOEvGrDVB8aRb13EC_7Ld97nVeJG4MMJk1tqSdWG3KOMGCA68a4XjyT5g/3D.png?format=300w" width="350" title="DLC-3D" alt="DLC 3D" align="right" vspace = "50">


## **注意：此仓库中的代码库假设您已完成以下准备：**

A. 您拥有 2D 视频，并且有一个用于分析这些视频的 DeepLabCut 网络，如[主文档](overview)中所述。这可以通过为每个摄像头设置多个独立的网络（不推荐），或训练一个在所有视图上都有效的网络（推荐！）（参阅 [Nath*, Mathis* 等, 2019](https://www.biorxiv.org/content/10.1101/476531v1)）。我们还支持使用此代码进行多动物 3D 姿态估计（请参阅 [Lauer 等, 2022](https://doi.org/10.1038/s41592-022-01443-0)）。

B. 您正在使用 2 个摄像头，并处于 [立体配置](https://github.com/DeepLabCut/DeepLabCut/blob/5ac4c8cb6bcf2314a3abfcf979b8dd170608e094/deeplabcut/pose_estimation_3d/camera_calibration.py#L223) 进行 3D 估计*。

C. 您已经拍摄了校准图像（详情见下文！）。


### ***如果您需要支持超过 2 个摄像头：**
以下是您可以使用的其他出色选项，它们扩展了 DeepLabCut 的功能：

<img src="https://images.squarespace-cdn.com/content/v1/57f6d51c9f74566f55ecf271/1628432165795-BBF6AWCK1BEKV3AJ6GF5/cheetah.gif?format=1500w" width="350" title="AcinoSet-3D" alt="DLC 3D" align="right" vspace = "50">

- **[AcinoSet](https://github.com/African-Robotics-Unit/AcinoSet)**；支持 **$n$** 个摄像头的三角剖分、扩展卡尔曼滤波和轨迹优化代码（请参阅右侧视频了解最小化演示，由 Patel 教授提供），以及一个用于可视化 3D 数据的 GUI。它旨在直接与 DeepLabCut 配合使用（但目前针对猎豹进行了定制，因此目前需要一些编码技能）。


- **[anipose.org](https://anipose.readthedocs.io/en/latest/)**；一个 3D deeplabcut 的包装器，提供 >3 摄像头支持，旨在直接与 DeepLabCut 配合使用。您可以将 `pip install anipose` 安装到您的 DLC conda 环境中。

- **Argus, easywand 或 DLTdv** 参见 https://github.com/backyardbiomech/DLCconverterDLT；这可以与非常流行的 Argus 或 DLTdv 工具一起用于棋盘格校准。截至 2025 年夏季，[Argus](https://github.com/kilmoretrout/argus_gui) 现在通过新的[工作流程文档](https://github.com/kilmoretrout/argus_gui/blob/master/docs/deeplabcut.md)支持直接导入和导出 DeepLabCut 输出文件到 GUI 中。

## 使用直接的 DeepLabCut 双摄像头支持快速入门：

- 支持单动物 DeepLabCut 和多动物 DeepLabCut (maDLC) 项目：

<p align="center">
<img src= "https://images.squarespace-cdn.com/content/v1/57f6d51c9f74566f55ecf271/1560968522350-COKR986AQESF5N1N7QNK/ke17ZwdGBToddI8pDm48kNaO57GzHjWqV-xM6jVvY6ZZw-zPPgdn4jUwVcJE1ZvWQUxwkmyExglNqGp0IvTJZUJFbgE-7XRK3dMEBRBhUpyR5k0u27ivMv3az5DOhUvLuYQefjfUWYPEDVexVC_mSas4X78tjQKn3yE00zHvnK8/3D_maousLarger.gif?format=750w" height="200">

<img src="https://images.squarespace-cdn.com/content/v1/57f6d51c9f74566f55ecf271/cd423302-0389-4b63-8869-b787a2c52b8b/maDLC_3d.gif?format=1500w" height="200">
</p>

### (1) 创建一个新的 3D 项目：

观看 [演示视频](https://youtu.be/Eh6oIGE4dwI) 了解如何使用此代码，并查看 [此处](https://github.com/DeepLabCut/DeepLabCut/blob/main/examples/JUPYTER/Demo_3D_DeepLabCut.ipynb) 的 Notebook！


您需要为每个项目运行此函数 **一次**；一个项目定义为给定的一组摄像头和校准图像。您始终可以在此项目中分析新视频。

函数 **create\_new\_project\_3d** 会创建一个新的项目目录，专门用于将 2D 姿态转换为 3D 姿态，创建所需的子目录以及基本的 3D 项目配置文件。每个项目由项目的名称（例如 Task1）、实验人员的名称（例如 YourName）以及创建日期来标识。

因此，此函数要求用户输入项目名称、实验人员名称和要使用的摄像头数量。目前，DeepLabCut 支持使用 2 个摄像头进行三角剖分，但在未来的版本中将扩展到超过 2 个摄像头。

要启动 3D 项目，请在 ipython 中输入以下内容：
```python
deeplabcut.create_new_project_3d("ProjectName", "NameofLabeler", num_cameras=2)
```
提示 1：如果您希望将此文件夹放置在当前工作目录之外的其他位置，您还可以传递 `working_directory="Working directory 的完整路径"`。如果未指定可选参数 `working_directory`，则项目目录将在当前工作目录中创建。

提示 2：您还可以将 `config_path3d` 放在 `deeplabcut.create_new_project_3d` 的前面，以创建一个保存到 config.yaml 文件路径的变量，即 `config_path3d=deeplabcut.create_new_project_3d(...`。或者，您可以设置此变量以方便使用。请注意，`config_path3d='3D 项目配置文件 的完整路径'`。

此函数将在 **工作目录** 中创建一个项目目录，名称为 **项目名称+实验人员名称+项目创建日期+3d**。该项目目录将包含子目录：**calibration\_images**、**camera\_matrix**、**corners** 和 **undistortion**。在项目过程中生成的所有输出都将存储在这些子目录之一中，从而允许每个项目与其他项目分开管理。

子目录的用途如下：

**calibration\_images:** 此目录将包含从两个摄像头获取的一组校准图像。使用打印的棋盘格采集校准图像，并从两个摄像头获取其配对图像作为一组校准图像。

**camera\_matrix:** 此目录将把两个摄像头的参数存储在一个 pickle 文件中。具体来说，这些 pickle 文件包含相机的内参和外参。内参代表从 3D 摄像头坐标到图像坐标的转换，而外参代表从世界坐标系到 3D 摄像头坐标系的刚性变换。

**corners:** 作为相机校准的一部分，将在校准图像中检测到棋盘格图案，并将这些图案存储在此目录中。棋盘格网格的每一行都用唯一的颜色标记。

**undistortion:** 为了检查校准情况，会对校准图像和相应的角点进行去畸变处理。这些去畸变后的图像将与去畸变后的点叠加，并存储在此目录中。

以下是后续的校准和三角剖分工作流程概述：

<p align="center">
<img src="https://images.squarespace-cdn.com/content/v1/57f6d51c9f74566f55ecf271/1559751031211-IOTHQDAEEFP939AD8L8Q/ke17ZwdGBToddI8pDm48kCpBvlJgRextwO-RLKSiThBZw-zPPgdn4jUwVcJE1ZvWQUxwkmyExglNqGp0IvTJZamWLI2zvYWH8K3-s_4yszcp2ryTI0HqTOaaUohrI8PIoI8wFyxyzDq4NO_A5fg6hgZUWi6FxVv9SjR8GkGxb-wKMshLAGzx4R3EDFOm1kBS/3dworkflow.png?format=1000w" width="55%">
</p>

### (2) 拍摄和处理相机校准图像：

（**关键！**）您必须拍摄棋盘格图像来校准您的图像。以下是您可以打印和使用的示例棋盘格（请将其安装在平坦、坚硬的表面上！）：
https://markhedleyjones.com/projects/calibration-checkerboard-collection。
- 必须将图像对保存为 .jpg 文件。
- 它们的命名必须以 **camera-\#** 作为前缀，例如，第一对图像的名称为 **camera-1-01.jpg** 和 **camera-2-01.jpg**。请注意，项目创建后此名称不能更改。

**提示：** 如果您想在移动棋盘格的同时拍摄短视频（而不是捕捉帧对），您可以在 conda 环境中（但在 ipython 外部！）使用此命令将视频转换为 **.jpg** 帧（这将捕获前 20 帧（由 `-vframes` 设置）并命名为 camera-1-001.jpg 等；请相应地编辑）：

```bash
ffmpeg -i videoname.mp4 -vframes 20 camera-1-%03d.jpg
```
- 在拍摄图像时：
  - 保持棋盘格的方向不变，旋转角度不要超过 30 度。绕圆周旋转棋盘格可能会改变跨帧的原点，并可能导致检测到的角点顺序不正确。

  - 覆盖多个深度距离，并在每个距离内覆盖图像视图的所有部分（所有角点和中心）。

  - 使用尽可能大的棋盘格，理想情况下至少有 8x6 个方格。

  - 目标是拍摄至少 30-70 对图像，因为在角点检测后，可能会因为角点检测不正确或检测到的角点顺序不正确而需要丢弃一些图像。

  - 您可以拍摄一系列 .jpg 图像，或者拍摄一个视频，然后事后同步配对的帧（参见上面的提示）。


相机校准是一个**迭代过程**，用户需要选择一组棋盘格图案被正确检测到的校准图像。函数 `deeplabcut.calibrate_cameras(config_path)` 从校准图像中提取网格图案，并将其存储在 `corners` 目录下。网格图案可以是 8x8 或 5x5 等。我们使用 8x6 的网格图案来查找棋盘格的内部角点。

在某些情况下，角点可能无法正确检测到，或者在 camera-1 图像和 camera-2 图像中检测到的角点顺序不正确。您需要从 **calibration\_images** 文件夹中删除这些图像对，因为它们会降低校准精度。

首先，请将您的图像放入 **calibration\_images** 目录中。

（**关键！**）编辑 **config.yaml** 文件以设置摄像头名称；请注意，设置后**不要更改名称！**

然后，运行：

```python
deeplabcut.calibrate_cameras(config_path3d, cbrow=8, cbcol=6, calibrate=False, alpha=0.9)
```

注意：您需要指定棋盘格有多少行（`cbrow`）和列（`cbcol`）（请注意，我们计算的是方格之间的边数，而不是方格本身，因此对于 8x8 方格的棋盘格，请设置 `cbrow=7` 和 `cbcol=7`）。此外，首先将变量 `calibrate` 设置为 **False**，以便您可以删除任何有问题的图像。您需要目视检查输出来检查检测到的角点，并选择角点检测正确的图像对。请注意，如果缩放参数 `alpha=0`，它会返回去畸变后的图像，同时去除不必要的像素最少。因此，它甚至可能会去除图像角点处的一些像素。如果 `alpha=1`，则保留所有像素，但会产生一些额外的黑色图像。

它们可能看起来像这样：

<p align="center">
<img src="https://images.squarespace-cdn.com/content/v1/57f6d51c9f74566f55ecf271/1559776966423-RATM6ZQT8JXHYAN768F6/ke17ZwdGBToddI8pDm48kKmw982fUOZVIQXHUCR1F55Zw-zPPgdn4jUwVcJE1ZvWQUxwkmyExglNqGp0IvTJZUJFbgE-7XRK3dMEBRBhUpx7krGdD6VO1HGZR3BdeCbrijc_yIxzfnirMo-szZRSL5-VIQGAVcQr6HuuQP1evvE/right02_corner.jpg?format=500w" height="220">
 <img src="https://images.squarespace-cdn.com/content/v1/57f6d51c9f74566f55ecf271/1559776952829-KRHFX74CDO3BPIY9E9U0/ke17ZwdGBToddI8pDm48kKmw982fUOZVIQXHUCR1F55Zw-zPPgdn4jUwVcJE1ZvWQUxwkmyExglNqGp0IvTJZUJFbgE-7XRK3dMEBRBhUpx7krGdD6VO1HGZR3BdeCbrijc_yIxzfnirMo-szZRSL5-VIQGAVcQr6HuuQP1evvE/left02_corner.jpg?format=500w" height="220">
</p>


确认所有图像对（即删除文件夹中所有错误的配对！）中的角点及其顺序都已正确检测到后，就可以使用以下命令对两个摄像头进行标定：

```python
deeplabcut.calibrate_cameras(config_path3d, cbrow=8, cbcol=6, calibrate=True, alpha=0.9)
```

这将计算每个摄像头的内参和外参。还会使用内参和外参计算重投影误差，以估计参数的优劣程度。估计两个摄像头之间的变换，并进行立体校准。此外，上述函数通过计算立体校正（stereo rectification）将两个摄像头的图像平面带到同一平面上。这些参数将存储在 `camera_matrix` 目录下一个名为 `stereo_params.pickle` 的 pickle 文件中。

成功为项目运行此步骤后，您就不需要再运行它了（除非您想重新校准摄像头）；请注意，如果您重新校准，您可能需要清楚地标记哪些视频是使用“旧”校准图像分析的，哪些是使用“新”校准图像分析的。

### (3) 检查去畸变：

为了检查立体校准的效果如何，建议使用相机矩阵对校准图像和角点进行去畸变处理，并将这些去畸变后的点投影到去畸变后的图像上，以检查它们是否正确对齐。这可以在 deeplabcut 中通过以下方式完成：

```python
deeplabcut.check_undistortion(config_path3d, cbrow=8, cbcol=6)
```

每个校准图像都会在 `undistortion` 目录下被去畸变并保存。还会存储一个图，其中包含一对去畸变后的相机图像以及叠加其上的去畸变后的角点。请目视检查此图像。所有校准图像中的去畸变后的角点都将被三角剖分并绘制出来，供用户可视化任何与去畸变相关的错误。如果它们不正确，请检查并修改校准图像（然后重复校准和此步骤）！

### (4) 三角剖分 --> 将 2D 转换为 3D！

如果没有去畸变错误，则可以对来自两个摄像头的姿态进行三角剖分，以获得 3D DeepLabCut 坐标！

（**关键！**）请以使文件名**包含 `config` 文件中指定的摄像头名称**的方式来命名视频文件。例如，如果摄像头命名为 `camera-1` 和 `camera-2`（或 `cam-1`, `cam-2` 等），则视频文件名必须包含此命名，例如，它可以命名为 `rig-1-mouse-day1-camera-1.avi` 和 `rig-1-mouse-day1-camera-2.avi`，或者可以是 `rig-1-mouse-day1-camera-1-date.avi` 和 `rig-1-mouse-day1-camera-2-date.avi`。

- **注意**，为了正确配对视频，文件名中其他部分需要相同！
- 如果有帮助，[这是我们用于录制视频的软件](https://github.com/AdaptiveMotorControlLab/Camera_Control)。

（**关键！**）您还必须编辑 **3D 项目 config.yaml** 文件，以指明哪些 DeepLabCut 项目包含 2D 视图的信息。

- 至关重要的是，您需要输入与 2D 项目的 config.yaml 文件中**相同**的身体部位名称。
- 您需要设置要使用的快照（默认为 -1，即网络的最后一个训练快照）。
- 您需要设置一个“scorer 3D”名称；这将指向项目文件，并设置在未来的 3D 输出文件名中。
- 您也应该在这里定义一个“骨架”（skeleton）（注意：这不是固定的，它只在绘图步骤中连接点）。并非所有点都需要“骨架化”，即这些点可以是完整身体部位列表的一个子集。其他点将仅被绘制到 3D 空间中。以下是带有示例输入的 config.yaml 文件的样子：

 <p align="center">
<img src="https://images.squarespace-cdn.com/content/v1/57f6d51c9f74566f55ecf271/1559756808766-2G6FG91S2I4ZX2SSP6QF/ke17ZwdGBToddI8pDm48kEULogWWASOhGi36VEr2SOlZw-zPPgdn4jUwVcJE1ZvWQUxwkmyExglNqGp0IvTJZamWLI2zvYWH8K3-s_4yszcp2ryTI0HqTOaaUohrI8PIoI8wFyxyzDq4NO_A5fg6hgZUWi6FxVv9SjR8GkGxb-wKMshLAGzx4R3EDFOm1kBS/config3d.jpg?format=1000w" width="95%">
</p>

（**关键！**）此步骤还会为您运行 2D 中的 `analyze_videos` 等效操作，然后对 2D 数据应用中值滤波器（`filterpredictions=True`）！如果您已经运行了 2D 分析并且存在过滤后的输出文件，它将默认使用该文件（否则它将使用您的未过滤的 2D 分析文件）！

接下来，传入 `config_path3d` 以及视频文件夹路径，即存储来自两个摄像头的**所有**视频的**文件夹**的路径。可以通过输入以下内容在 deeplabcut 中进行三角剖分：

```python
deeplabcut.triangulate(
  config_path3d,
  "/yourcomputer/fullpath/videofolder",
  filterpredictions=True/False
)
```
注意：Windows 用户，路径必须输入为：``r`C:\Users\computername\videofolder` ``或 ``C:\\Users\\computername\\videofolder` ``。

**提示：** 以下是您可以传递的所有参数：

```python
Parameters
----------
config : string
    config.yaml 文件的完整路径（字符串形式）。

video_path : string
    保存视频的目录的完整路径。

videotype: string, optional
    在输入视频是目录的情况下，检查视频的扩展名。
    仅分析具有此扩展名的视频。默认为 ``.avi``

filterpredictions: Bool, optional
    通过拟合中值（默认）或 arima 滤波器来过滤预测结果。如果指定，则必须为 ``True`` 或 ``False``。

filtertype: string
    选择哪个滤波器，'arima' 或 'median' 滤波器。

gputouse: int, optional. 自然数，表示您的 GPU 编号（参见 nvidia-smi 中的编号）。如果您没有 GPU，请设置为 None。
    参见：https://nvidia.custhelp.com/app/answers/detail/a_id/3751/~/useful-nvidia-smi-queries

destfolder: string, optional
    指定分析数据的目标文件夹（默认为视频的路径）

save_as_csv: bool, optional
    将预测结果保存到 .csv 文件中。默认为 ``False``；如果提供，则必须为 ``True`` 或 ``False``

track_method: str, optional
    用于跟踪的方法："box" 或 "ellipse"
```
**三角剖分后的文件**现在保存在视频文件所在的目录（或您设置的目标文件夹）下！这可用于未来的分析。此步骤可以随时运行，以收集新视频，并轻松添加到您的自动化分析流程中，例如**替换**
`deeplabcut.triangulate(config_path3d, video_path)` 为 `deeplabcut.analyze_videos`（如果尚未在 2D 中分析，此函数将处理它；）：

<p align="center">
<img src= https://images.squarespace-cdn.com/content/v1/57f6d51c9f74566f55ecf271/1559758477126-B9PU1EFA7L7L1I24Z2EH/ke17ZwdGBToddI8pDm48kH6mtUjqMdETiS6k4kEkCoR7gQa3H78H3Y0txjaiv_0fDoOvxcdMmMKkDsyUqMSsMWxHk725yiiHCCLfrh8O1z5QPOohDIaIeljMHgDF5CVlOqpeNLcJ80NK65_fV7S1UQf4d-kVja3vCG3Q_2S8RPAcZTZ9JxgjXkf3-Un9aT84H3bqxw7fF48mhrq5Ulr0Hg/howtouseDLC2d_3d-01.png?format=1000w width="65%">
 </p>

### (5) 可视化您的 3D DeepLabCut 视频：

为了可视化带有跟踪点的 2D 视频以及 3D 中的姿态，用户可以为某些帧创建 3D 视频（这些文件很大，因此我们建议只查看部分帧）。用户可以指定配置文件、**三角剖分文件所在的路径**，并指定起始和结束帧索引来创建 3D 标记视频。请注意，`triangulated_file_folder` 是以 `yourDLC_3D_scorername.h5` 结尾的新创建的文件所在的目录。这可以通过以下方式完成：

```python
deeplabcut.create_labeled_video_3d(
  config_path,
  ["triangulated_file_folder"],
  start=50,
  end=250
)
```

**提示：**（请参阅下文了解更多参数）您可以通过更改变量 `xlim`、`ylim`、`zlim` 和 `view` 来设置最右侧 3D 图上坐标轴的外观。上面创建的 `checkerboard_3d.png` 图像将显示坐标轴范围。示例如下：

 <p align="center">
<img src="https://images.squarespace-cdn.com/content/v1/57f6d51c9f74566f55ecf271/1559864026106-B6XQHUDUA8VB6F0FNVBA/ke17ZwdGBToddI8pDm48kKmw982fUOZVIQXHUCR1F55Zw-zPPgdn4jUwVcJE1ZvWQUxwkmyExglNqGp0IvTJZUJFbgE-7XRK3dMEBRBhUpx7krGdD6VO1HGZR3BdeCbrijc_yIxzfnirMo-szZRSL5-VIQGAVcQr6HuuQP1evvE/checkerboard_3d.png?format=750w" width="45%">
</p>

`View` 用于设置 Z 平面的仰角和 X,Y 平面的方位角（默认为 [113, 270]，您应该多尝试几次以找到您喜欢的视角！）。此外，请注意，视频是由“temp”目录中的一组 .png 文件创建的，因此一旦运行此命令，您可以打开第一个图像，如果您不喜欢该视角，请按 `CNTRL+C` 停止，编辑值，然后重新开始！

**其他可选参数包括：**
在此处查看：
```python
videofolder: string
    存储视频的文件夹的完整路径。如果视频存储在不同于三角测量文件存储的位置，请使用此参数。默认为 ``None``，因此它会在三角测量文件所在的目录中查找视频文件。

trailpoints: int
    在当前帧中绘制身体部位时考虑的先前帧数（用于显示历史记录）。默认为 0。

videotype: string
    在输入是目录的情况下，检查视频的扩展名。
    仅分析具有此扩展名的视频。默认为 ``.avi``

view: list
    一个列表，用于设置 3D 视图在 Z 平面上的仰角和在 X,Y 平面上的方位角。如果您想旋转 3D 视图的坐标轴，这非常有用。

xlim: list
    一个整数列表，指定 3D 视图 X 轴的限制。默认为 [None,None]，其中 X 限制是通过获取所有身体部位的 X 坐标的最小值和最大值来设置的。

ylim: list
    一个整数列表，指定 3D 视图 Y 轴的限制。默认为 [None,None]，其中 Y 限制是通过获取所有身体部位的 Y 坐标的最小值和最大值来设置的。

zlim: list
    一个整数列表，指定 3D 视图 Z 轴的限制。默认为 [None,None]，其中 Z 限制是通过获取所有身体部位的 Z 坐标的最小值和最大值来设置的。

draw_skeleton: bool
    如果为 True，则在每帧上添加一条线连接身体部位，形成骨架。要连接的身体部位和连接线的颜色在配置文件中指定。默认：True

color_by : string, optional (default='bodypart')
    着色规则。默认情况下，每个身体部位的颜色都不同。
    如果设置为 'individual'，则属于单个个体的点将被着以相同的颜色。

figsize: tuple[int, int], optional, default=(80, 8)
    图形大小

fps: int, optional, default=30
    每秒帧数

dpi: int, optional, default=300
    每英寸点数（分辨率）
```

### 如果您使用此代码：

我们恳请您引用 [Mathis 等, 2018](https://www.nature.com/articles/s41593-018-0209-y) **和** [Nath*, Mathis* 等, 2019](https://doi.org/10.1038/s41596-019-0176-0)。如果您使用 3D
多动物跟踪：[Lauer 等, 2022](https://doi.org/10.1038/s41592-022-01443-0)。
```