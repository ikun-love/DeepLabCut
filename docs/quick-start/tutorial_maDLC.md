# 使用 DeepLabCut 进行多动物姿态估计：5 分钟教程

## GUI（图形用户界面）：

使用完整的图形用户界面：只需按照 GUI 中的选项卡操作即可！运行 `python -m deeplabcut` 命令可以启动 GUI。

## 终端（Terminal）：

**导入 deeplabcut 库**
```python
import deeplabcut
```

**(1) 创建一个项目**
```python
project_name = "cutemice"
experimenter = "teamdlc"
video_path = "path_to_a_video_file"
config_path = deeplabcut.create_new_project(
    project_name,
    experimenter,
    [video_paths],
    multianimal=True,
    copy_videos=True,
)
```
> **_注意：_** 请确保指定视频文件的**绝对路径**。
> 在 Windows 上，可以通过按住 <kbd>⇧ Shift</kbd> 键 + <kbd>鼠标右键</kbd>，然后选择 `Copy as path`（复制为路径）快速获取；
> 在 Mac 上，可以通过按住 <kbd>⌥ Option</kbd> 键 + <kbd>鼠标右键</kbd>，然后选择 `Copy as Pathname`（复制为路径名）快速获取。
> Ubuntu 用户只需复制文件，其路径就会被添加到剪贴板。

> 接下来，您可以为 `config_path`：'项目配置文件完整路径*' 设置一个变量。

**(2) 编辑 config.yaml 文件以设置您的项目**
> **_注意：_** 在这里，您将定义关键点名称和动物 ID。您也可以更改下一步要提取的默认帧数（# of frames）。

**(3) 提取视频帧以进行标注**
```python
deeplabcut.extract_frames(
    config_path,
    mode="automatic",
    algo="kmeans",
    userfeedback=False,
)
```
> **_注意：_** 尝试从**许多视频**中提取**少量帧**，而不是从**一个视频**中提取**大量帧**！

**(4) 标注帧**
```python
deeplabcut.label_frames(config_path)
```


**(5) 目视检查已标注的帧**
```python
deeplabcut.check_labels(
    config_path,
    draw_skeleton=False,
)
```

**(6) 创建训练数据集**
```python
deeplabcut.create_multianimaltraining_dataset(
    config_path,
    num_shuffles=1,
    net_type="dlcrnet_ms5",
)
```

**(7) 训练网络**

```python
# PyTorch 引擎
deeplabcut.train_network(
    config_path,
    device="cuda",
    save_epochs=5,
    epochs=200,
)

# TensorFlow 引擎
deeplabcut.train_network(
    config_path,
    saveiters=10000,
    maxiters=50000,
    allow_growth=True,
)
```

**(8) 评估网络**
```python
deeplabcut.evaluate_network(
    config_path,
    plotting=True,
)
```

**(9) 分析视频（提取检测结果和关联成本）**
```python
deeplabcut.analyze_videos(
    config_path,
    [video],
    auto_track=True,
)
```
> **_注意：_** 设置 `auto_track=True` 会自动为您完成第 10-11 步，因此您将直接获得最终的 H5 文件。如果您需要根据数据集更改跟踪的参数，请使用下面的步骤。


**(10) 空间和（局部）时间分组：逐帧跟踪身体部位集合**
```python
deeplabcut.convert_detections2tracklets(
    config_path,
    [video],
    track_method="ellipse",
)
```


**(11) 重建完整的动物轨迹（从轨迹片段中合并）**
```python
deeplabcut.stitch_tracklets(
    config_path,
    [video],
    track_method="ellipse",
    min_length=5,
)
```


**(12) 创建漂亮的视频输出**
```python
deeplabcut.create_labeled_video(
    config_path,
    [video],
    color_by="individual",
    keypoints_only=False,
    trailpoints=10,
    draw_skeleton=False,
    track_method="ellipse",
)
```