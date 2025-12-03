# 一些数据处理配方！

## 标记身体部位距离异常的帧

除了使用 `deeplabcut.check_labels` 之外，您可能还想自动检测那些两个身体部位之间的距离超过给定阈值的已标记帧。例如，您可以按如下方式找到头部到尾部的距离超过 100 像素的帧：

```python
import numpy as np
import pandas as pd

max_dist = 100
df = pd.read_hdf('path_to_your_labeled_data_file')
bpt1 = df.xs('head', level='bodyparts', axis=1).to_numpy()
bpt2 = df.xs('tail', level='bodyparts', axis=1).to_numpy()
# 我们计算从一个点到另一个点的向量，并按帧和动物进行分组。
try:
    diff = (bpt1 - bpt2).reshape((len(df), -1, 2))
except ValueError:
    diff = (bpt1 - bpt2).reshape((len(df), -1, 3))
dist = np.linalg.norm(diff, axis=2)
mask = np.any(dist >= max_dist, axis=1)
flagged_frames = df.iloc[mask].index
```