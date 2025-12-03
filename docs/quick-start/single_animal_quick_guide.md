# 单个动物行为训练快速指南
**核心步骤：从项目创建到视频分析的主要流程：**

在终端中打开 ipython：
```
ipython
```

导入 DeepLabCut 库：
```
import deeplabcut
```

创建一个新项目：
```
deeplabcut.create_new_project("project_name", "experimenter", ["path of video 1", "path of video2", ..])
```

设置 `config_path` 变量以方便后续操作，并前往编辑此配置文件：
```
config_path = "yourdirectory/project_name/config.yaml"
```

提取视频帧：
```
deeplabcut.extract_frames(config_path)
```

标注视频帧：
``` 
deeplabcut.label_frames(config_path)
```

检查标注（可选）：
```
deeplabcut.check_labels(config_path)
```

创建训练数据集：
```
deeplabcut.create_training_dataset(config_path)
```

训练网络模型：
```
deeplabcut.train_network(config_path)
```

评估已训练的网络模型：
```
deeplabcut.evaluate_network(config_path)
```

视频分析：
```
deeplabcut.analyze_videos(config_path, ["path of video 1", "path of video2", ..])
```

筛选预测结果（可选）：
```
deeplabcut.filterpredictions(config_path, ["path of video 1", "path of video2", ..])
```

绘制结果（轨迹图）：
```
deeplabcut.plot_trajectories(config_path, ["path of video 1", "path of video2", ..], filtered=True)
```

创建带有标注的视频：
```
deeplabcut.create_labeled_video(config_path, ["path of video 1", "path of video2", ..], filtered=True)
```