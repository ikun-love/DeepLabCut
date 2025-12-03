# 如何使用 DeepLabCut 外部标记的数据

- 或者如果您合并了跨评分员（scorer）的项目（见下文）：

## 使用外部标记的数据：

有些用户可能拥有不同格式的注释数据，但仍希望使用 DLC（DeepLabCut）流程。在这种情况下，您需要将数据转换为我们的格式。简单来说，您可以将数据格式化为 Excel 表格（`.csv` 文件）或 pandas 数组（`.h5` 文件）。

以下是通过“`.csv` 路径”进行此操作的指南：（pandas 数组的路径是相同的，只需以相同的方式格式化 pandas 数组即可）。

**步骤 1**: 按照用户指南中所述创建一个项目：https://github.com/DeepLabCut/DeepLabCut/blob/main/docs/UseOverviewGuide.md#create-a-new-project

**步骤 2**: 编辑 ``config.yaml`` 文件以包含身体部位名称。请注意，拼写、间距和大小写必须与“标记数据中的身体部位名称”**完全相同**。

**步骤 3**: 请检查我们 [演示项目](https://github.com/DeepLabCut/DeepLabCut/tree/main/examples/Reaching-Mackenzie-2018-08-30/labeled-data/reachingvideo1) 中的 Excel 格式工作表（`.csv`）：
- 即此文件：https://github.com/DeepLabCut/DeepLabCut/blob/main/examples/Reaching-Mackenzie-2018-08-30/labeled-data/reachingvideo1/CollectedData_Mackenzie.csv

**步骤 4**: 编辑 `.csv` 文件，使其包含 X、Y 像素坐标、身体部位名称、评分员名称以及图像的相对路径：例如 `/labeled-data/somefolder/img017.jpg`。然后确保评分员名称和身体部位名称与 `config.yaml` 文件中的名称相同。

此外，请为 **每个文件夹** 向 `config.yaml` 文件中的 `video_set` 添加一个视频。这也可以是一个虚拟变量，但如果文件夹名为 `somefolder`，则应为例如 `C://somefolder.avi`。有关正确的格式设置，请参阅演示的 `config.yaml` 文件。

**步骤 5**: 完成后，运行 ``deeplabcut.convertcsv2h5('path_to_config.yaml', scorer= 'experimenter')``

- 评分员名称必须与您创建项目时使用的 `experimenter` 输入名称相同。这将自动将示例演示笔记本中名称为 "Mackenzie" 的内容替换为您自己的名称。

## 如果您合并项目：

**步骤 1**: 将 CSV 文件重命名为您目标项目的名称。

**步骤 2**: 运行并传入目标名称 ``deeplabcut.convertcsv2h5('path_to_config.yaml', scorer= 'experimenter')``。这将覆盖 H5 文件，使所有数据都合并到目标名称下。