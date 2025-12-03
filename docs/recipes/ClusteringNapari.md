# napari-DeepLabCut GUI 中的聚类

为了提高模型的性能，可以检查用户定义的标签（或视频推理后输出的 H5 文件）中的错误。您可以纠正这些错误并将其添加回训练数据集中，这个过程被称为**主动学习 (active learning)**。

用户错误可能会严重影响模型性能，因此除了简单的 `check_labels` 之外，此工具还允许您发现自己的错误。如果您对错误如何影响性能感到好奇，请阅读这篇论文：
[A Primer on Motion Capture with Deep Learning: Principles, Pitfalls, and Perspectives](https://www.sciencedirect.com/science/article/pii/S0896627320307170)。

**简单来说：您的数据质量至关重要！**

<img src="https://images.squarespace-cdn.com/content/v1/57f6d51c9f74566f55ecf271/1661886442646-A9JAWGH3JU3WTTTPMNCW/swaps.jpg?format=1000w" width="900" title="DLC" alt="DLC" align="center" vspace = "10">

```{Hint}
**标注陷阱：损坏如何影响性能**
(A) 说明两种类型的标注错误。顶部是地面实况（ground truth），中间是尾基部（tailbase）的标签丢失，底部是标注者交换了耳朵的身份信息（例如从左到右）。(B) 使用 106 帧的小型训练数据集，(A) 中的损坏如何影响测试集上关键点正确百分比 (PCK，Percent of Correct Keypoints)，随着与地面实况的距离从 0 像素（完美预测）增加到 20 像素（较大误差）？x 轴表示地面实况与预测位置的差异（RMSE，以像素为单位），而 y 轴是准确帧的分数（例如，对于未损坏的点，80% 的帧落在 9 像素内，即使在这个小型训练数据集上，但对于被交换点的帧，这一比例下降到 65%）。数据集中被损坏的比例会影响这个值。图中显示的是在 1%、5%、10% 和 20% 的帧中丢失尾基部标签（顶部）或交换耳朵（的百分比，这些帧总共 106 张标注训练图像）。与丢失标签相比，交换标签对网络性能有更显著的不利影响。
```

DeepLabCut 工具箱支持**主动学习**，它通过多种方法提取异常帧，允许用户校正这些帧，然后重新训练模型。有关详细步骤，请参阅
[Nature Protocols 论文](https://www.nature.com/articles/s41596-019-0176-0)，或在文档中查看
[此处](active-learning)。

为了促进这一过程，我们在这里提出一种检测“异常帧”的新方法。
欢迎您的贡献和建议，请测试一下这个
[PR](https://github.com/DeepLabCut/napari-deeplabcut/pull/38) 并提供反馈！

这篇 #cookbook 教程旨在展示 **napari 中聚类的用例**，由 2022 年 DLC AI Resident
[Sabrina Benas](https://twitter.com/Sabrineiitor) 💜 撰写。


## 检测异常帧以精炼标签

### 打开 `napari` 和 `DeepLabCut 插件`

然后打开您的 `CollectedData_<ScorerName>.h5` 文件。我们使用 Horse-30 数据集作为我们的演示和开发集，该数据集在
[Mathis, Biasi et al. WACV 2022](http://horse10.deeplabcut.org/) 中介绍。以下是它应该出现的样子示例：


<img src="https://images.squarespace-cdn.com/content/v1/57f6d51c9f74566f55ecf271/1661885256863-M67UV06P8JHAR1243K1F/1.png?format=750w" width="900" title="DLC" alt="DLC" align="center" vspace = "10">

### 聚类

点击 `cluster` 按钮，等待几秒钟，直到显示一个带有聚类的**新层**：

<img src="https://images.squarespace-cdn.com/content/v1/57f6d51c9f74566f55ecf271/1661885257126-HRBHYJNJHE0TFH42L034/2.png?format=750w" width="900" title="DLC" alt="DLC" align="center" vspace = "10">

您可以点击一个点，然后在右侧的图像中查看带有关键点的信息：

<img src="https://images.squarespace-cdn.com/content/v1/57f6d51c9f74566f55ecf271/1661885255947-G8PFQC41KDMV6JH75RSO/2_b.png?format=750w" width="900" title="DLC" alt="DLC" align="center" vspace = "10">

### 可视化与精炼

如果您决定精炼该帧（我们移动了点以使异常值更加明显），请点击 `show img` 并使用插件功能和说明进行精炼：

<img src="https://images.squarespace-cdn.com/content/v1/57f6d51c9f74566f55ecf271/1661885255421-B9QEUDOJANXWYX4K649G/3.png?format=750w" width="900" title="DLC" alt="DLC" align="center" vspace = "10">

```{Attention}
完成后，您需要点击 `ctl-s` 来保存它。
```

您可以通过点击 `close img` 返回到聚类层并精炼另一张图像。请记住，完成编辑后，您需要点击 `ctl-s` 来保存您的工作。现在您可以获取更新后的 `CollectedData` 文件，创建一个**新的训练混洗 (shuffle)**，然后训练网络！阅读更多关于如何
[创建训练数据集](create-training-dataset) 的信息。

```{hint}
如果您想更改聚类方法，可以修改文件
[kmeans.py](https://github.com/DeepLabCutAIResidency/napari-deeplabcut/blob/cluster1/src/napari_deeplabcut/kmeans.py)
```

::::{important}
您必须保持打开文件的方式（pandas dataframe），并且输出**必须**按以下顺序排列：聚类点、聚类点的颜色，以及帧名称。
::::

```

### 即将推出

- 目前我们用用户标签演示此功能，为了获得最佳模型，这些标签总是值得检查和更正！
- 接下来，我们将支持 machine-labeled.h5 文件，以实现完整的全主动学习支持。

祝大家编码愉快！