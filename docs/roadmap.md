(dev-roadmap)=
## DeepLabCut 开发路线图

📢 ⏳ 🚧

**总体增强 (General Enhancements):**
- [ ] DeepLabCut PyTorch 和模型库 --> DLC 3.0 🔥
- [X] DLC-CookBook v0.1
- [X] DLC 博客，用于发布版本和用户亮点分享
- [X] 新的 Docker 容器集成到主仓库/链接到 Docker Hub 和仓库
- [ ] 3D >2 摄像头支持 --> PyTorch 版本中更好的 3D 功能 🔥

**通用神经网络改进 (General NN Improvements):**
- [X] 添加了 EfficientNet 主干网络 (目前在 ImageNet 上是 SOTA)。https://openaccess.thecvf.com/content/WACV2021/html/Mathis_Pretraining_Boosts_Out-of-Domain_Robustness_for_Pose_Estimation_WACV_2021_paper.html https://github.com/DeepLabCut/DeepLabCut/commit/96da2cacf837a9b84ecdeafb50dfb4a93b402f33
- [X] 新的多融合多尺度网络；DLCRNet\_ms5
- [ ] BUCTD 集成，参见 ICCV 2023 论文：https://arxiv.org/abs/2306.07879

**deeplabcut 2.2: 多动物姿态估计和跟踪**
- [X] Alpha 测试完成 (2020 年 5 月初)
- [X] Beta 版本发布：2.2.b5 于 5 / 22 / 20 发布 :smile:
- [X] Beta 版本发布：2.2b8 于 9 / 2020 发布 :smile:
- [X] Beta 版本 2.2b9 (合并到 2.1.9 --> 候选版本，计划于 2020 年 10 月)
- [X] 2.2rc1
- [X] 2.2rc2
- [X] 2.2rc3
- [X] 论文 Lauer 等人 2021：https://www.biorxiv.org/content/10.1101/2021.04.30.442096v1
- [X] 2.2 稳定版完全发布

**实时模块 (Real-time module)，附带演示如何设置您的相机系统，并与我们的 [相机控制软件]**(https://github.com/AdaptiveMotorControlLab/Camera_Control) **集成**
- [X] 与 Bonsai 集成完成！参见：https://github.com/bonsai-rx/deeplabcut
- [X] 与 Auto-pi-lot 集成。参见：https://auto-pi-lot.com/
- [X] DeepLabCut-live! 于 2020 年 8 月 5 日发布：预印本和代码：https://www.biorxiv.org/content/10.1101/2020.08.04.236422v1
- [X] DeepLabCut-live! 在 eLife 上发表

**DeepLabCut 模型库 (Model Zoo)：一个用于即插即用 DLC 和社区众包的预训练模型集合。**
- [X] BETa 版本随 2.1.8b0 发布：https://www.mackenziemathislab.org/deeplabcut
- [X] 随 2.1.8.1 完全发布 https://www.mackenziemathislab.org/deeplabcut
- [X] 论文即将发布！--> 查看 arXiv：https://arxiv.org/abs/2203.07436
- [X] 添加了新模型；马、猎豹
- [X] TopView\_Mouse 模型
- [X] 四足动物模型 (Quadruped model)
- [ ] 贡献模块
- [ ] PyTorch 模型库代码

**DeepLabCut GUI 和 DeepLabCut-core:**
- [X] 为了使 DLC 模块化，我们将核心功能移至 https://github.com/DeepLabCut/DeepLabCut-core
- [X] DLC-core 弃用，核心现在通过 `pip install deeplabcut` 安装，GUI 通过 `pip install deeplabcut[gui]` 安装
- [X] DeepLabCut 新的 GUI；由于 wxPython 持续存在问题，我们将迁移到一个 napari 插件 https://github.com/napari/napari
- [X] 新的项目管理 GUI
- [X] DeepLabCut-core 中支持 tensorflow 2.2：https://github.com/DeepLabCut/DeepLabCut/issues/601
- [X] DeepLabCut-Core 将被弃用；TF2 将进入主仓库。
- [X] 在支持 TF1 的同时支持 TF2，直到 2022 年。
- [ ] 基于 Web 的标注 GUI --> 用户的 Colab 训练管道 (完全无需安装 DLC)