# 💚 MegaDetector+DeepLabCut 💜

[DeepLabCut-Live](https://github.com/DeepLabCut/DeepLabCut-live) 是一个开源且免费的实时包，它来自 DeepLabCut 框架，允许进行实时、低延迟的姿态估计。 [DeepLabCut-ModelZoo](http://modelzoo.deeplabcut.org/) 是我们不断壮大的预训练动物模型集合，可用于快速部署；通常情况下，使用这些模型无需进行额外的训练。MegaDetector 是一款免费的开源软件，经过训练，可以从相机陷阱图像中检测动物、人物和车辆。如需了解更多信息，请查看 [此处](https://github.com/microsoft/CameraTraps/blob/main/megadetector.md)。

在这个 #cookbook 教程中，我们将向您展示如何使用 MegaDetector 来检测动物，并运行 DeepLabCut-Live（使用 ModelZoo 模型）来获得姿态估计结果。本文档由 2022 年 DLC AI 住院研究员 [Nirel Kadzo](https://github.com/Kadzon) 💜 投稿！

## 什么是 MegaDetector？

 <img src="https://images.squarespace-cdn.com/content/v1/57f6d51c9f74566f55ecf271/1661853650953-3L3EZYF69701J8FJZCPT/anim1.jpeg?format=500ww" width="250" title="DLC" alt="DLC" align="right" vspace = "5">

MegaDetector 能够检测到动物，并在其周围生成一个边界框。感谢 [Sara Beery](https://beerys.github.io/) 在 2022 年夏天访问 #DLCAIResidents，向我们介绍了这个很棒的项目。下面是一个示例结果：



## DeepLabCut-Live

DeepLabCut-Live! 是一个用于运行 DeepLabCut 的实时包。但是，即使您不需要实时性能，也可以将其用作运行 DeepLabCut 的轻量级包。它在 HPC（高性能计算）或服务器，或在应用程序（Apps）中非常有用，就像我们在这里做的一样。要了解更多信息，请查看 [文档](deeplabcut-live)。

### MegaDetector 遇上 DeepLabCut

MegaDetector 和 DeepLabCut 的结合现在可以在**以动物为边界框的图像**上进行动物姿态估计。这是一个使用 `full_macaque` 模型的示例，该模型来自 MacaquePose。该模型由富山大学的 Jumpei Matsumoto 贡献。有关更多详细信息，请参阅他们的论文 [此处](https://www.biorxiv.org/content/10.1101/2020.07.30.229989v2)。如果您使用此模型，请引用他们的论文 [此处](https://doi.org/10.3389/fnbeh.2020.581154)。

 <img src="https://images.squarespace-cdn.com/content/v1/57f6d51c9f74566f55ecf271/1661853652273-KG8FYHYDVJ5IBPY0UDVS/monkmddlc.png?format=500w" width="600" title="DLC" alt="DLC" align="center" vspace = "5">

# 🤗 HuggingFace App

我们使用基于 Gradio 的 [Hugging Face](huggingface.co) 空间来创建 MegaDetector+DeepLabCut 的应用程序，以便您可以与之交互并亲自试用。感谢 [Merve Noyan](https://github.com/merveenoyan) 在 2022 年夏天访问 #DLCAIResidents，教我们了解他们的生态系统，并感谢 DLC 住院计划中的其他应用程序合著者（请参阅 [App 页面](https://huggingface.co/spaces/DeepLabCut/MegaDetector_DeepLabCut)）。

让我们深入了解如何使用该应用程序的详细信息：

1. 点击此链接，您将被重定向到 Hugging Face 上的 [MegaDetector+DeepLabCut 应用程序](https://huggingface.co/spaces/DeepLabCut/MegaDetector_DeepLabCut)。

2. 在 “Input Image”（输入图像）部分上传您的图像，或者直接拖放。

3. 为您的图像选择所需的功能

 <img src="https://images.squarespace-cdn.com/content/v1/57f6d51c9f74566f55ecf271/1661853652069-DDS019L4HA245HZOOI3F/toggle.png?format=500w" width="400" title="DLC" alt="DLC" align="center" vspace = "15">

- ``Select MegaDetector model`` (选择 MegaDetector 模型) 允许您在 `md_v5a` 和 `md_v5b` 之间进行选择，您可以在 [此处](https://github.com/microsoft/CameraTraps/releases) 了解更多信息。它们基于 YOLOv5 运行，这使得它们比以前的版本快 3 到 4 倍。

- ``Select DeepLabCut Model`` (选择 DeepLabCut 模型) 选择与您上传的图像最接近的相关 ModelZoo 模型。所选模型将在图像上运行，以预测动物的关键点。

```{hint}
为了使模型获得接近准确的关键点，您上传到界面的动物应与“Select DeepLabCut Model”面板中列出的动物模型相匹配。
```

- ``Run DLClive`` (运行 DLClive) 复选框允许您直接在图像上运行 DeepLabCut-Live，而无需 MegaDetector。然而，MegaDetector 通常通过屏蔽边界框外的像素来简化姿态估计。但运行它也没有坏处（可能只是速度慢一些），请自行测试 ;)

- ``Set confidence threshold for animal detections`` (设置动物检测置信度阈值)，在上面的示例中，置信度阈值设置为 0.8，这意味着如果 MegaDetector 有 >0.8 的把握认为它是一个动物，它就会放置一个边界框。显示的图像具有 0.94 的置信度水平。

- ``Set confidence threshold for keypoints`` (设置关键点置信度阈值) 表示模型对在动物身上预测准确关键点的信心程度。这通过彩色关键点的透明度显示在动物身上。

- ``Set marker size, Set font size, Select keypoiny label font`` (设置标记大小、设置字体大小、选择关键点标签字体) 是您可以自行选择的设计规范——我们都喜欢漂亮的绘图！

4. 设置完毕并对图像和功能感到满意后，提交图像。预期的输出将显示您的输入图像：动物周围有边界框（如果已使用），跟踪的关键点和标签。还会有一个可下载的 `JSON` 文件，其中包含如下所示的标记数据：

 <img src="https://images.squarespace-cdn.com/content/v1/57f6d51c9f74566f55ecf271/1661853655974-YUBP0QQ1LR144NT37TLP/outputdog.png?format=500w" width="400" title="DLC" alt="DLC" align="center" vspace = "15">

 图像来源：[Scientific American](https://www.scientificamerican.com/article/dogs-personalities-arent-determined-by-their-breed/)。

输出图像上看到的所有信息都记录在 **Download JSON file** (下载 JSON 文件) 中。下面的代码片段已添加注释，以便您对代码的含义有一个总体的了解 😀
```
{
 "date": "2022-08-26",
 "MD_model": "md_v5a",  //使用的 Megadetector 模型
 "file": "image0.jpg",  //上传的图像文件名
 "number_of_bb": 1,     //图像中检测到的边界框数量
 "dlc_model": "full_dog",  //使用的模型
 "bb_0": {              
  "corner_1": [          //左上角
   76.08082580566406,    //x  
   91.02932739257812     //y
  ],
  "corner_2": [          //右下角
   393.8626708984375,    //x
   399.9506530761719     //y
  ],
  "predict MD": "animal",  // MegaDetector 预测图像属于 animal（动物）类别
  "confidence MD": 0.9437874555587769,  // 0.94% 确定是动物
  "dlc_pred": {     //关键点预测坐标
   "Nose": [        //标签
    264.89501953125,    //x
    89.19121551513672,  //y
    0.9611953496932983  //z
   ],
   ...
}
```


```{hint}
要使用更多相机陷阱图像进行实验，请查看 [Lila Science!](https://lila.science/)
```

Hugging Face 界面中也添加了示例，您可以在其中尝试各种动物以了解情况，也可以添加您自己的图像。

## 示例

我们鼓励您尝试在您的相机陷阱或其他动物图像上进行实验。事实上，我们发现它不仅限于相机陷阱图像，您也可以用您的相机拍摄的照片进行测试。看看 Mackenzie 在日内瓦拍摄的一张 🦊 图片，并在 [Hugging Face](huggingface.co) 上使用了 MegaDetector+DeepLabCut。

 <img src="https://images.squarespace-cdn.com/content/v1/57f6d51c9f74566f55ecf271/1661854010041-5RQGQTRSTKUDYU9KSTIE/foxGeneva.png?format=750ww" width="400" title="DLC" alt="DLC" align="center" vspace = "15">

 或者这张在餐厅外面的几个可爱的小家伙 🐶🐶🙀🐶，图片来自 [Twitter 梗](https://twitter.com/standardpuppies/status/1563188163962515457?s=21&t=f2kM2HoUygyLmmAH7Ho-HQ)。

 <img src="https://images.squarespace-cdn.com/content/v1/57f6d51c9f74566f55ecf271/1661853654276-WEA4UUD7I1VEGHXSMIXE/pupscat.png?format=300w" width="400" title="DLC" alt="DLC" align="center" vspace = "15">

```{note}
DLC-Live 允许您批量处理视频和帧，但当前发布的 MegaDetetctor+DeepLabCut-Live 一次只能处理一张图像。但请继续关注后续发布，我们才刚刚开始 ;)
```


### 开发者模式 (Developer Mode):
要在本地运行，您可以在终端中 `git clone` 仓库，然后自己探索 MegaDetector+DeepLabCut :)

在您的终端中运行以下每一行命令：
```bash
git clone https://huggingface.co/spaces/DeepLabCut/MegaDetector_DeepLabCut

conda create -n megaDLC python==3.8
conda activate megaDLC

cd MegaDetector_DeepLabCut

pip install -r requirements.txt
python3 app.py
```

终端应该会打印出供您本地加载的链接。祝您玩得开心，编码愉快！