# DeepLabCut 基准测试（Benchmark）

有关更多信息和排行榜（Leaderboard），请参阅 [官方主页](https://benchmark.deeplabcut.org/)。

## 高级 API（High Level API）

当您自己实现基准测试时，最重要的函数可以直接在 ``deeplabcut.benchmark`` 包下访问。

```{eval-rst}
.. automodule:: deeplabcut.benchmark
   :members:
   :show-inheritance:
```

## 可用的基准测试定义

有关可用数据集的完整概述，请参阅 [官方基准测试页面](https://benchmark.deeplabcut.org/datasets.html)。基准测试提交（Submission）应包含至少其中一个基准测试的结果。有关如何实现基准测试提交的示例，请参阅 [DeepLabCut 基准测试代码仓库](https://github.com/DeepLabCut/benchmark/tree/main/benchmark/baselines) 中的基线（baselines）部分。

```{eval-rst}
.. automodule:: deeplabcut.benchmark.benchmarks
   :members:
   :show-inheritance:
```

## 指标计算（Metric calculation）

```{eval-rst}
.. automodule:: deeplabcut.benchmark.metrics
   :members:
   :show-inheritance:
```