# 如何撰写 DLC（DeepLabCut）方法部分

**使用 DeepLabCut 进行姿态估计**

我们使用 DeepLabCut（版本 3.X.X）[Mathis 等人, 2018, Nath 等人, 2019] 来进行身体关键点追踪。具体来说，我们标注了来自 X 个视频/动物的 X 帧图像（然后使用 X% 进行训练，默认值为 95%）。我们使用了基于 X 的神经网络（例如 X = ResNet-50, ResNet-101, MobileNetV2-0.35, MobileNetV2-0.5, MobileNetV2-0.75, MobileNetV2-1, EfficientNet...X, dlcrnet_ms5, cspnext_s, dekr_w32, rtmpose_s 等）*** 并使用默认参数*** 进行了 X 次训练迭代。我们使用 X 次混洗（shuffles）进行了验证，发现测试误差为：X 像素，训练误差为：X 像素（图像大小为 X x X）。然后，我们使用 p 截止值（p-cutoff）X（即 0.9）来确定后续分析所需的 X,Y 坐标。此网络随后被用于分析相似实验设置下的视频数据。

*如果修改了 `pose_config.yaml` 中的任何默认设置，请在此处提及。

常见的可能更改项如下：
* 加载器（loader）：选项包括 `default`、`imgaug`、`tensorpack`、`deterministic`。
* `post_dist_threshold`：默认值为 17，它决定了训练分辨率。
* 优化器（optimizer）：您使用的是默认的 `SGD` 还是 `ADAM`？

*** 此处可以添加额外的参考文献。
如果您使用了 ResNets，请考虑引用 Insafutdinov 等人 2016 年和 He 等人 2016 年的文献。如果您使用了 MobileNetV2，请考虑引用 Mathis 等人 2021 年和 Sandler 等人 2018 年的文献。如果您使用了 DLCRNet，请引用 Lauer 等人 2021 年的文献。

> Mathis, A. et al. Deeplabcut: markerless pose estimation of user-defined body parts with deep learning. Nature Neuroscience 21, 1281–1289 (2018).

> Nath, T. et al. Using deeplabcut for 3d markerless pose estimation across species and behaviors. Nature Protocols 14, 2152–2176 (2019).

> Mathis, A. Biasi, T. et al. Pretraining boosts out-of-domain robustness for pose estimation. WACV (2021).

> Lauer et al. Multi-animal pose estimation and tracking with DeepLabCut. BioRxiv (2021).

> Insafutdinov, E., Pishchulin, L., Andres, B., Andriluka, M. & Schiele, B. DeeperCut: A deeper, stronger, and faster multi-person pose estimation model. In European Conference on Computer Vision, 34–50 (Springer, 2016).

> Sandler, M., Howard, A., Zhu, M., Zhmoginov, A. & Chen, L.-C. Mobilenetv2: Inverted residuals and linear bottlenecks. In Proceedings of the IEEE Conference on Computer Vision and Pattern Recognition, 4510–4520 (2018).

> He, K., Zhang, X., Ren, S. & Sun, J. Deep residual learning for image recognition. In Proceedings of the IEEE conference on computer vision and pattern recognition, 770–778 (2016). URL https://arxiv.org/abs/1512.03385.

如果您想使用我们的网络图表，它可以在 SciDraw.io 上免费获取！ https://scidraw.io/drawing/290
如果您使用了我们的 DLC 徽标，请务必包含 TM 符号，谢谢！