# GUI 中的神经网络训练与评估
<img src="https://images.squarespace-cdn.com/content/v1/57f6d51c9f74566f55ecf271/1572296495650-Y4ZTJ2XP2Z9XF1AD74VW/ke17ZwdGBToddI8pDm48kMulEJPOrz9Y8HeI7oJuXxR7gQa3H78H3Y0txjaiv_0fDoOvxcdMmMKkDsyUqMSsMWxHk725yiiHCCLfrh8O1z5QPOohDIaIeljMHgDF5CVlOqpeNLcJ80NK65_fV7S1UZiU3J6AN9rgO1lHw9nGbkYQrCLTag1XBHRgOrY8YAdXW07ycm2Trb21kYhaLJjddA/DLC_logo_blk-01.png?format=1000w" width="150" title="DLC-live" alt="DLC LIVE!" align="right" vspace = "50">


在训练模型之前，第一步是组装您的训练数据集。

**创建训练数据集：** 切换到相应的选项卡，然后点击 **`Create Training Dataset`**（创建训练数据集）。对于初学者来说，默认设置通常已经足够好用。虽然您可以考虑更强大的模型和数据增强技术，但请相信，对于大多数项目而言，默认设置是一个理想的起点。

> 💡 **注意：** 本指南假设您的本地机器上安装了 GPU。如果您受限于 CPU 性能，并发现训练过程很困难，请考虑使用 Google Colab。我们的 [Colab 指南](https://colab.research.google.com/github/DeepLabCut/DeepLabCut/blob/master/examples/COLAB/COLAB_YOURDATA_TrainNetwork_VideoAnalysis.ipynb) 可以帮助您快速入门！

## 启动训练流程

准备好训练数据集后，就可以开始训练模型了。

- **导航到训练网络：** 前往 **`Train Network`**（训练网络）选项卡。
- **设置训练参数：** 在这里，您需要指定以下内容：
  - **`Display iterations/epochs`**（显示迭代次数/周期）：用于指定训练进度将以多频繁的间隔进行可视化更新。请注意，我们的 TensorFlow 模型使用“迭代次数”（iterations），而 PyTorch 模型使用“周期”（epochs）。
  - **`Maximum Iterations/epochs`**（最大迭代次数/周期）：决定您希望运行多少次迭代。对于 TensorFlow 模型的快速演示，10K 次迭代效果很好。对于 PyTorch 模型，200 个周期就足够好了！
  - **`Number of Snapshots to keep`**（要保留的快照数量）：选择您希望保留多少个模型快照，以及 **`Save iterations`**（保存迭代次数）和它们应该在哪个迭代间隔保存。
- **启动训练：** 点击 **`Train Network`**（训练网络）开始训练。

您可以通过终端窗口密切关注训练进度。这将实时显示您的模型学习的情况（PyTorch 模型的一个额外好处是它还会显示每个周期结束后的评估指标！）。

![DeepLabCut Training in Terminal with TF](https://images.squarespace-cdn.com/content/v1/57f6d51c9f74566f55ecf271/1717779598041-DC8UJA2NXJXG65ZWJH1O/training-terminal.png?format=500w)

## 评估网络

训练完成后，是时候查看模型的性能如何了。

### 评估网络的步骤

1. 找到并点击 **`Evaluate Network`**（评估网络）选项卡。
2. **选择评估选项：**
   - **Plot Predictions**（绘制预测）：选择此项可可视化模型的预测结果，这类似于标准的 DeepLabCut (DLC) 评估。
   - **Compare Bodyparts**（比较身体部位）：选择此项以比较所有身体部位，以进行全面的评估。
3. 点击主窗口右侧的 **`Evaluate Network`**（评估网络）按钮。

> 💡 提示：如果您希望评估所有已保存的快照，请转到配置文件并将 `snapshotindex` 参数更改为 `all`。

### 理解评估结果

- **性能指标：** DLC 将评估模型的最新快照，并生成一个包含性能指标的 `.CSV` 文件。该文件存储在项目文件夹内的 **`evaluation-results`**（对于 TensorFlow 模型）或 **`evaluation-results-pytorch`**（对于 PyTorch 模型）文件夹中。


![Combined Evaluation Results in DeepLabCut](https://images.squarespace-cdn.com/content/v1/57f6d51c9f74566f55ecf271/1717779617667-0RLTM9DVRALN9YIKSHJZ/combined-evaluation-results.png?format=750w))
- **视觉反馈：** 此外，DLC 还会创建子文件夹，其中包含带有已标记身体部位和模型预测结果叠加的视频帧，让您可以直观地衡量网络的性能。

![Evaluation Example in DeepLabCut](https://images.squarespace-cdn.com/content/v1/57f6d51c9f74566f55ecf271/1717779623162-BFDAW37B9TO94EGME2O5/check-labels.png?format=500w))

## 接下来，请转到初学者指南，了解 [如何使用您的新神经网络进行视频分析](video-analysis)