# DeepLabCut 模型库！

![image](https://images.squarespace-cdn.com/content/v1/57f6d51c9f74566f55ecf271/8957c690-4f27-4430-8581-4161fd58d052/68747470733a2f2f696d616765732e73717561726573706163652d63646e2e636f6d2f636f6e74656e742f76312f3537663664353163396637343536366635356563663237312f313631363439323337333730302d50474f41433732494f4236415545343756544a582f6b6531375a77644742546f646449.png?format=450w)


## 🏠 [主页](http://modelzoo.deeplabcut.org/)


Model Zoo 起步于 2020 年，于 2022 年由博士生 [Shaokai Ye 等人](https://arxiv.org/abs/2203.07436v1) 扩展，并在 2024 年发布了首批正式的 [SuperAnimal 基础模型](#about-the-superanimal-models) 🔥。Model Zoo 包含四个组成部分：

- (1) 这是一个模型集合，这些模型在多样化的（通常是大型）数据集上进行训练，这意味着您无需自己训练模型，可以直接在您的研究应用中使用它们。
- (2) 这是一个社区众包网站，用于收集专家标注的关键点数据，以改进模型！您可以在此处参与贡献：[contrib.deeplabcut.org](https://contrib.deeplabcut.org/)。
- (3) 一个无需安装的 DeepLabCut 版本，您可以在 ♾[Google Colab](https://github.com/DeepLabCut/DeepLabCut/blob/main/examples/COLAB/COLAB_DEMO_SuperAnimal.ipynb) 上使用，在 🕸[浏览器](https://contrib.deeplabcut.org/) 中测试我们的模型，或者在我们的 🤗[HuggingFace](https://huggingface.co/spaces/DeepLabCut/DeepLabCutModelZoo-SuperAnimals) 应用上使用！
- (4) 新方法，用于创建 SuperAnimal 基础模型，这些模型整合了来自不同实验室/数据集、关键点、动物/物种的数据，并可应用于您自己的数据！

## 快速开始：
```
pip install deeplabcut[gui,modelzoo]
```

## 关于 SuperAnimal 模型

动物姿态估计在从神经科学到兽医学的应用中都至关重要。然而，可靠地推断动物姿态目前通常需要领域知识和大量的标注工作。为了简化在各种环境和物种中访问高性能动物姿态估计模型的难度，我们提出了一种用于预训练和微调的新范式，该范式在两大类动物姿态数据（四足动物和实验小鼠）上提供了出色的零样本（无需训练）性能。

为了让社区能够方便地访问这些跨越不同环境和物种的高性能模型，我们提出了一种构建预训练动物姿态模型的新范式——我们称之为 **SuperAnimal 模型**——并提供了将它们用于迁移学习（例如，根据需要进行微调）的能力。

## SuperAnimal 成员：
- 模型是基于其训练数据来命名的，例如 `superanimal_quadruped_x` 是在 [SuperAnimal-Quadruped-80K]([https://zenodo.org/records/10619173](https://zenodo.org/records/10619173)) 上训练的。每个模型类别在下面进行了描述：

### SuperAnimal-Quadruped：

- `superanimal_quadruped_x` 模型旨在适用于广泛的四足动物，从马、狗、羊、啮齿动物到大象。相机的视角与动物正交（“侧视图”），并且大部分数据包括动物的面部（即动物的前部和侧面）。您会注意到我们有几个变体，它们在速度与性能之间有所不同，请务必在您的数据上进行测试，以确定最适合您应用的变体。还要注意我们有一个“视频适配”功能，它允许您以自监督的方式将您的数据适配到模型中。无需标注！
- [请在此处查看完整的数据手册](https://zenodo.org/records/10619173)
- [关于模型的更多详细信息（检测器、姿态估计器）](https://huggingface.co/mwmathis/DeepLabCutModelZoo-SuperAnimal-Quadruped)
- 我们提供以下几种模型：
    - `superanimal_quadruped_hrnetw32` (pytorch 引擎)
        - `superanimal_quadruped_hrnetw32` 是一个自上而下的模型，它与检测器配对使用。这意味着它从对象检测器获取裁剪后的图像并预测关键点。该对象检测器目前是一个经过训练的 [ResNet50 基础的 Faster-RCNN](https://pytorch.org/vision/stable/models/faster_rcnn.html)。
    - `superanimal_quadruped_dlcrnet` (tensorflow 引擎)
        - `superanimal_quadruped_dlcrnet` 是一个自下而上的模型，它预测所有关键点，然后将它们分组到独立的个体中。这可能运行得更快，但也更容易出错。
    - `superanimal_quadruped` -> 这与 `superanimal_quadruped_dlcrnet` 相同，这是旧的命名方式，目前正在弃用。
    - 对于所有模型，当使用时，它们会自动下载到 `modelzoo/checkpoints` 目录下。

- 以下是模型训练所用示例图像：
![SA_Q](https://user-images.githubusercontent.com/28102185/209957688-954fb616-7750-4521-bb52-20a51c3a7718.png)



### SuperAnimal-TopViewMouse：

- `superanimal_topviewmouse_x` 旨在处理不同实验环境下的实验小鼠，采用俯视视角；这在许多自由活动小鼠的行为分析实验中非常常见。
- [请在此处查看完整的数据手册](https://zenodo.org/records/10618947)
- [关于模型的更多详细信息（检测器、姿态估计器）](https://huggingface.co/mwmathis/DeepLabCutModelZoo-SuperAnimal-TopViewMouse)
- 我们提供以下几种模型：
    - `superanimal_topviewmouse_hrnetw32` (pytorch 引擎)
        - `superanimal_topviewmouse_hrnetw32` 是一个自上而下的模型，它与检测器配对使用。这意味着它从对象检测器获取裁剪后的图像并预测关键点。该对象检测器目前是一个经过训练的 [ResNet50 基础的 Faster-RCNN](https://pytorch.org/vision/stable/models/faster_rcnn.html)。
    - `superanimal_topviewmouse_dlcrnet` (tensorflow 引擎)
        - `superanimal_topviewmouse_dlcrnet` 是一个自下而上的模型，它预测所有关键点，然后将它们分组到独立的个体中。这可能运行得更快，但也更容易出错。
    - `superanimal_topviewmouse` -> 这与 `superanimal_topviewmouse_dlcrnet` 相同，这是旧的命名方式，目前正在弃用。
    - 对于所有模型，当使用时，它们会自动下载到 `modelzoo/checkpoints` 目录下。
    
- 以下是模型训练所用示例图像：
![SA-TVM](https://user-images.githubusercontent.com/28102185/209957260-c0db72e0-4fdf-434c-8579-34bc5f27f907.png)

### SuperAnimal-Human：

- `superanimal_humanbody` 模型旨在处理来自各种摄像机视角和环境下的人体姿态估计。这些模型旨在处理人体运动分析、体育分析和行为研究中常见的人体姿势、活动和光照条件。
    - `superanimal_humanbody_rtmpose_x` (pytorch 引擎)
        - `superanimal_humanbody_rtmpose_x` 是一个自上而下的模型，它与 `torchvision` 预训练的检测器配对使用。这意味着它从对象检测器获取裁剪后的图像并预测关键点。该模型使用 COCO body7 格式中的 17 个身体部位。


### 实践示例：在不训练的情况下使用 SuperAnimal 模型进行推理。

您可以简单地调用模型并运行视频推理。

值得注意的是，一个好的步骤通常是使用我们的自监督视频适配方法来减少抖动。在 `deeplabcut.video_inference_superanimal` 函数中，只需将 `video_adapt` 选项设置为 `__True__` 即可。请注意，启用此选项会（轻微地）延长处理时间。

```python
import deeplabcut
video_path = "demo-video.mp4"
superanimal_name = "superanimal_quadruped"

deeplabcut.video_inference_superanimal([video_path],
                                        superanimal_name,
                                        model_name="hrnet_w32",
                                        detector_name="fasterrcnn_resnet50_fpn_v2",
                                        video_adapt = False)
```


### 实践示例：自下而上使用 SuperAnimal 模型，考虑视频/动物大小。

在我们的工作中，我们引入了一个空间金字塔结构，用于智能地重新缩放图像。想象一下，如果您的视频帧远大于我们训练时使用的尺寸，模型将很难找到该动物！在这种情况下，您可以通过 `scale_list` 来简单地指导模型：

```python
import deeplabcut
video_path = "demo-video.mp4"
superanimal_name = "superanimal_quadruped"

# scale_list 的目的是聚合来自各种图像尺寸的预测结果。我们预计动物在图像中的外观大小约为 400 像素。
scale_list = range(200, 600, 50)

deeplabcut.video_inference_superanimal([video_path],
                                        superanimal_name,
                                        model_name="hrnet_w32",
                                        detector_name="fasterrcnn_resnet50_fpn_v2",
                                        scale_list=scale_list,
                                        video_adapt = False)
```

### 实践示例：使用 SuperAnimal 权重进行迁移学习。
在 `deeplabcut.train_network` 函数中，`superanimal_transfer_learning` 选项起着关键作用。如果设置为 `__True__`，它将使用一个新的解码层，并允许您在任何项目中（无论关键点数量如何）使用 superanimal 权重。但是，如果设置为 `__False__`，您则是在进行微调。因此，请确保您的数据集具有正确数量的关键点。

具体来说：
    * `superanimal_quadruped_x` 使用 39 个关键点
    * `superanimal_topviewmouse_x` 使用 27 个关键点
    * `superanimal_humanbody_x` 使用 17 个关键点

```python
import os
import deeplabcut
from deeplabcut.modelzoo import build_weight_init

superanimal_name = "superanimal_topviewmouse"

config_path = os.path.join(os.getcwd(), "openfield-Pranav-2018-10-30", "config.yaml")

weight_init = build_weight_init(
    cfg=config_path,
    super_animal=superanimal_name,
    model_name="hrnet_w32",
    detector_name="fasterrcnn_resnet50_fpn_v2",
    with_decoder=False,
)

deeplabcut.create_training_dataset(config_path, weight_init = weight_init)

deeplabcut.train_network(config_path,
                         epochs=10,
                         superanimal_name = superanimal_name,
                         superanimal_transfer_learning = True)
```

### SuperAnimal 模型潜在的故障模式及修复方法。

空间域偏移：典型的深度神经网络 (DNN) 模型会受到训练数据集与测试视频之间空间分辨率差异的影响。为了帮助我们的模型找到合适的分辨率，请在 API 中尝试 `scale_list` 的一个范围（API 文档中有详细信息）。对于 `superanimal_quadruped`，我们经验性地观察到，如果您的视频尺寸大于 1500 像素，最好传递 1000 范围内的 `scale_list`。

像素统计域偏移：您的视频的亮度可能与我们的训练数据集大不相同。这可能导致视频中出现抖动的预测，或者在实验小鼠视频中出现故障模式（如果小鼠的亮度与我们的训练数据集相比异常）。您可以使用我们的“视频适配”模型来解决此问题。



### 我们更长远的展望……

通过 DeepLabCut Model Zoo，我们旨在提供即插即用的模型，这些模型无需任何标注，并且可以在新的视频上良好地工作。如果由于如下所述的故障模式导致预测效果不够理想，请向我们提供反馈！我们正在快速改进我们的模型和适配方法。我们将继续把这个项目扩展到新的模型/数据类别。如果您有数据或想法，请与我们联系：modelzoo@deeplabcut.org

## 出版物：

要查看关于这项工作的初步预印本，请点击 [此处](https://arxiv.org/abs/2203.07436v1)。

我们关于此项目的首篇 [出版物](https://www.nature.com/articles/s41467-024-48792-2) 现已在《自然-通讯》（Nature Communications）上发表：

```{hint}
引用如下：
@article{Ye2024,
  title={SuperAnimal pretrained pose estimation models for behavioral analysis},
  author={Shaokai Ye and Anastasiia Filippova and Jessy Lauer and Steffen Schneider and Maxime Vidal and Tian Qiu and Alexander Mathis and Mackenzie Weygandt Mathis},
  journal={Nature Communications},
  year={2024},
  preprint={abs/2203.07436}
}
```