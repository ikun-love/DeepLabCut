# 如何引用 DeepLabCut

感谢您使用 DeepLabCut！以下是我们关于在“方法（Methods）”部分引用和说明使用 DeepLabCut 的建议：

如果您使用了我们的代码或数据，我们恳请您引用 **Mathis 等人, 2018** [https://www.nature.com/articles/s41593-018-0209-y](https://www.nature.com/articles/s41593-018-0209-y)。如果使用 Python 包（DeepLabCut2.x+ 版本），请同时引用 **Nath, Mathis 等人, 2019** [https://doi.org/10.1038/s41596-019-0176-0](https://doi.org/10.1038/s41596-019-0176-0)。

如果您使用了 MobileNetV2s 或 EfficientNets 模型，请引用 **Mathis, Biasi 等人, 2021** [https://openaccess.thecvf.com/content/WACV2021/papers/Mathis_Pretraining_Boosts_Out-of-Domain_Robustness_for_Pose_Estimation_WACV_2021_paper.pdf](https://openaccess.thecvf.com/content/WACV2021/papers/Mathis_Pretraining_Boosts_Out-of-Domain_Robustness_for_Pose_Estimation_WACV_2021_paper.pdf)。
如果您使用的是多动物版本 2.2beta+ 或 2.2rc1+，请引用 **Lauer 等人, 2022** [https://www.nature.com/articles/s41592-022-01443-0](https://www.nature.com/articles/s41592-022-01443-0)。
如果您使用的是我们的 SuperAnimal 模型，请引用 **Ye 等人, 2024** [https://www.nature.com/articles/s41467-024-48792-2](https://www.nature.com/articles/s41467-024-48792-2)。

DOI 号（#专家提示：为了方便您查找软件引用，请查看 [CiteAs.org](http://citeas.org/)！）：

- Mathis 等人 2018: [10.1038/s41593-018-0209-y](https://doi.org/10.1038/s41593-018-0209-y)
- Nath, Mathis 等人 2019: [10.1038/s41596-019-0176-0](https://doi.org/10.1038/s41596-019-0176-0)
- Lauer 等人 2022: [10.1038/s41592-022-01443-0](https://doi.org/10.1038/s41592-022-01443-0)
- Ye 等人 2024: [10.1038/s41467-024-48792-2](https://www.nature.com/articles/s41467-024-48792-2)

## 格式化引用：

    @article{Mathisetal2018,
        title = {DeepLabCut: markerless pose estimation of user-defined body parts with deep learning},
        author = {Alexander Mathis and Pranav Mamidanna and Kevin M. Cury and Taiga Abe  and Venkatesh N. Murthy and Mackenzie W. Mathis and Matthias Bethge},
        journal = {Nature Neuroscience},
        year = {2018},
        url = {https://www.nature.com/articles/s41593-018-0209-y}}

     @article{NathMathisetal2019,
        title = {Using DeepLabCut for 3D markerless pose estimation across species and behaviors},
        author = {Nath*, Tanmay and Mathis*, Alexander and Chen, An Chi and Patel, Amir and Bethge, Matthias and Mathis, Mackenzie W},
        journal = {Nature Protocols},
        year = {2019},
        url = {https://doi.org/10.1038/s41596-019-0176-0}}
        
    @InProceedings{Mathis_2021_WACV,
        author    = {Mathis, Alexander and Biasi, Thomas and Schneider, Steffen and Yuksekgonul, Mert and Rogers, Byron and Bethge, Matthias and Mathis, Mackenzie W.},
        title     = {Pretraining Boosts Out-of-Domain Robustness for Pose Estimation},
        booktitle = {Proceedings of the IEEE/CVF Winter Conference on Applications of Computer Vision (WACV)},
        month     = {January},
        year      = {2021},
        pages     = {1859-1868}}
        
    @article{Lauer2022MultianimalPE,
        title={Multi-animal pose estimation, identification and tracking with DeepLabCut},
        author={Jessy Lauer and Mu Zhou and Shaokai Ye and William Menegas and Steffen Schneider and Tanmay Nath and Mohammed Mostafizur Rahman and     Valentina Di Santo and Daniel Soberanes and Guoping Feng and Venkatesh N. Murthy and George Lauder and Catherine Dulac and M. Mathis and Alexander Mathis},
        journal={Nature Methods},
        year={2022},
        volume={19},
        pages={496 - 504}}

    @article{Ye2024SuperAnimal,
        title={SuperAnimal pretrained pose estimation models for behavioral analysis},
        author={Shaokai Ye and Anastasiia Filippova and Jessy Lauer and Steffen Schneider and Maxime Vidal and and Tian Qiu and Alexander Mathis and Mackenzie W. Mathis},
        journal={Nature Communications},
        year={2024},
        volume={15}}


### 综述与教育性文章：

    @article{Mathis2020DeepLT,
        title={Deep learning tools for the measurement of animal behavior in neuroscience},
        author={Mackenzie W. Mathis and Alexander Mathis},
        journal={Current Opinion in Neurobiology},
        year={2020},
        volume={60},
        pages={1-11}}

    @article{Mathis2020Primer,
        title={A Primer on Motion Capture with Deep Learning: Principles, Pitfalls, and Perspectives},
        author={Alexander Mathis and Steffen Schneider and Jessy Lauer and Mackenzie W. Mathis},
        journal={Neuron},
        year={2020},
        volume={108},
        pages={44-65}}

### 其他与 DeepLabCut 相关的开放获取预印本：

    @article{MathisWarren2018speed,
        author = {Mathis, Alexander and Warren, Richard A.},
        title = {On the inference speed and video-compression robustness of DeepLabCut},
        year = {2018},
        doi = {10.1101/457242},
        publisher = {Cold Spring Harbor Laboratory},
        URL = {https://www.biorxiv.org/content/early/2018/10/30/457242},
        eprint = {https://www.biorxiv.org/content/early/2018/10/30/457242.full.pdf},
        journal = {bioRxiv}}



## 方法论建议：

**对于身体部位追踪，我们使用了 DeepLabCut (版本 2.X.X)* [Mathis 等人, 2018, Nath 等人, 2019, Lauer 等人. 2022]。具体来说，我们标记了从 x 个视频/动物中提取的 x 帧（然后使用 x% 进行训练，默认值为 95%）。我们使用了一个基于 x 的神经网络（即 x = ResNet-50, ResNet-101, MobileNetV2-0.35, MobileNetV2-0.5, MobileNetV2-0.75, MobileNetV2-1***），使用默认参数*进行了 x 次训练迭代。我们使用 x 次洗牌（shuffles）进行了验证，发现测试误差为：x 像素，训练误差：x 像素（图像大小为 x 乘 x）。然后我们使用 p-cutoff 为 x（即 0.9）来对 X,Y 坐标进行条件筛选，以供后续分析。然后将该网络应用于分析具有相似实验设置的视频。

> Mathis, A. et al. Deeplabcut: markerless pose estimation
> of user-defined body parts with deep learning. Nature
> Neuroscience 21, 1281–1289 (2018).

> Nath, T. et al. Using deeplabcut for 3d markerless pose
> estimation across species and behaviors. Nature Protocols
> 14, 2152–2176 (2019).

*如果在 *`pose_config.yaml`* 中更改了任何默认设置，请在此处提及。

i.e. 人们可能更改的一些常见设置：
* loader（选项有 `default`, `imgaug`, `tensorpack`, `deterministic`）。
* `post_dist_threshold`（默认为 17，它决定了训练分辨率）。
* 优化器：您使用的是默认的 `SGD` 还是 `ADAM`？

*** 在此处，您可以添加额外的引用。
如果您使用 ResNets，建议引用 Insafutdinov 等人 2016 & He 等人 2016。如果您使用 MobileNetV2s，请考虑引用 Mathis 等人 2019，以及 Sandler 等人, 2018。


> Mathis, A. et al. Pretraining boosts out-of-domain robustness for pose estimation
> arXiv 1909.11229 (2019)

> Insafutdinov, E., Pishchulin, L., Andres, B., Andriluka,
> M. & Schiele, B. DeeperCut: A deeper, stronger, and
> faster multi-person pose estimation model. In European
> Conference on Computer Vision, 34–50 (Springer, 2016).

> Sandler, M., Howard, A., Zhu, M., Zhmoginov, A. &
> Chen, L.-C. Mobilenetv2: Inverted residuals and linear
> bottlenecks. In Proceedings of the IEEE Conference
> on Computer Vision and Pattern Recognition, 4510–4520
> (2018).

> He, K., Zhang, X., Ren, S. & Sun, J. Deep residual
> learning for image recognition. In Proceedings of the
> IEEE conference on computer vision and pattern recognition,
> 770–778 (2016). URL https://arxiv.org/abs/
> 1512.03385.

## 图形

如果您想使用我们的网络图，它也可以在 SciDraw.io 上免费获取！ [https://scidraw.io/drawing/290](https://scidraw.io/drawing/290)

欢迎您在您的工作中也使用我们的徽标。