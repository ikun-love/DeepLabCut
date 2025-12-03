(project-manager-gui)=
# 交互式项目管理器 GUI

由于有些用户可能更习惯使用交互式界面进行操作，我们希望为该软件提供一个便捷的切入点。所有主要功能都包含在一个易于部署的 GUI（图形用户界面）中。因此，虽然这个项目 GUI 中尚未完全涵盖所有的高级功能，但我们希望这能帮助更多用户快速上手并运行起来。

**发布说明：** 从 DeepLabCut 2.1 及以上版本开始，我们为 DeepLabCut 提供了一个完整的前端用户体验。从 2.3 及以上版本开始，我们已将 GUI 从 wxPython 迁移到 PySide6，并增加了对 napari 的支持。

## 开始使用：

(1) 使用 Anaconda 的简单安装方法（[见此处](how-to-install)*）安装 DeepLabCut。
现在您已经安装了 DeepLabCut，但如果您想更新它，可以遵循以下两种方式之一：等待 GUI 中出现提示（当新版本可用时会提示您升级），或者直接进入您的环境（激活 `DEEPLABCUT`），然后运行：

` pip install 'deeplabcut[gui,modelzoo]'` *但请参阅[完整的安装指南](how-to-install)！


(2) 打开终端并运行：`python -m deeplabcut`


<p align="center">
<img src="https://images.squarespace-cdn.com/content/v1/57f6d51c9f74566f55ecf271/07ae2633-dc3e-4b6d-beec-27c08d9f8531/ezgif.com-gif-maker+%284%29.gif?format=2500w" width="80%">
</p>

请从“项目管理 (Project Management)” 选项卡开始，然后依次操作其他选项卡，以构建您的定制模型并将其应用于新数据。
我们建议保持终端窗口（以及 GUI）可见，这样您在逐步完成项目流程时，可以看到正在进行的进程或者可能出现的任何错误。

- 有关特定的 napari 标签（labeling）功能，请参阅我们的 [“napari gui” 文档](napari-gui-usage)。
- 要将界面从深色模式切换到浅色模式，请在顶部设置外观：
<p align="center">
<img src="https://images.squarespace-cdn.com/content/v1/57f6d51c9f74566f55ecf271/5e41b01d-3101-40b2-9c53-129d8988370f/Screen+Shot+2022-10-09+at+3.45.46+PM.png?format=2500w
" width="30%">
</p>

## 视频演示：如何启动和运行项目管理器 GUI：

**点击图片！**

请注意，当前的视频演示使用的是 wxPython 版本，但操作逻辑是相同的！

[![Watch the video](https://images.squarespace-cdn.com/content/v1/57f6d51c9f74566f55ecf271/1572824438905-QY9XQKZ8LAJZG6BLPWOQ/ke17ZwdGBToddI8pDm48kIIa76w436aRzIF_cdFnEbEUqsxRUqqbr1mOJYKfIPR7LoDQ9mXPOjoJoqy81S2I8N_N4V1vUb5AoIIIbLZhVYxCRW4BPu10St3TBAUQYVKcLthF_aOEGVRewCT7qiippiAuU5PSJ9SSYal26FEts0MmqyMIhpMOn8vJAUvOV4MI/guilaunch.jpg?format=1000w)](https://youtu.be/KcXogR-p5Ak)

### 使用最新 DLC 代码的项目管理器 GUI（单个动物，加上物体）：⬇️

[![Watch the video](https://images.squarespace-cdn.com/content/v1/57f6d51c9f74566f55ecf271/1589046800303-OV1CCNZINWDMF1PZWCWE/ke17ZwdGBToddI8pDm48kB4PVlRPKDmSlQNbUD3wvXgUqsxRUqqbr1mOJYKfIPR7LoDQ9mXPOjoJoqy81S2I8N_N4V1vUb5AoIIIbLZhVYxCRW4BPu10St3TBAUQYVKcaja1QZ1SznGf7WzFOi-J6zLusnaF2VdeZcKivwxvFiDfGDqVYuwbAlftad9hfoui/dlc_gui_22.png?format=1000w)](https://www.youtube.com/watch?v=JDsa8R5J0nQ)

[在此处阅读更多信息](important-info-regd-usage)

### 使用最新 DLC 代码的项目管理器 GUI（多个外观相同的动物，加上物体）：

[![Watch the video](https://images.squarespace-cdn.com/content/v1/57f6d51c9f74566f55ecf271/1589047147498-G1KTFA5BXR4PVHOOR7OG/ke17ZwdGBToddI8pDm48kJDij24pM2COisBTLIGjR1pZw-zPPgdn4jUwVcJE1ZvWQUxwkmyExglNqGp0IvTJZamWLI2zvYWH8K3-s_4yszcp2ryTI0HqTOaaUohrI8PIel60EThn7SDFlTiSprUhmjQQHn9bhdY9dnQSKs8bCCo/Untitled.png?format=1000w)](https://www.youtube.com/watch?v=Kp-stcTm77g)

[在此处阅读更多信息](important-info-regd-usage)

## 视频演示：如何使用新的网络和数据增强管道对数据进行基准测试：

[观看视频](https://youtu.be/WXCVr6xAcCA)