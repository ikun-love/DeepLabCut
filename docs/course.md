```rst
# DeepLabCut 自步调课程

::::{warning}
本课程是为 DLC 2 设计的。
DLC 3 的更新版本正在开发中。
::::

您有动物行为的视频吗？ 第 1 步：获取姿态...

 <img src="https://images.squarespace-cdn.com/content/v1/57f6d51c9f74566f55ecf271/1572296495650-Y4ZTJ2XP2Z9XF1AD74VW/ke17ZwdGBToddI8pDm48kMulEJPOrz9Y8HeI7oJuXxR7gQa3H78H3Y0txjaiv_0fDoOvxcdMmMKkDsyUqMSsMWxHk725yiiHCCLfrh8O1z5QPOohDIaIeljMHgDF5CVlOqpeNLcJ80NK65_fV7S1UZiU3J6AN9rgO1lHw9nGbkYQrCLTag1XBHRgOrY8YAdXW07ycm2Trb21kYhaLJjddA/DLC_logo_blk-01.png?format=1000w" width="150" title="DLC-live" alt="DLC LIVE!" align="right" vspace = "50">

本文档是为希望学习使用 ``Python`` 和 ``DeepLabCut`` 的人员提供的课程资源大纲。
我们预计如果您严格按照课程内容学习，大约需要 1-2 周才能完成。 如果只是掌握基础知识，大概需要 1-2 天。

[点击此处启动交互式图形以开始学习！](https://view.genial.ly/5fb40a49f8a0ef13943d4e5e/horizontal-infographic-review-learning-to-use-deeplabcut)（下方有迷你预览）或者，请从下面开始！

<p align="center">
<img src="https://images.squarespace-cdn.com/content/v1/57f6d51c9f74566f55ecf271/1605642639913-OIUBVR8R0JLYZQPRIYIR/ke17ZwdGBToddI8pDm48kMMAxEenKbh651VJujierMxZw-zPPgdn4jUwVcJE1ZvWQUxwkmyExglNqGp0IvTJZUJFbgE-7XRK3dMEBRBhUpw-CO5bsXt3Lwn3O5kv-PfTgGtLU9oye8D4J7Fixq38Gl-o9tfrEtbnqpPzC5bXTas/ezgif.com-gif-maker.gif?format=750w" width="95%">
</p>


## 安装：

您需要安装 Python 和 DeepLabCut！
- [请参阅这些“初学者文档”以获取帮助！](beginners-guide)

- **观看：** 关于 conda 的概述：[Python 教程：Anaconda - 安装和使用 Conda](https://www.youtube.com/watch?v=YJC6ldI3hWk)


## 课程大纲：

### **Python、终端计算基础知识及 DeepLabCut 概述：**

- **学习：** 使用您计算机上的终端/cmd：[视频教程！](https://www.youtube.com/watch?v=5XgBd6rjuDQ)

- **学习：** 虽然只需要最少甚至不需要 Python 编码（即，您可以使用 DLC GUI 在不编写代码的情况下运行整个程序），但这里有一些您可能想查看的资源。[Software Carpentry: 使用 Python 编程](https://swcarpentry.github.io/python-novice-inflammation/)

- **学习：** 学习并了解信号处理，以及 Demba Ba 教授在 [JupyterCon 上的演讲](https://www.youtube.com/watch?v=ywz-LLYwkQQ) 所做的概述

- **演示：** 我可以快速演示 DEEPLABCUT (DLC) 吗？
    - 是的：[您可以点击浏览此演示 notebook](https://github.com/DeepLabCut/DeepLabCut/blob/main/examples/COLAB/COLAB_DEMO_mouse_openfield.ipynb)
    - 并跟随我的操作：[视频教程！](https://www.youtube.com/watch?v=DRT-Cq2vdWs)


- **观看：** 如何知道 DLC 是否已正确安装？（即如何使用我们的测试脚本！）[视频教程！](https://youtu.be/IOWtKn3l33s)


<img src="https://images.squarespace-cdn.com/content/v1/57f6d51c9f74566f55ecf271/1587608364285-A8R2F24K4DCP0KLAYI91/ke17ZwdGBToddI8pDm48kOhrDvKq54Xu9oStUCFZX0R7gQa3H78H3Y0txjaiv_0fDoOvxcdMmMKkDsyUqMSsMWxHk725yiiHCCLfrh8O1z4YTzHvnKhyp6Da-NYroOW3ZGjoBKy3azqku80C789l0p4XabXLlNWpcJMv7FrN_NLe3GEN018us8vX03EdtIDHsW7dEh7nvL5CemxAxOy1gg/EKlIEXyXUAE0cy3.jpeg?format=1000w" width="350" title="DLC" alt="回顾！" align="right" vspace = "50">

- **阅读论文：** 深度学习在动物姿态估计中的现状，即“用于神经科学中动物行为测量的深度学习工具” [arXiv](https://arxiv.org/abs/1909.13868) 和 [发表版本](https://www.sciencedirect.com/science/article/pii/S0959438819301151)

- **阅读论文：** [深度学习运动捕捉入门指南：原理、陷阱与观点](https://www.sciencedirect.com/science/article/pii/S0896627320307170)


- **观看：** 文档很多……从哪里开始：[视频教程！](https://www.youtube.com/watch?v=A9qZidI7tL8)

### **模块 1：数据入门**

**您需要什么：** 任何可以看到动物/物体的视频等。
您可以使用我们的演示视频、从互联网上获取一些视频，或者使用您拥有的任何旧数据。 任何相机、彩色/单色等都可以。 找到多样化的视频，并好好标记您想要跟踪的内容 :)
- 如果您参加课程：您将为 DLC 模型库做贡献 😊

   - **幻灯片：** [新项目概览](https://github.com/DeepLabCut/DeepLabCut-Workshop-Materials/blob/main/part1-labeling.pdf)
   - **请阅读：** [DeepLabCut，科学](https://rdcu.be/4Rep)
   - **请阅读：** [DeepLabCut，用户指南](https://rdcu.be/bHpHN)
   - **观看：** 视频教程 1：[使用项目管理器 GUI](https://www.youtube.com/watch?v=KcXogR-p5Ak)
     - 请从项目创建（使用 >1 个视频！）到标记数据，然后检查标签！
   - **观看：** 视频教程 2：[使用项目管理器 GUI 进行多动物姿态估计](https://www.youtube.com/watch?v=Kp-stcTm77g)
     - 请从项目创建（使用 >1 个视频！）到标记数据，然后检查标签！
   - **观看：** 视频教程 3：[使用 ipython/pythonw（更多功能！）](https://www.youtube.com/watch?v=7xwOhUcIGio)
      - 多动物 DLC：[标记](https://www.youtube.com/watch?v=Kp-stcTm77g)
      - 请从项目创建（使用 >1 个视频！）到标记数据，然后检查标签！


### **模块 2：神经网络**

   - **幻灯片：** [创建训练和测试数据以及训练网络的概述](https://github.com/DeepLabCut/DeepLabCut-Workshop-Materials/blob/main/part2-network.pdf)
   - **请阅读：** [卷积神经网络综合指南（通俗易懂版）](https://towardsdatascience.com/a-comprehensive-guide-to-convolutional-neural-networks-the-eli5-way-3bd2b1164a53)

   - **请阅读：** 这是我们描述鲁棒姿态估计挑战的一篇新论文，解释了**预训练**为何如此重要——这是我们在低数据输入姿态估计方面的主要科学贡献——并描述了您可用的新网络。[预训练提高了姿态估计的域外鲁棒性](https://paperswithcode.com/paper/pretraining-boosts-out-of-domain-robustness)

       - **更多详情：** ImageNet：请查看原始论文和数据集：http://www.image-net.org/

   - **阅读论文：** [深度学习运动捕捉入门指南：原理、陷阱与观点](https://www.sciencedirect.com/science/article/pii/S0896627320307170)


 <img src="https://images.squarespace-cdn.com/content/v1/57f6d51c9f74566f55ecf271/1603101997909-7IEYAYZYE9C8AX6SK7GR/ke17ZwdGBToddI8pDm48kND1NDuHF9nqrgeclEdLoeR7gQa3H78H3Y0txjaiv_0fDoOvxcdMmMKkDsyUqMSsMWxHk725yiiHCCLfrh8O1z4YTzHvnKhyp6Da-NYroOW3ZGjoBKy3azqku80C789l0qN_-Z3B7EvygvPOPmeOryWYMQ3pkjXJ5SX4aMqPMuK4PimCRlyu3R6yKl-KltrlZA/networks.jpg?format=2500w" width="350" title="DLC" alt="回顾！" align="right" vspace = "50">

在创建训练/测试集之前，请阅读/观看：
   - **更多信息：** [有哪些可用的神经网络类型，我应该使用哪种？](https://github.com/DeepLabCut/DeepLabCut/wiki/What-neural-network-should-I-use%3F-(Trade-offs,-speed-performance,-and-considerations))
   - **观看：** 视频教程 1：[如何以受控方式测试不同的网络](https://www.youtube.com/watch?v=WXCVr6xAcCA)
     - 现在，决定您想要测试的模型。
        - 如果您想在 CPU 上训练，则在本地计算机上运行 GUI 中的 `create_training_dataset` 步骤等。
        - 如果您想在 Google Colab 上使用 GPU，请[**（1）首先观看此内容/在此处跟随操作！**](https://www.youtube.com/watch?v=qJGs8nxx80A) **（2）将您的整个项目文件夹移至 Google Drive**，然后[**使用此 notebook**](https://github.com/DeepLabCut/DeepLabCut/blob/main/examples/COLAB/COLAB_YOURDATA_TrainNetwork_VideoAnalysis.ipynb)

        **模块 2 网络研讨会**：https://youtu.be/ILsuC4icBU0


### **模块 3：网络性能评估**

   - **幻灯片** [评估您的网络](https://github.com/DeepLabCut/DeepLabCut-Workshop-Materials/blob/master/part3-analysis.pdf)
   - **观看：** [在 ipython 中评估网络](https://www.youtube.com/watch?v=bgfnz1wtlpo)
      - 评估为何重要；如何进行基准测试；分析视频并使用 scoremaps、置信度读数等。

### **模块 4：将分析扩展到许多新视频**

一旦您有了好的网络，就可以部署它们了。您可以创建“cron 作业”来运行定时分析脚本，例如。我们每天在新收集的视频上运行它。查看一个简单的脚本以开始使用，并在下面阅读更多内容：

   - [批量分析视频，跨越多个文件夹，设置自动化数据处理](https://github.com/DeepLabCut/DLCutils/tree/master/SCALE_YOUR_ANALYSIS)

  - 如何在实验室中自动化分析：[datajoint.io](https://datajoint.io)，Cron 作业：[安排您的代码运行](https://www.ostechnix.com/a-beginners-guide-to-cron-jobs/)

### **模块 5：获得姿态数据后该做什么……**

姿态估计消除了数字化数据的痛苦部分，但接下来该怎么办？ 有一整套丰富的工具可以帮助您创建自己的自定义分析，或使用他人的（并根据您的需求进行编辑）。 在下面查看更多信息：

   - [对 DLC 输出使用的辅助代码和包](https://github.com/DeepLabCut/DLCutils)

   - 创建您自己的机器学习分类器：https://scikit-learn.org/stable/

   - **阅读论文：** [迈向计算动物行为学科学](https://www.sciencedirect.com/science/article/pii/S0896627314007934)

   - **阅读论文：** 深度学习在动物姿态估计中的现状，即“用于神经科学中动物行为测量的深度学习工具” [arXiv](https://arxiv.org/abs/1909.13868) 和 [发表版本](https://www.sciencedirect.com/science/article/pii/S0959438819301151)

   - **阅读论文：** [大型行为研究：深度行为分析新时代的挑战与机遇](https://www.nature.com/articles/s41386-020-0751-7)

   - **阅读：** [使用深度感测、视频跟踪和机器学习自动测量小鼠社交行为](https://www.pnas.org/content/112/38/E5351)


*由 Mackenzie Mathis 编纂和编辑*
```