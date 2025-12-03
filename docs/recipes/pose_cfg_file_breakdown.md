```markdown
# `pose_cfg.yaml` 指南手册

::::{warning}
以下内容专门针对基于 Tensorflow 的模型。要阅读基于 Pytorch 模型的相应解释，请
[点击此处](dlc3-pytorch-config)
::::

👋 您好！Mabuhay! Hola! 本指南由 [2023 年 DLC AI 住院医生团队](https://www.deeplabcutairesidency.org/) 编写！

当您使用神经网络进行训练、评估和推理时，需要考虑许多超参数。虽然 DLC 试图设置“放之四海而皆准”的默认参数，但您可能希望根据特定需求进行更改。因此，在本指南中，我们将回顾与神经网络模型及其相关数据增强有关的姿态配置参数！

# 1. 什么是 *pose_cfg.yml* 文件？
<a id="whatisposecfg"></a>
- `pose_cfg.yaml` 文件提供了对一系列训练参数的便捷访问，用户可能需要或必须根据所使用的数据集和任务进行调整。
- 您可以在 `dlc-models > test` 和 `train` 子目录中找到此文件。GUI 中也有一个按钮可以直接打开此文件。
- 本指南旨在为普通用户提供关于这些超参数的直观理解，以及在何种情况下调整它们会很有用。

# 2. 快速开始：完整参数列表目录
<a id="fullparamlist"></a>
- [2. 完整参数列表](#2-full-parameter-list)
  - [2.1 训练超参数](#21-training-hyperparameters)
    - [2.1.A `max_input_size` 和 `min_input_size`](#21a-max_input_size-and-min_input_size)
    - [2.1.B `global_scale`](#21b-global_scale)
    - [2.1.C `batch_size`](#21c-batch_size)
    - [2.1.D `pos_dist_thresh`](#21d-pos_dist_thresh)
    - [2.1.E `pafwidth`](#21e-pafwidth)
  - [2.2 数据增强参数](#22-data-augmentation-parameters)
    - [几何变换](#geometric-transformations)
    - [2.2.1 `scale_jitter_lo` 和 `scale_jitter_up`](#221-scale_jitter_lo-and-scale_jitter_up)
    - [2.1.2 `rotation`](#212-rotation)
    - [2.2.3 `rotratio` (旋转比例)](#223-rotratio-rotation-ratio)
    - [2.2.4 `fliplr` (或水平翻转)](#224-fliplr-or-a-horizontal-flip)
    - [2.2.5 `crop_size`](#225-crop_size)
    - [2.2.6 `crop_ratio`](#226-crop_ratio)
    - [2.2.7 `max_shift`](#227-max_shift)
    - [2.2.8 `crop_sampling`](#228-crop_sampling)
    - [核变换](#kernel-transformations)
    - [2.2.9 `sharpening` 和 `sharpenratio`](#229-sharpening-and-sharpenratio)
    - [2.2.10 `edge`](#2210-edge)
- [参考文献](#references)

<a id="hyperparam"></a>
## 2.1 训练超参数

<a id="input_size"></a>
### 2.1.A `max_input_size` 和 `min_input_size`
默认值分别为 `1500` 和 `64`。

💡专业提示:💡
- 当视频分辨率高于 1500x1500，或者当 `scale_jitter_up` 的值可能超过 1500 时，更改 `max_input_size`。
- 当视频分辨率小于 64x64，或者当 `scale_jitter_lo` 的值可能低于 64 时，更改 `min_input_size`。

<a id="global_scale"></a>
### 2.1.B `global_scale`
默认值为 `0.8`。这是在训练队列中对所有图像进行的第一个、最基本的缩放操作。

💡专业提示:💡
- 对于低分辨率或缺乏细节的图像，将 `global_scale` 增加到 1 可能是有益的，以保持原始尺寸并尽可能多地保留信息。

### 2.1.C `batch_size`
<a id="batch_size"></a>

单动物项目（single animal projects）的默认值是 1，而对于多动物项目（maDLC projects）的默认值是 `8`。它表示每次训练迭代中使用的帧数。

在任何情况下，您可以根据 GPU 内存限制增加 `batch_size`，从而减少训练迭代次数。迭代次数与 `batch_size` 的关系不是线性的，因此 `batch_size: 8` 并不意味着您可以少训练 8 倍的迭代次数。但像所有训练一样，损失函数平台期（plateauing loss）可以作为达到最佳性能的指标。

💡专业提示:💡
- 较高的 `batch_size` 有助于提高模型的泛化能力。

___________________________________________________________________________________

上面提到的值和数据增强参数通常是直观的，了解我们自己的数据后，我们就能决定什么是有益的，什么不是。不幸的是，并非所有超参数都如此简单或直观。在具有挑战性的数据集上，可能需要进行一些调整的两个参数是 `pafwidth` 和 `pos_dist_thresh`。

<a id="pos"></a>
### 2.1.D `pos_dist_thresh`
默认值为 `17`。它是一个窗口的大小，在此窗口内的检测被视为正样本（positive training samples），这意味着它们告诉模型它正朝着正确的方向前进。

<a id="paf"></a>
### 2.1.E `pafwidth`
默认值为 `20`。PAF 代表部分亲和场（part affinity fields）。这是一种学习身体部位对之间关系的方法，通过保留肢体（两个关键点之间的连接）的位置和方向来实现。学习到的这种部分亲和力有助于正确的动物姿态组装，使模型不太可能将一个动物的身体部位与另一个动物的身体部位关联起来。[1](#ref1)
<a id="data_aug"></a>

## 2.2 数据增强参数
最简单的理解方式是，数据增强类似于想象或做梦。人类会根据经验想象不同的场景，最终使我们能更好地理解世界。[2, 3, 4](#references)

同样地，我们训练模型适应不同类型的“想象”场景，这些场景被限制在可预见的范围内，从而最终获得一个鲁棒的模型，该模型更有可能处理新的数据和场景。

根据性质划分的数据增强类别由以下几类组成：
- [**几何变换**](#geometric)
    1. [`scale_jitter_lo` 和 `scale_jitter_up`](#scale_jitter)
    2. [`rotation`](#rot)
    3. [`rotratio`](#rotratio)
    4. [`mirror`](#mirror)
    5. [`crop size`](#crop_size)
    6. [`crop ratio`](#crop_ratio)
    7. [`max shift`](#max_shift)
    8. [`crop sampling`](#crop_sampling)
- [**核变换**](#kernel)
    9. [`sharpening` 和 `sharpen_ratio`](#sharp)
    10. [`edge_enhancement`](#edge)

<a id="geometric"></a>
### 几何变换
**几何变换**，例如*翻转*、*旋转*、*平移*、*裁剪*、*缩放*和*注入噪声*，对于解决训练数据中存在的**位置偏差**非常有效。

<a id="scale_jitter"></a>
### 2.2.1 `scale_jitter_lo` 和 `scale_jitter_up`
*尺度抖动*（Scale jittering）在给定缩放范围内调整图像大小。这使得模型可以从场景中不同大小的物体中学习，从而提高其泛化到新场景或新物体尺寸的鲁棒性。

下图（摘自 [3](#ref3)）说明了两种尺度抖动方法的区别。

<img src="https://images.squarespace-cdn.com/content/v1/57f6d51c9f74566f55ecf271/1690471482096-VLLQJU4H6AH6ESMZGNQW/scale_jittering.png?format=1000w">

在训练过程中，每张图像都会在范围 `[scale_jitter_lo, scale_jitter_up]` 内随机缩放，以增强训练数据。这两个参数的默认值如下：
- `scale_jitter_lo = 0.5`
- `scale_jitter_up = 1.25`

💡专业提示:💡
- ⭐⭐⭐ 如果目标动物在整个视频中尺寸变化不大（例如，没有跳跃或朝向静态相机移动），保持**默认**值**不变**将提供足够的变异性，使模型泛化得更好 ✅

- ⭐⭐然而，如果希望模型能够处理具有**更大（比原始大 25%）**或**更小（比原始小 50%）**主体的新数据，则可能需要调整这些参数：
  - 处理新数据中可能**更大**的动物主体 ➡️ 在这种情况下，增加 *scale_jitter_up* 的值
  - 处理新数据中可能**更小**的动物主体 ➡️ 在这种情况下，减小 *scale_jitter_lo* 的值
  - 在**最少预训练**的情况下，在**新的设置/环境中良好泛化**
  ⚠️ 但作为代价，**训练时间会更长**。😔🕒
- ⭐如果您的相机设置完全是静态的，并且动物的大小差异很小，您也可以尝试**缩短**此范围以**减少训练时间**。😃🕒（⚠️ 但作为代价，您的模型可能只适用于您的数据，泛化能力较差）

<a id="rot"></a>
### 2.1.2 `rotation`
*旋转增强*是通过围绕轴线以 $1^{\circ}$ 到 $359^{\circ}$ 的范围向右或向左旋转图像来完成的。旋转增强的安全性在很大程度上取决于旋转角度参数。较小的旋转，例如在 $+1^{\circ}$ 到 $+20^{\circ}$ 或 $-1^{\circ}$ 到 $-20^{\circ}$ 之间，通常是一个可接受的范围。请记住，随着旋转角度的增加，标签定位的精度可能会降低。

下图（摘自 [2](#ref2)）说明了不同旋转角度的区别。

<img src="https://images.squarespace-cdn.com/content/v1/57f6d51c9f74566f55ecf271/1690471478493-Z4JEWJG0I7MB9AYCB322/augset_rot.png?format=750w">

在训练过程中，每张图像都会围绕其自身进行 $+/-$ `rotation` 角度参数的旋转。默认情况下，此参数设置为 `25`，这意味着图像会进行 $+25^{\circ}$ 和 $-25^{\circ}$ 的旋转增强。如果您想选择退出此增强，请将旋转值设置为 `False`。

💡专业提示:💡
- ⭐ 如果您已经标注了动物所有可能的旋转角度，保持**默认**值**不变**就**足够了** ✅

- 但是，如果您希望模型能够：
  - 处理具有新旋转角度的动物主体的新数据
  - 处理少量标注数据中可能未标注的旋转角度
  - 那么您可能需要调整此参数。
  - 但作为代价，旋转角度越大，原始关键点标签可能保留得越少。

<a id="rotratio"></a>
### 2.2.3 `rotratio` (旋转比例)
此参数在 DLC 模块中表示从训练数据中采样的、将被增强的数据的百分比。默认值设置为 `0.4`，即 $40\%$。这意味着当前批次中的图像有 $40\%$ 的概率被旋转。

💡专业提示:💡
- ⭐ 通常情况下，保持**默认**值**不变**是**足够**的 ✅

<a id="fliplr"></a>
### 2.2.4 `fliplr` (或水平翻转)
**镜像**，也称为**水平轴翻转**，比垂直轴翻转更为常见。这种增强易于实现，并且在 CIFAR-10 和 ImageNet 等数据集上被证明非常有用。然而，在涉及文本识别的数据集（如 MNIST 或 SVHN）上，这不是一个标签保持不变的变换。

下图说明了这一特性（显示在最右侧的列中）。

<img src="https://images.squarespace-cdn.com/content/v1/57f6d51c9f74566f55ecf271/1690471476980-RGW6NDYR5BSMN27G9K30/augset_flip.png?format=1500w">

此参数会随机水平翻转图像以增强训练数据。默认情况下，此参数设置为 `False`，尤其是在关节具有镜像对称性的姿势中（例如，这样左手和右手就不会交换位置）。

💡专业提示:💡
- ⭐ 如果您的标签涉及对称关节，请保持**默认**值**不变**——除非数据集存在偏差（动物主要向一个方向移动，但有时也向相反方向移动）✅
- 在大多数情况下，保持默认值 `False` 可以工作得很好。

<a id ="crop_size"></a>
 ### 2.2.5 `crop_size`
 裁剪是通过去除图像中不需要的像素来实现的，即选择图像的一部分并丢弃其余部分，从而减小输入尺寸。

 在 DeepLabCut 的 *pose_config.yaml* 文件中，默认情况下，`crop_size` 设置为 (`400,400`)，分别代表宽度和高度。这意味着它将从图像中切出此大小的部分。

 💡专业提示:💡
  - 如果您的图像非常大，可以考虑增加裁剪尺寸。但请注意，您将需要一块强大的 GPU，否则可能会遇到内存错误！
  - 如果您的图像非常小，可以考虑减小裁剪尺寸。

 <a id ="cropratio"></a>
 ### 2.2.6 `crop_ratio`
 此外，要裁剪的帧数由变量 `cropratio` 定义，默认设置为 `0.4`。这意味着当前批次内的图像有 $40\%$ 的概率被裁剪。默认情况下，此值效果良好。

 <a id ="max_shift"></a>
 ### 2.2.7 `max_shift`

 每次裁剪图像之间的偏移量由 `max_shift` 变量定义，它解释了相对于裁剪中心的最大相对偏移。默认设置为 `0.4`，这意味着最大偏移量为 $40\%$，这样当同一图像在训练期间被多次遇到时，不会应用完全相同的裁剪（这对于 `density` 和 `hybrid` 裁剪方法尤为重要）。

 下图修改自 [2](#references)。

 <img src="https://images.squarespace-cdn.com/content/v1/57f6d51c9f74566f55ecf271/1690471479692-R078RFZXIQ8K552OIOFP/cropping.png?format=750w">

 <a id ="crop_sampling"></a>
 ### 2.2.8 `crop_sampling`
 同样，我们可以根据图像的外观使用不同的裁剪采样方法（`crop_sampling`）。

 💡专业提示💡
 - 对于人群密集的场景，`hybrid` 和 `density` 方法效果最佳。
 - `uniform` 会随机移除图像的部分区域，完全忽略标注信息。
 - 'keypoint' 以随机关键点为中心进行裁剪，并基于该位置进行操作（如果使用合理的 `crop_size`，可能最适合保留整个动物）。

 <a id ="kernel"></a>
 ### 核变换
 核滤波器在图像处理中常用于锐化和模糊图像。直观地说，模糊图像可能会提高测试期间的运动模糊抵抗能力。相反，用于数据增强的锐化可以提高关注对象的细节。

 <a id ="sharp"></a>
 ### 2.2.9 `sharpening` 和 `sharpenratio`
 在 DeepLabCut 的 *pose_config.yaml* 文件中，默认情况下 `sharpening` 设置为 `False`，但如果我们想使用这种数据增强，可以将其设置为 `True`，并指定 `sharpenratio` 的值，该值默认设置为 `0.3`。*pose_config.yaml* 中没有定义模糊（blurring），但如果用户觉得方便，可以将其添加到数据增强流程中。

 下图修改自 [2](#references)。

 <img src="https://images.squarespace-cdn.com/content/v1/57f6d51c9f74566f55ecf271/1690471480991-HBZAYJP1FY0K8H2KB8DB/kernelfilter.png?format=1500w">

 <a id ="edge"></a>
 ### 2.2.10 `edge`
 关于清晰度，我们有另一个参数 `edge` 增强，它会增强图像的边缘对比度，以提高其表观清晰度。同样，默认情况下此参数设置为 `False`，但如果您想包含它，只需将其设置为 `True` 即可。


# 参考文献
 <ol id="references">
     <li id="ref1">Cao, Z., Simon, T., Wei, S. E., & Sheikh, Y. (2017). Realtime multi-person 2d pose estimation using part affinity fields. In Proceedings of the IEEE conference on Computer Vision and Pattern Recognition (pp. 7291-7299).<a href="https://openaccess.thecvf.com/content_cvpr_2017/html/Cao_Realtime_Multi-Person_2D_CVPR_2017_paper.html">https://openaccess.thecvf.com/content_cvpr_2017/html/Cao_Realtime_Multi-Person_2D_CVPR_2017_paper.html</a></li>
     <li id="ref2">Mathis, A., Schneider, S., Lauer, J., & Mathis, M. W. (2020). A Primer on Motion Capture with Deep Learning: Principles, Pitfalls, and Perspectives. In Neuron (Vol. 108, Issue 1, pp. 44-65). <a href="https://doi.org/10.1016/j.neuron.2020.09.017">https://doi.org/10.1016/j.neuron.2020.09.017</a></li>
     <li id="ref3">Ghiasi, G., Cui, Y., Srinivas, A., Qian, R., Lin, T.-Y., Cubuk, E. D., Le, Q. V., & Zoph, B. (2020). Simple Copy-Paste is a Strong Data Augmentation Method for Instance Segmentation (Version 2). arXiv. <a href="https://doi.org/10.48550/ARXIV.2012.07177">https://doi.org/10.48550/ARXIV.2012.07177</a></li>
     <li id="ref4">Shorten, C., & Khoshgoftaar, T. M. (2019). A survey on Image Data Augmentation for Deep Learning. In Journal of Big Data (Vol. 6, Issue 1). <a href="https://doi.org/10.1186/s40537-019-0197-0">https://doi.org/10.1186/s40537-019-0197-0</a> </li>
 </ol>
```