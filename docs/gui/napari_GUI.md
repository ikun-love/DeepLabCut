```rst
(napari-gui)=
# napari 标注 GUI

从版本 2.3 开始，我们用 PySide6 替换了 wxPython。以下是新 GUI 中使用 napari 相关功能的方法。它可以在 napari-hub 中作为独立 GUI 使用，也可以集成到我们的主 GUI 中，[请参阅此处文档](https://deeplabcut.github.io/DeepLabCut/docs/PROJECT_GUI.html)。

[![License: BSD-3](https://img.shields.io/badge/License-BSD3-blue.svg)](https://www.gnu.org/licenses/bsd3)
[![PyPI](https://img.shields.io/pypi/v/napari-deeplabcut.svg?color=green)](https://pypi.org/project/napari-deeplabcut)
[![Python Version](https://img.shields.io/pypi/pyversions/napari-deeplabcut.svg?color=green)](https://python.org)
[![tests](https://github.com/DeepLabCut/napari-deeplabcut/workflows/tests/badge.svg)](https://github.com/DeepLabCut/napari-deeplabcut/actions)
[![codecov](https://codecov.io/gh/DeepLabCut/napari-deeplabcut/branch/main/graph/badge.svg)](https://codecov.io/gh/DeepLabCut/napari-deeplabcut)
[![napari hub](https://img.shields.io/endpoint?url=https://api.napari-hub.org/shields/napari-deeplabcut)](https://napari-hub.org/plugins/napari-deeplabcut)

一个用于使用 DeepLabCut 进行关键点标注的 napari 插件。


## 安装

您可以通过在 conda 环境中运行以下命令，通过 [pip] 来安装包含完整 DeepLabCut napari 的 GUI：

`pip install 'deeplabcut[tf,gui]'` 或者对于 Mac M1/M2 芯片用户：`pip install 'deeplabcut[apple_mchips,gui]'`

*请注意，这从 v2.3 版本开始可用

如果您已运行上述安装，则不需要此步骤，但您可以通过 [pip] 安装独立的 `napari-deeplabcut`：

`     pip install napari-deeplabcut `


要安装最新的开发版本：

  `  pip install git+https://github.com/DeepLabCut/napari-deeplabcut.git `


(napari-gui-usage)=
## 用法

要使用完整的 GUI，请运行：

`python -m deeplabcut`

要使用独立的 napari 插件，请启动 napari：

`napari `

然后，在 Plugins > napari-deeplabcut: Keypoint controls 中激活该插件。

所有可接受的文件（`config.yaml`、图像、`.h5` 数据文件）都可以通过将它们直接拖放到画布上，或通过“文件”菜单加载。

入门最简单的方法是拖放一个文件夹（通常是 DeepLabCut 的 `labeled-data` 目录中的一个文件夹），如果从头开始标注，请拖放相应的 `config.yaml` 文件，以自动添加一个 *Points layer*（点图层）并填充下拉菜单。

