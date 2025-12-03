# 自动化训练和视频分析：批量处理

## 使用 DLC 网络的小技巧：

现在您已经有了一个 DLC 网络，并且对选定视频上的性能感到满意，您可能希望在所有视频上运行它，而无需手动干预。如果所有视频都在一个文件夹中，这很容易实现，只需将文件夹名称传递给 `deeplabcut.analyze_videos(config,[folder])` 即可。但如果视频分散在不同位置呢？

您可以创建一个简单的脚本，遍历所有包含您选择的网络的视频文件夹。这个网络的核心“密钥”是您的 `config.yaml` 文件。

![](https://static1.squarespace.com/static/57f6d51c9f74566f55ecf271/t/5ccc5abe0d9297405a428522/1556896461304/howtouseDLC-01.png?format=1000w)

这是一个可用于对所有文件夹中的视频进行分析的脚本。

https://github.com/DeepLabCut/DLCutils/tree/master/SCALE_YOUR_ANALYSIS (下文也有提及)

注意：如果视频已经被分析过，它将不会被重新分析！或者，您可以使用 `destfolder` 标志将输出推送到其他位置。输入 `deeplabcut.analyze_videos?` 可查看您的所有可用选项。

下面是一个示例脚本。您可以复制粘贴到一个文件中，并以 ".py" 结尾，使其成为一个 Python 脚本。

```python
#!/usr/bin/env python3
# -*- coding: utf-8 -*-
"""
Created on Sun Feb 10 16:04:37 2019

@author: alex
"""

import os

import deeplabcut

def getsubfolders(folder):
    ''' 返回子文件夹列表 '''
    return [os.path.join(folder, p) for p in os.listdir(folder) if os.path.isdir(os.path.join(folder, p))]

project = "ComplexWheelD3-12-Fumi-2019-01-28"

shuffle = 1

prefix = "/home/alex/DLC-workshopRowland"

projectpath = os.path.join(prefix, project)
config = os.path.join(projectpath, "config.yaml")

basepath = "/home/alex/BenchmarkingExperimentsJan2019"

'''

假设数据（此处：3 种类型的视频）位于子文件夹中：
    /January/January29 ..
    /February/February1
    /February/February2

    等等

'''

subfolders = getsubfolders(basepath)
for subfolder in subfolders: # 在上面的例子中，这会是 January, February 等
    print("Starting analyze data in: ", subfolder)
    subsubfolders = getsubfolders(subfolder)
    for subsubfolder in subsubfolders: # 这会是 February1, 等等...
        print("Starting analyze data in: ", subsubfolder)
        for vtype in [".mp4", ".m4v", ".mpg"]:
            deeplabcut.analyze_videos(config,[subsubfolder],shuffle=shuffle,videotype=vtype,save_as_csv=True)
```

## 那么，如何对多个项目进行训练呢？

通过帮助运行所有人的项目，让您的实验室伙伴满意！我们将其用于研讨会，但可以轻松根据您的需求进行调整。下面是一个示例脚本。您可以复制粘贴到一个文件中，并以 ".py" 结尾，使其成为一个 Python 脚本。
```python
#!/usr/bin/env python3
# -*- coding: utf-8 -*-
"""
Created on Sat Nov 17 14:12:43 2018

一个用于在 3 个不同的 GPU 上自动化分析不同项目的示例脚本。请随时根据您的需求进行调整！

@author: alex mathis

"""

import subprocess, sys
import numpy as np
import itertools
import os

import deeplabcut

epochs = 200

model=int(sys.argv[1])

Projects=[["project1-phoenix-2019-01-28"], ["ComplexWheelD3-12-Fumi-2019-01-28", "maze-ariel-2019-01-28"], ["TBI-BvA-2019-01-28", "group-eli-2019-01-28"]]

shuffle=1

prefix = "/home/alex/DLC-workshopRowland"

for project in Projects[model]:
    projectpath = os.path.join(prefix, project)
    config = os.path.join(projectpath, "config.yaml")

    cfg = deeplabcut.auxiliaryfunctions.read_config(config)
    previous_path = cfg["project_path"]

    cfg["project_path"]=projectpath
    deeplabcut.auxiliaryfunctions.write_config(config, cfg)

    print("This is the name of the script: ", sys.argv[0])
    print("Shuffle: ", shuffle)
    print("config: ", config)

    deeplabcut.create_training_dataset(config, Shuffles=[shuffle])

    deeplabcut.train_network(config, shuffle=shuffle, max_snapshots_to_keep=5, epochs=epochs)
    print("Evaluating...")
    deeplabcut.evaluate_network(config, Shuffles=[shuffle], plotting=True)

    print("Analyzing videos..., switching to last snapshot...")
    for vtype in ['.mp4','.m4v','.mpg']:
        try:
            deeplabcut.analyze_videos(config, [str(os.path.join(projectpath, "videos"))], shuffle=shuffle, videotype=vtype, save_as_csv=True)
        except:
            pass

    print("DONE WITH ", project," resetting to original path")
    cfg["project_path"] = previous_path
    deeplabcut.auxiliaryfunctions.write_config(config, cfg)
```