# 在您自己的数据集上使用 ModelZoo 模型

<p style='text-align: justify;'>动物行为必须以极其精确的方式进行分析。因此，动物姿态估计一直是精确研究动物行为的重要工具。

除了为研究人员提供开源工具箱来开发用于无标记姿态估计的定制化深度神经网络外，DeepLabCut 还致力于构建稳健、可泛化的模型。这项工作的一部分是通过 [DeeplabCut ModelZoo](http://modelzoo.deeplabcut.org/) 来实现的。

ModelZoo 托管了用户贡献和 DLC 团队开发的、针对特定动物和场景训练好的模型。您无需训练即可直接使用这些模型分析您的视频。这些模型在未见过的**域外数据 (out-of-domain data)** 上表现出强大的零样本 (zero-shot) 性能，并且可以通过伪标签 (pseudo-labeling) 进一步改进。有关更多详细信息，请参阅第一个 [ModelZoo 论文](https://arxiv.org/abs/2203.07436v1)。

本指南旨在展示 **mouse_pupil_vclose** 模型的一个用例，由 2022 年 DLC AI 住院研究员 [Neslihan Wittek](https://github.com/neslihanedes) 💜 贡献。

## `mouse_pupil_vclose` 模型

此模型由美国加州大学河滨分校的 Jim McBurney-Lin 贡献。该模型是在 C57/B6J 小鼠眼睛的图像上训练的，并且还通过 EPFL Mathis 实验室的小鼠眼睛数据进行了增强。

 <img src="https://images.squarespace-cdn.com/content/v1/57f6d51c9f74566f55ecf271/1661439618442-RAACYCYD4RWEND4X1UFU/pupil_one.png?format=500w" width="250" title="DLC" alt="DLC" align="left" vspace = "50">

  <img src="https://images.squarespace-cdn.com/content/v1/57f6d51c9f74566f55ecf271/1661439618750-97KC2HW8HH6VOJHLMO46/pupil_two.png?format=300w" width="250" title="DLC" alt="DLC" align="right" vspace = "50">

| Landmark\_Number | Landmark\_Name | 说明 |
| --- | --- | --- |
| 1 | Lpupil | 瞳孔的左侧部分 |
| 2 | LDpupil | 瞳孔的左/背侧部分 |
| 3 | Dpupil | 瞳孔的背侧部分 |
| 4 | DRpupil | 瞳孔的背侧/右侧部分 |
| 5 | Rpupil | 瞳孔的右侧部分 |
| 6 | RVpupil | 瞳孔的右/腹侧部分 |
| 7 | Vpupil | 瞳孔的腹侧部分 |
| 8 | VLpupil | 瞳孔的腹侧/左侧部分 |

由于我们希望评估模型在域外数据上的性能，我们将分析鸽子瞳孔。关于所谓的域外数据的更多讨论和工作，请参阅 [Mathis, Biasi 2020](https://paperswithcode.com/dataset/horse-10)。

## 鸽子瞳孔

眼球的瞳孔负责接纳和调节进入视网膜的光量，以实现图像感知。除了这一关键作用外，瞳孔还能反映大脑的状态。尽管马克斯·普朗克鸟类学研究所（Max Planck Institute for Ornithology in Seewiesen）的研究人员揭示了鸽子瞳孔的行为，但瞳孔的系统性行为在鸟类中尚未得到广泛研究。

雄性鸽子在求偶行为中瞳孔会缩小。这与哺乳动物形成对比，哺乳动物在唤醒程度增加时瞳孔会散大。此外，鸽子在非快速眼动 (non-REM) 睡眠期间瞳孔会散大，而在快速眼动 (REM) 睡眠期间则会迅速收缩。研究这些差异及其背后的原因，可能有助于了解瞳孔行为的普遍规律。

鉴于这些发现，我们想展示 **mouse_pupil_vclose** 模型是否也能在鸽子瞳孔上提供准确的追踪性能。

### Jupyter & Google Colab 笔记本

DeepLabCut 提供了一个 Google Colab 笔记本，用于使用 ModelZoo 中的预训练网络分析您的视频。**无需在本地安装 DeepLabCut！**

由于我们关注 **mouse_pupil_vclose** 模型在鸽子瞳孔数据上的准确性，我们将使用一个包含 7 段鸽子瞳孔记录的视频。

请查看 [ModelZoo Colab 页面](https://github.com/DeepLabCut/DeepLabCut/blob/main/examples/COLAB/COLAB_DLC_ModelZoo.ipynb) 以及有关如何在 Google Colab 上使用 ModelZoo 的视频教程。

<div align="center">
  <a href="https://www.youtube.com/watch?v=twHBa1ZvXM8" target= "_blank"><img src="http://img.youtube.com/vi/twHBa1ZvXM8/0.jpg" alt="图像替代文本"></a>
</div>

```{hint}
您对模型感到满意，并希望在本地机器上继续分析更多视频，或者您想针对您的具体用例优化模型？
```html
!zip -r /content/file.zip /content/pigeon_modelZoo-nessi-2022-08-22
from google.colab import files
files.download("/content/file.zip")

```

### 在本地机器上分析视频

DeepLabCut 托管了来自 [DeepLabCut ModelZoo 项目](http://modelzoo.deeplabcut.org/) 的模型。

`create_pretrained_project` 函数将创建一个新的项目目录，其中包含必要的子目录和一个基本的配置文件。它还将使用来自 DeepLabCut ModelZoo 的预训练模型来初始化您的项目。

其余代码应在您的 DeepLabCut 环境中运行。请查看[此处](how-to-install)获取 DeepLabCut 安装说明。

要使用来自 DeepLabCut ModelZoo 的预训练模型初始化一个新的项目目录，请运行以下代码。

::::{warning}
此方法目前仅对 Tensorflow 实现，Pytorch 兼容性即将推出。
::::

```python
import deeplabcut

deeplabcut.create_pretrained_project(
    "projectname",
    "experimenter",
    [r"path_for_the_videos"],
    model="mouse_pupil_vclose",
    working_directory=r"project_directory",
    copy_videos=True,
    videotype=".mp4 or .avi?",
    analyzevideo=True,
    filtered=True,
    createlabeledvideo=True,
    trainFraction=None,
    engine=deeplabcut.Engine.TF,
)
```

::::{important}
为了获得更好的模型准确性，您的视频应围绕眼睛进行裁剪！👁🐭
::::

令人兴奋的是，7 个鸽子瞳孔中有 6 个被很好地追踪到了：

<img src="https://images.squarespace-cdn.com/content/v1/57f6d51c9f74566f55ecf271/44858d34-dca7-4bb5-a6e5-cd8078b50bec/Screen+Shot+2022-08-25+at+5.39.33+PM.png?format=1500w" width="500" title="DLC" alt="DLC" align="center" vspace = "50">

当我们通过检查被追踪点的置信度来进一步评估模型准确性时，我们发现当鸽子闭上眼睑时（当然这是可以预期的，并且可以利用它来测量眨眼👁），追踪的置信度很低。

<img src="https://images.squarespace-cdn.com/content/v1/57f6d51c9f74566f55ecf271/1661439615047-OVOOMU1Z5NJIWJ1HHNFD/likelihood.png?format=500w" width="600" title="DLC" alt="6只用deeplabcut追踪的鸽子眼睛" align="center" vspace = "50">

但是你也可能遇到比小的追踪故障更大的问题：

<img src="https://images.squarespace-cdn.com/content/v1/57f6d51c9f74566f55ecf271/1661439618037-4GNTZD476MJMQX19N0Z4/pigeon_7.png?format=500w" width="250" title="DLC" alt="1只眼睛" align="center" vspace = "5">

要解决这个问题，您可以提取追踪不佳的异常帧，对其进行精炼，然后将它们馈送到训练数据集中以重新训练。请确保在项目文件夹的 `config.yaml` 文件中设置要标记的帧数。您遇到的问题越多，可能希望标记的帧数就应该越高。

您还应该将视频路径添加到 `config.yaml` 文件中，或者运行以下命令将视频添加到您的项目中：

```python
deeplabcut.add_new_videos(
    "/pathofproject/config.yaml",
    ["/pathofvideos/pigeon.mp4"],
    copy_videos=False,
    coords=None,
    extract_frames=False
)
```
`deeplabcut.extract_outlier_frames` 函数将检查异常点，并询问您的反馈，是否应该提取这些异常帧。

```python
deeplabcut.analyze_videos(
    "/pathofproject/config.yaml",
    ["/pathofvideos/pigeon.mp4"]
)
deeplabcut.extract_outlier_frames(
    "/pathofproject/config.yaml",
    ["/pathofvideos/pigeon.mp4"],
    automatic=True
)
```
`deeplabcut.refine_labels` 函数启动 GUI，允许您手动精炼异常帧。您应该加载前一个模型的异常帧目录和对应的 `.h5` 文件。它会要求您定义 `likelihood`(置信度) 阈值：低于该阈值的标签应在此阶段进行精炼。

精炼后，您应该将这些数据与前一个模型的训练数据集中已有的数据合并，并创建新的训练数据集。

```python
deeplabcut.refine_labels("/pathofproject/config.yaml")
deeplabcut.merge_datasets("/pathofproject/config.yaml")
deeplabcut.create_training_dataset("/pathofproject/config.yaml")
```
在开始训练模型之前，还有最后一步：编辑 `pose_cfg.yaml` 文件中的 `init_weights` 参数。进入您的项目，并在 `dlc-models/train` 目录中查看模型的最新快照（例如 `snapshot-610000`）。编辑 `pose_cfg.yaml` 文件中 `init_weights` 键的值，然后开始重新训练您的模型！

`init_weights: pathofyourproject\dlc-models\iteration-0\DLCFeb31-trainset95shuffle1\train\snapshot-610000`

```python
deeplabcut.train_network("/pathofproject/config.yaml", shuffle=1, saveiters=25000)
```
```{hint}
请查看此视频了解如何精炼模型！
<div align="center">
  <a href="https://www.youtube.com/watch?v=bgfnz1wtlpo" target="_blank"><img src="http://img.youtube.com/vi/bgfnz1wtlpo/0.jpg" alt="图像替代文本"></a>
</div>
```