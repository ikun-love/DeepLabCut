# DeepLabCut 基准测试 - 用户指南

## 在 DLC 中（跨 DLC 版本和架构）对模型进行基准测试的原因

DeepLabCut 3.0+ 引入了使用 PyTorch 🔥 作为深度学习引擎（并且将弃用 TensorFlow）。
为了保证数据分析的可复现性，将使用 3.0 之前版本的 DeepLabCut 创建的现有模型与在 DeepLabCut 3.0+ 及更高版本中创建的新模型进行基准测试是非常重要的。

在比较不同模型时，使用**相同的训练-测试数据划分**以确保公平比较非常关键。如果模型是在不同数据集上训练的，它们的性能指标将无法准确比较。当比较具有不同架构或不同超参数集的模型性能时，这一点尤为重要。例如，如果我们比较一个模型在“简单”测试图像上的 RMSE 与另一个模型在“困难”测试图像上的 RMSE，这并不能判断哪个模型更好，因为我们不清楚是模型架构本身表现更好，还是因为用于训练的图像本身更“容易”学习。因此，我们不仅需要在**相同的测试图像**上计算指标来比较模型，还必须在**相同的固定训练集**上训练它们，以将数据集与模型架构“解耦”。

使用相同数据划分创建模型可以通过 GUI 或代码进行，本指南将概述这两种方法的步骤。

## 重要文件和文件夹

```
dlc-project
|
|___dlc-models-pytorch
|   |__ iterationX
|       |__ shuffleX
|           |__ pytorch_config.yaml
|  
|___training-datasets
|   |__ metadata.yaml
|
|___config.yaml
```

## 使用 PyTorch 模型对 TensorFlow 模型进行基准测试

### 创建一个 Shuffle（数据划分）

创建具有与现有 Shuffle 相同训练/测试划分的新 Shuffle：

### 在 DeepLabCut GUI 中

1.  首页 (Front page) > 加载项目 (Load project) > 打开项目文件夹 (Open project folder) > 选择 *config.yaml*
2.  选择“创建训练数据集”(‘Create training dataset’) 选项卡
3.  勾选“使用现有数据划分”(Use an existing data split) 选项

    ![create_from_existing](<assets/img1.png>)
4.  点击“查看现有 Shuffle”(‘View existing shuffles’):
    *   这用于查看为项目创建的 Shuffle 的索引，以确定哪个索引可用于分配给新的 Shuffle。
    *   此窗口中描述的元素包括：
        *   `train_fraction`: 用于训练的数据集分数。
        *   `index`: Shuffle 的索引。
        *   `split`: 数据的划分。单独的整数值没有实际意义，但此“split”值表示**哪些 Shuffle 具有相同的划分**（因为它们的**结果**可以相互比较）。
        *   `engine`: 是 PyTorch 还是 TensorFlow Shuffle。

            ![view_existing_sh](<assets/img2.png>)
5.  选择要复制的训练 Shuffle 的索引。我们假设想要复制 `OpenfieldOct30-trainset95shuffle3` 中的训练-测试划分，该划分的 `split: 3`。在这种情况下，我们在*“From shuffle”*菜单中输入 `3`：

    ![choose_existing_index](<assets/img3.png>)
6.  要创建这个新数据集，将 Shuffle 选项设置为一个**未使用的** Shuffle 编号（此处为 4）：

    ![choose_new_index](<assets/img4.png>)
7.  点击*“创建训练数据集”(‘Create training dataset’)*，然后转到*“训练网络”(‘train network’)*。Shuffle 应设置为上一步中输入的新的 Shuffle 号（此处为 4）：

    ![create_from_existing](<assets/img5.png>)
8.  要查看/编辑您创建的模型的规格，可以查看位于以下位置的 `pytorch_config.yaml` 文件：
    ```
    dlc-project
    |
    |___ dlc-models-pytorch
        |__ iterationX
            |__ shuffleX
                |__ pytorch_config.yaml
    ```

### 在代码中 (In Code)

使用 Python 中的 `deeplabcut` 模块，请使用 `create_training_dataset_from_existing_split()` 方法从现有 Shuffle（例如 TensorFlow Shuffle）创建新的 Shuffle。

同样地，这里我们从现有的 Shuffle '3' 创建一个新的 Shuffle '4'。

```python
import deeplabcut
from deeplabcut.core.engine import Engine

config = "path/to/project/config.yaml"

training_dataset = deeplabcut.create_training_dataset_from_existing_split(
   config=config,
   from_shuffle=3,
   from_trainsetindex=0,
   shuffles=[4],
   net_type="resnet_50",
)
```

然后，我们可以使用与 TensorFlow 模型**相同的数据划分**来训练我们的新 PyTorch 模型。

```python
deeplabcut.train_network(config, shuffle=4, engine=Engine.PYTORCH, batch_size=8)
```

训练完成后，我们可以使用以下命令评估我们的模型：

```python
deeplabcut.evaluate_network(config, Shuffles=[4], snapshotindex="all")
```
现在，我们可以放心地比较性能了！

### 最佳实践：命名从现有 Shuffle 创建的 Shuffle

在一个拥有多个 TensorFlow 模型并打算将其性能与新的 PyTorch 模型进行基准测试的环境中，遵循我们创建的 Shuffle 的命名约定是一个很好的做法。

假设我们有 TensorFlow Shuffle 0、1 和 2。我们可以通过将新的 PyTorch Shuffle 命名为 1000、1001 和 1002 来从它们创建它们。这使我们能够快速识别出属于 100x 范围的 Shuffle 是 PyTorch Shuffle，并且例如 Shuffle 1001 具有与 TensorFlow Shuffle 1 相同的数据划分。这样，比较可以更直接且保证正确！

此贡献由 [2024 DLC AI Residents](https://www.deeplabcutairesidency.org/our-team) 提供！