[🎥 演示](https://youtu.be/hsA9IB5r73E)

**工具和快捷键如下：**

- `2` 和 `3`，用于在标注模式和选择模式之间轻松切换
- `4`，用于启用平移和缩放（通过鼠标滚轮或在触摸板上手指滚动实现）
- `M`，用于循环遍历常规（顺序）、快速和循环标注模式（请参阅[此处](https://github.com/DeepLabCut/DeepLabCut-label/blob/ee71b0e15018228c98db3b88769e8a8f4e2c0454/dlclabel/layers.py#L9-L19)的描述）
- `E`，用于启用边缘着色（默认情况下，如果此选项在精修 GUI 模式下使用，置信度低于 0.6 的点将标记为红色）
- `F`，用于在动物和身体部位颜色方案之间切换。
- `V`，用于切换选定图层的可见性。
- `backspace`（退格键）用于删除一个点。
- 勾选“display text”框以在画布上显示标签名称。
- 要移动到另一个文件夹，请务必保存（Ctrl+S），然后删除图层，最后重新拖放下一个文件夹。

![napari_shortcuts](https://images.squarespace-cdn.com/content/v1/57f6d51c9f74566f55ecf271/192345a5-e411-4d56-b718-ef52f91e195e/Qwerty.png?format=1500w)



### 保存图层

使用 `File > Save Selected Layer(s)...`（或其快捷键 `Ctrl+S`）保存标注和分割。
只有在保存分割掩码时，才会弹出对话框让您命名目标文件夹；
关键点标注则会自动保存到相应文件夹中，文件名为 `CollectedData_<ScorerName>.h5`。
- 提醒一下，DLC 只会使用 H5 文件；因此，如果您打开已标注的图像，请确保保存/覆盖 H5 文件。
- 请注意，在保存图层之前，请确保选中了点图层（points layer）。如果用户首先点击了图像图层，然后选择了 `Save As`，接着关闭了窗口，那么该会话中进行的任何标注工作都将丢失！
- 修改然后保存 `machinelabels...` 图层中的点，将会向现有的 `CollectedData` 图层添加数据或覆盖现有数据，但**不会**保存到 `machinelabels` 文件中。

### 视频帧提取和预测精修 (Prediction Refinement)

从 v0.0.4 开始，视频可以在 GUI 中查看。

从 v0.0.5 开始，可以可视化尾随点；例如，这有助于识别交换点或离群的、抖动的预测结果。

加载视频（及其相应的输出 h5 文件）将启用工具栏顶部的视频操作：它们提供手动从 GUI 中提取视频帧，或定义裁剪坐标的选项。
请注意，关键点可以像在标注单个帧时一样进行位移和保存。


## 工作流程 (Workflow)

根据图像文件夹的内容，建议的工作流程如下：

1. **从头开始标注 (Labeling from scratch)** – 图像文件夹不包含 `CollectedData_<ScorerName>.h5` 文件。

    如[用法](#usage)所述打开 *napari*，并与 DeepLabCut 项目的 `config.yaml` 一起打开一个图像文件夹。
    图像文件夹会创建一个 *image layer*（图像图层），其中包含需要标注的图像。
    支持的图像格式有：`jpg`、`jpeg`、`png`。
    `config.yaml` 文件会创建一个 *Points layer*（点图层），其中包含标注所需的元数据（如从配置文件读取的关键点）。
    在图层列表（GUI 左下角窗格）中选择 *Points layer*，然后点击（GUI 左上角）图层控制菜单中的 *+ 号* 图标开始标注。
    当前的关键点可以在关键点下拉菜单（右侧窗格）中查看/选择。
    显示图像下方的滑块（或使用左/右箭头键）可以选择要标注的图像。

    要保存标注进度，请参阅[保存图层](#save-layers)。
    状态栏应显示 `Data successfully saved`（数据已成功保存），并且图像文件夹中现在应该包含一个 `CollectedData_<ScorerName>.h5` 文件。
    （注意：为方便起见，还会同时保存一个同名的 CSV 文件。）

2. **恢复标注 (Resuming labeling)** – 图像文件夹包含 `CollectedData_<ScorerName>.h5` 文件。

    打开 *napari* 并打开一个图像文件夹（该文件夹需要包含 `CollectedData_<ScorerName>.h5` 文件）。
    在这种情况下，没有必要打开 DLC 项目的 `config.yaml` 文件，因为所有必需的元数据都会从 `h5` 数据文件中读取。

    保存操作如 *1* 中所述。

    ***请注意，如果在开始标注后向 `config.yaml` 文件中添加了新的身体部位，则有必要在 GUI 中加载 config 文件，以更新下拉菜单和其他元数据。***

    ***由于 `viridis` 是 `napari-deeplabcut` 的默认颜色图（colormap），在 GUI 中选择颜色图或加载 config 文件可用于更新颜色方案。***

4. **精修标签 (Refining labels)** – 图像文件夹包含 `machinelabels-iter<#>.h5` 文件。

    过程类似于 *2*。
    打开 *napari* 并打开一个图像文件夹。
    如果视频最初被标注过，*并且*已经提取了离群点，它将包含 `CollectedData_<ScorerName>.h5` 文件和一个 `machinelabels-iter<#>.h5` 文件。在这种情况下，在 GUI 中选择 `machinelabels` 图层，然后输入 `e` 显示边缘。红色表示置信度 < 0.6。当您浏览帧时，带有边缘标签的图像需要进行精修（移动、删除等）。没有边缘标签的图像将位于 `CollectedData`（先前的手动标注）图层上，通常不需要精修。但是，您可以切换到该图层并修复错误。您还可以右键单击 `CollectedData` 图层并选择 `toggle visibility` 来隐藏该图层。选择 `machinelabels` 图层后再保存，这将把您的精修标注追加到 `CollectedData` 中。

    如果该文件夹仅提取了离群点而最初没有进行标注，则不会有 `CollectedData` 图层。请选择 `machinelabels` 图层进行精修标注位置的调整，然后再保存。

    在这种情况下，没有必要打开 DLC 项目的 `config.yaml` 文件，因为所有必需的元数据都会从 `h5` 数据文件中读取。

    保存操作如 *1* 中所述。

6. **绘制分割掩码 (Drawing segmentation masks)**

    像 *1* 中那样拖放一个图像文件夹，手动添加一个 *shapes layer*（形状图层）。然后选择左上角控制面板中的 *rectangle*（矩形），
    并在图像上开始绘制矩形。掩码和矩形顶点按[保存图层](#save-layers)中所述进行保存。
    请注意，掩码可以在稍后阶段通过将 `vertices.csv` 文件拖放到画布上来重新加载和编辑。

### 工作流程流程图 (Workflow flowchart)

```{mermaid}
graph TD
  id1[当前处于哪个标注阶段？]
  id2[deeplabcut.label_frames]
  id3[deeplabcut.refine_labels]
  id4[向 \n `CollectedData...` 图层添加标签或修改，并保存该图层]
  id5[修改 `machinelabels` 图层中的标签并保存，\n 这将创建 `CollectedData...` 文件]
  id6[您是否已经精修了最新迭代的某些标签并保存了？]
  id7["所有提取的帧已保存在 `CollectedData...` 中。
1. 隐藏或删除所有 `machinelabels` 图层。
2. 然后在 `CollectedData` 中修改并保存"]
  id8["
1. 隐藏或删除除最新版本外的所有 `machinelabels` 图层。
2. 选择最新的 `machinelabels` 图层并按 `e` 显示边缘。
3. 只修改 `machinelabels` 中的内容，并跳过显示无边缘标签的帧。
4. 保存 `machinelabels` 图层，它会将数据添加到 `CollectedData`。
	- 如果之后需要重新审阅此视频，请忽略 `machinelabels`，仅在 `CollectedData` 中操作"]

  id1 -->|我需要手动标注新帧 \n 或修复我的标签|id2
  id1 ---->|我需要精修已分析视频中的离群帧|id3
  id2 -->id4
  id3 -->|我只有一个 `machinelabels...` 文件|id5
  id3 ---->|我同时拥有 `machinelabels` 和 `CollectedData` 文件|id6
  id6 -->|是|id7
  id6 ---->|否，我只是提取了离群点|id8
```

### 标注多个图像文件夹

多个图像文件夹的标注必须按顺序进行；即，一次只能打开一个图像文件夹。
对特定文件夹的图像标注完成后并且相关的 *Points layer* 已保存后，应通过选择所有图层并点击垃圾桶图标，将*所有*图层从图层列表（GUI 左下角窗格）中移除。
现在，可以根据特定图像文件夹的情况，按照 *1*、*2* 或 *3* 中描述的过程来标注另一个图像文件夹。


### 定义裁剪坐标

在定义裁剪坐标之前，应在 GUI 中加载两个元素：
一个视频和 DeepLabCut 项目的 `config.yaml` 文件（裁剪尺寸将存储到该文件中）。
然后只需添加一个 `Shapes layer`（形状图层），在其中绘制一个具有所需区域的 `rectangle`（矩形），
然后点击 `Store crop coordinates`（存储裁剪坐标）按钮；坐标会自动写入配置文件中。


## 贡献

非常欢迎您的贡献。可以使用 [tox] 运行测试，请确保在提交拉取请求（pull request）之前覆盖率至少保持不变。

要本地安装代码，请 git clone 仓库，然后运行 `pip install -e .`


## 问题 (Issues)

如果您遇到任何问题，请[提交一个问题 (file an issue)] 并附上详细描述。

[file an issue]: https://github.com/DeepLabCut/napari-deeplabcut/issues


## 致谢

这个 napari 插件是使用 [@napari] 的 [cookiecutter-napari-plugin] 模板通过 [Cookiecutter] 生成的。我们感谢 Chan Zuckerberg Initiative (CZI) 对这项工作的资助！

<!--
不要忘记查看设置新包的完整入门指南：
https://github.com/napari/cookiecutter-napari-plugin#getting-started

并查阅 napari 插件开发人员文档：
https://napari.org/plugins/stable/index.html
-->


[napari]: https://github.com/napari/napari
[Cookiecutter]: https://github.com/audreyr/cookiecutter
[@napari]: https://github.com/napari
[cookiecutter-napari-plugin]: https://github.com/napari/cookiecutter-napari-plugin
[BSD-3]: http://opensource.org/licenses/BSD-3-Clause
[tox]: https://tox.readthedocs.io/en/latest/
[pip]: https://pypi.org/project/pip/
[PyPI]: https://pypi.org/
```