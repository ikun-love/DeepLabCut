```rst
(helper-functions)=
# 辅助函数与高级可选功能文档

存在一些附加函数，它们不是必需的，但可能非常有用。

首先，如果您是 Python 新手，您可能不知道一个很方便的技巧：输入 `deeplabcut.` 然后按 "tab" 键，您就可以查看 `deeplabcut` 中**所有**的函数。您将看到一个庞大的列表！

<p align="center">
<img src="https://images.squarespace-cdn.com/content/v1/57f6d51c9f74566f55ecf271/1567907875609-57X4S1LVZWTRJ8GPM34T/ke17ZwdGBToddI8pDm48kKLvSvW2qdCTCjZZgzhLzasUqsxRUqqbr1mOJYKfIPR7LoDQ9mXPOjoJoqy81S2I8N_N4V1vUb5AoIIIbLZhVYxCRW4BPu10St3TBAUQYVKc0_818bg8q0aD7_W_W22OLw0yYD6y1fnQ3mVB6beYNdnbXafewWM7FbBaWqQqcLy-/options.png?format=1000w" width="90%">
</p>

或者，您可能大致知道函数名称的一部分，但不完全确定，那么您可以开始键入命令，例如，输入 ``deeplabcut.a `` 然后按 tab 键：

<p align="center">
<img src="https://images.squarespace-cdn.com/content/v1/57f6d51c9f74566f55ecf271/1567907844296-STHTZ2SD6UB5WCVEN2I8/ke17ZwdGBToddI8pDm48kJEw9Z-3B5ptjcdSkknf02DlfiSMXz2YNBs8ylwAJx2qgRUppHe6ToX8uSOdETM-XipuQpH02DE1EkoTaghKW779xRs5veesYFcByqmynT9oByNVWkh1tiIAZLs8gRhPycqbSMdPDHKAvTCdk8NbVnE/optionsA.png?format=1000w" width="90%">
</p>


现在，对于这些函数中的任何一个，如果您输入 ``deeplabcut.analyze_videos_converth5_to_csv?``，您将得到：

```text
Signature: deeplabcut.analyze_videos_converth5_to_csv(videopath, videotype='.avi')
Docstring:
默认情况下，当运行 analyze_videos 时输出的姿态数据存储为 MultiIndex Pandas 数组，其中包含网络的名称、身体部位的名称、像素中的 (x, y) 标签位置，以及每个身体部位的每帧的似然度。这些数组存储在与视频存储在同一目录下的高效的分级数据格式 (HDF) 文件中。如果将标志 save_as_csv 设置为 True，数据也将导出为逗号分隔值文件。但是，如果没有设置该标志，那么此函数允许将所有 h5 文件转换为 csv 文件（无需重新分析视频）！

此函数将 hdf (h5) 文件转换为逗号分隔值格式 (.csv)，该格式随后可导入到许多程序中，例如 MATLAB、R、Prism 等。

 参数
----------

    videopath : string
        包含用于分析的视频的完整路径的字符串，或包含所有具有相同扩展名的视频的目录路径。

    videotype: string, optional
        如果输入是目录，则检查视频的扩展名。
只分析具有此扩展名的视频。默认值为 ``.avi``

 示例
-----------

    将文件夹 '/media/alex/experimentaldata/cheetahvideos' 中属于 mp4 视频的所有姿态输出文件转换为 csv 文件。
    deeplabcut.analyze_videos_converth5_to_csv('/media/alex/experimentaldata/cheetahvideos','.mp4')  
```

虽然有些名称冗长得离谱，但我们希望它们是“自解释的”。下面是当前可用辅助函数的列表（该列表肯定会持续更新）。如上所述，要查看任何函数的有关信息，包括**如何**使用它们，请在调用末尾使用 ``?``。


```python
deeplabcut.analyze_videos_converth5_to_csv

deeplabcut.mergeandsplit

deeplabcut.analyze_time_lapse_frames

deeplabcut.convertcsv2h5

deeplabcut.ShortenVideo

deeplabcut.DownSampleVideo

deeplabcut.CropVideo

deeplabcut.adddatasetstovideolistandviceversa

deeplabcut.comparevideolistsanddatafolders

deeplabcut.dropannotationfileentriesduetodeletedimages

deeplabcut.dropduplicatesinannotatinfiles

deeplabcut.load_demo_data

deeplabcut.merge_datasets

deeplabcut.export_model
```

## 模型导出函数：

此函数允许您导出训练良好的单动物模型，以用于实时应用等。此函数是 [Kane et al, 2020 eLife](https://elifesciences.org/articles/61909) 的一部分。请参阅该论文及相关代码库，了解如何使用此工具。

- 另一个示例用途是与 [Bonsai-DeepLabCut](https://github.com/bonsai-rx/deeplabcut) 插件一起使用。具体来说，您需要先从 DLC 导出训练好的模型，然后遵循 Bonsai 特定用法的说明。

```python
deeplabcut.export_model(cfg_path, iteration=None, shuffle=1, trainingsetindex=0, snapshotindex=None, TFGPUinference=True, overwrite=False, make_tar=True)
```

## 关于跨摄像头的Advanced Labeling（高级标注）：

### 如果您有两台摄像头并希望从数据创建 3D 项目，您可以在标注 GUI 中利用此功能：

如果您有多台摄像头，您可能希望使用投影到图像上的外极线（epipolar lines）来帮助您在每台摄像机角度下标注身体的同一位置。外极线是从一个摄像头投影到第二个摄像头图像中可能与第一个摄像头图像中已标注点匹配的所有可能点的线。正确标注的点将落在该投影线的某个位置上。

为了使用外极线进行标注，您必须在**标注之前**完成另外两组步骤。

- 首先，您必须创建一个 3D 项目并校准摄像头——为此，请完成 [3D 概述](3D-overview) 中的步骤 1-3。

- 其次，您必须首先从 `camera_1` 提取图像；在这里，您应该运行标准的 `deeplabcut.extract_frames(config_path, userfeedback=True)`，但只提取来自 1 个摄像头的图像。接下来，您需要提取来自 `camera_2` 的匹配帧：
```python
deeplabcut.extract_frames(config_path, mode = 'match', config3d=config_path3d, extracted_cam=0)
```
您可以设置 `extracted_cam=0` 将所有其他摄像头的图像与 `camera_1` 文件夹中的帧号进行匹配，或者将其更改为匹配其他摄像头。如果您先前使用 `mode='automatic'` 运行了 `deeplabcut.extract_frames`，那么您选择哪个摄像头并不重要。如果您已经从两个摄像头提取了图像，请注意这将覆盖 `camera_2` 的图像。

- 第三，您现在可以使用外极线进行标注：

     - 在这里，像往常一样标注 `camera_1`，即：
    ```python
    deeplabcut.label_frames(config_path)
    ```
    - 然后对于 `camera_2`（现在它将根据 camera_1 的标签计算外极线并将其投影到 GUI 上）：
    ```python
    deeplabcut.label_frames(config_path, config3d=config_path3d)
    ```
```