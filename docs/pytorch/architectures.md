(dlc3-architectures)=
# DeepLabCut 3.0 - PyTorch 模型架构

## 引言 (Introduction)

您可以通过以下方式查看受支持的架构/变体列表：

```python
from deeplabcut.pose_estimation_pytorch import available_models
print(available_models())
```

您可以通过以下方式查看受支持的目标检测架构/变体列表：

```python
from deeplabcut.pose_estimation_pytorch import available_detectors
print(available_detectors())
```

## 神经网络架构 (Neural Networks Architectures)

DeepLabCut PyTorch 目前实现了多种架构（更多架构即将推出，并且您可以轻松地在我们的新模型注册表中添加更多）。另请参阅下面对自底向上/自顶向下方法的解释。

**ResNets**
- 改编自 [He, Kaiming, et al. "Deep residual learning for image recognition." Proceedings of the IEEE conference on Computer Vision and Pattern Recognition. 2016.](https://openaccess.thecvf.com/content_cvpr_2016/html/He_Deep_Residual_Learning_CVPR_2016_paper.html) 和 [Insafutdinov, Eldar et al. "DeeperCut: A Deeper, Stronger, and Faster Multi-Person Pose Estimation Model". European Conference on Computer Vision (ECCV) 2016.]
- 当前的自底向上（bottom-up）变体有：`resnet_50`, `resnet_101`
- 当前的自顶向下（top-down）变体有：`top_down_resnet_101`, `top_down_resnet_50`

**HRNet**
- 改编自 [Wang, Jingdong, et al. "Deep high-resolution representation learning for visual recognition." IEEE transactions on pattern analysis and machine intelligence 43.10 (2020): 3349-3364.](https://arxiv.org/abs/1908.07919)
- 当前变体有：`hrnet_w18`, `hrnet_w32`, `hrnet_w48`
- 当前自顶向下变体有：`top_down_hrnet_w18`, `top_down_hrnet_w32`, `top_down_hrnet_w48`
- 比 ResNets 慢，但通常性能更强 (more powerful)

**DEKR**
- 改编自 [Geng, Zigang et al. "Bottom-Up Human Pose Estimation Via Disentangled Keypoint Regression." Proceedings of the IEEE conference on Computer Vision and Pattern Recognition. 2021.](https://openaccess.thecvf.com/content/CVPR2021/papers/Geng_Bottom-Up_Human_Pose_Estimation_via_Disentangled_Keypoint_Regression_CVPR_2021_paper.pdf)
- 这是一个使用 HRNet 作为骨干网络（backbone）的自底向上模型。它学习预测每个动物的中心点，并预测每个动物中心点与其关键点之间的偏移量 (offset)。
- 当前已实现的变体（从小到大）：`dekr_w18`, `dekr_w32`, `dekr_w48`
- 注意：这是一个功能强大的多动物模型，但非常庞大且速度较慢 (very heavy/slow)

**BUCTD**
- 改编自 [Zhou\*, Stoffl\*, Mathis, Mathis. "Rethinking Pose Estimation in Crowds: Overcoming the Detection Information Bottleneck and Ambiguity." Proceedings of the IEEE/CVF International Conference on Computer Vision (ICCV). 2023](https://openaccess.thecvf.com/content/ICCV2023/papers/Zhou_Rethinking_Pose_Estimation_in_Crowds_Overcoming_the_Detection_Information_Bottleneck_ICCV_2023_paper.pdf)
- [![PWC](https://img.shields.io/endpoint.svg?url=https://paperswithcode.com/badge/rethinking-pose-estimation-in-crowds/pose-estimation-on-crowdpose)](https://paperswithcode.com/sota/pose-estimation-on-crowdpose?p=rethinking-pose-estimation-in-crowds)
- 这是一种性能领先的多动物方法，它结合了自底向上和自顶向下方法的优点，并且在人类（人类也是动物）上也表现出卓越的性能。
- 它可以与多种不同的架构结合使用。当前变体包括：`ctd_coam_w32`, `ctd_coam_w48`/`ctd_coam_w48_human`, `ctd_prenet_hrnet_w32`, `ctd_prenet_hrnet_w48`, `ctd_prenet_rtmpose_s`, `ctd_prenet_rtmpose_m`, `ctd_prenet_rtmpose_x`/`ctd_prenet_rtmpose_x_human`

**DLCRNet**
- 来自 [Lauer, Zhou, et al. "Multi-animal pose estimation, identification and tracking with DeepLabCut." Nature Methods 19.4 (2022): 496-504.](https://www.nature.com/articles/s41592-022-01443-0)
- 该模型使用 ResNet 的多尺度变体作为骨干网络，并使用部件亲和力场 (part-affinity fields) 来组装个体。
- 变体：`dlcrnet_stride16_ms5`, `dlcrnet_stride32_ms5`

**RTMPose**
- 来自 [Jiang, Tao et al. "RTMPose: Real-Time Multi-Person Pose Estimation based on MMPose"](https://arxiv.org/abs/2303.07399)
- 一种自顶向下姿态估计模型，使用快速的 CSPNeXt 骨干网络和 SimCC 样式的头部网络 (head)。
- 变体：`rtmpose_s`, `rtmpose_m`, `rtmpose_x`

**AnimalTokenPose**
- 改编自 [Li, Yanjie, et al. "Tokenpose: Learning keypoint tokens for human pose estimation." Proceedings of the IEEE/CVF International conference on computer vision. 2021.](https://arxiv.org/abs/2104.03516)，如 Ye 等人在 "SuperAnimal pretrained pose estimation models for behavioral analysis." Nature Communications. 2024 中所述](https://arxiv.org/abs/2203.07436)
  - 实现的一个变体是：`animal_tokenpose_base`，仅用于视频推理（我们不支持在 deeplabcut 中直接训练此模型）


## 关于单动物模型的信息 (Information on Single Animal Models)

单动物模型由一个骨干网络（编码器）和一个预测关键点位置的头部网络（解码器）组成。默认的头部网络包含一个反卷积层 (deconvolutional layer)。要创建由骨干网络和头部网络组成的单动物模型，您可以调用 `deeplabcut.create_training_dataset` 并将 `net_type` 设置为骨干网络名称（例如 `resnet_50` 或 `hrnet_w32`）。

如果您想添加第二个反卷积层（这会使您的模型变慢，但可能会提高性能），您可以简单地编辑 `pytorch_config.yaml` 文件。

当然，任何多动物模型也可以用于单动物项目！

## 多动物姿态估计的方法 (Approaches to Multi-Animal pose estimation)

单动物姿态估计相当直接：模型接收一张图像作为输入，并输出每个身体部位的预测坐标。

多动物姿态估计要复杂得多。您不仅需要在图像中定位身体部位，还需要按个体对这些身体部位进行分组。多动物姿态估计有两种主要方法。

### 自底向上估计 (Bottom-up estimation)

第一种方法，**自底向上 (bottom-up)** 姿态估计，首先在图像中检测身体部位，然后再找出它们是如何组合在一起的（即哪些关键点属于同一个动物）。

![Schema representing the bottom-up approach to pose estimation](
assets/bottom-up-approach.png)

### 带有部件亲和力场的骨干网络 (Backbones with Part-Affinity Fields)

如同 DeepLabCut 2.X 中一样，基础的多动物模型由一个骨干网络（编码器）和一个预测关键点和部件亲和力场 (PAFs) 的头部网络组成。这些 PAFs 用于将关键点组装成个体。

对于多动物项目，将骨干网络（例如 `resnet_50`、`hrnet_w32`）作为 `net_type` 传入，将创建一个由骨干网络以及热图 + PAFs 头部网络组成的新模型。

### 自顶向下估计 (Top-down estimation)

第二种方法，**自顶向下 (top-down)** 姿态估计，采用两步法。首先，使用一个模型（目标检测器）通过其边界框来定位图像中存在的所有动物。然后，通过在每个边界框内预测身体部位来确定每只动物的姿态。

![Schema representing the top-down approach to pose estimation](
assets/top-down-approach.png)

在较少拥挤的场景中，自顶向下方法往往更准确，因为姿态模型只需要处理与单只动物相关的像素。然而，在更拥挤的场景中，姿态估计任务会变得模糊。多个重叠的个体将具有非常相似的边界框，并且姿态模型无法知道它应该为哪只动物预测关键点。

自底向上方法没有这种模糊性，并且还具有仅需要运行一个姿态估计模型的优势，而不是首先需要运行目标检测器。然而，分组关键点是一个难题。

因此，任何单动物模型都可以转换为自顶向下、多动物模型。要做到这一点，只需在单动物模型名称前加上 `top_down` 前缀。目前，可用的检测器有：`ssdlite`、`fasterrcnn_mobilenet_v3_large_fpn`、`fasterrcnn_resnet50_fpn_v2`。

### 混合方法：自底向上 (BU) 加上一个“条件化”的自顶向下 (CTD)

一种新的姿态估计方法，称为自底向上条件化自顶向下（或 **BUCTD**），在 [Zhou, Stoffl, Mathis, Mathis. "Rethinking Pose Estimation in Crowds: Overcoming the Detection Information Bottleneck and Ambiguity." Proceedings of the IEEE/CVF International Conference on Computer Vision (ICCV). 2023](
https://openaccess.thecvf.com/content/ICCV2023/papers/Zhou_Rethinking_Pose_Estimation_in_Crowds_Overcoming_the_Detection_Information_Bottleneck_ICCV_2023_paper.pdf) 中介绍。这是一种混合两阶段方法，利用了自底向上和自顶向下方法的优点，以克服边界框引入的模糊性。它不使用目标检测模型来定位个体，而是使用自底向上姿态估计模型。自底向上模型做出的预测作为提议（或**条件**）提供给姿态估计模型。下图说明了这一点。用现代术语来说，可以说 CTD 模型是“可姿态提示 (pose-promptable)”的。


![BUCTD](https://github.com/amathislab/BUCTD/raw/main/media/BUCTD_fig1.png)
Zhou, Mu, et al. *"Rethinking pose estimation in crowds: overcoming the
detection information bottleneck and ambiguity."* Proceedings of the IEEE/CVF
International Conference on Computer Vision. 2023.