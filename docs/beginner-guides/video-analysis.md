```markdown
# 使用 DeepLabCut 进行视频分析
<img src="https://images.squarespace-cdn.com/content/v1/57f6d51c9f74566f55ecf271/1572296495650-Y4ZTJ2XP2Z9XF1AD74VW/ke17ZwdGBToddI8pDm48kMulEJPOrz9Y8HeI7oJuXxR7gQa3H78H3Y0txjaiv_0fDoOvxcdMmMKkDsyUqMSsMWxHk725yiiHCCLfrh8O1z5QPOohDIaIeljMHgDF5CVlOqpeNLcJ80NK65_fV7S1UZiU3J6AN9rgO1lHw9nGbkYQrCLTag1XBHRgOrY8YAdXW07ycm2Trb21kYhaLJjddA/DLC_logo_blk-01.png?format=1000w" width="150" title="DLC-live" alt="DLC LIVE!" align="right" vspace = "50">


在训练并评估完您的模型后，下一步就是将其应用于您的视频数据。

**如何分析视频**

1. **导航至 ‘Analyze Videos’ 标签页:** 在这里开始将您训练好的模型应用于视频数据。
2. **选择您的视频格式和文件:**
  - **选择视频格式:** 选择您视频的格式（`.mp4`、`.avi`、`.mkv` 或 `.mov`）。
  - **选择视频:** 点击 **`Select Videos`** 来找到并打开您的视频文件。
3. **开始分析:** 点击 **`Analyze`**。分析时间取决于视频的长度和分辨率。请在终端（terminal）或 Anaconda 提示符（prompt）中跟踪进度。

## 审查分析结果

- **在项目文件夹中查找结果:** 分析完成后，请转到您项目的视频文件夹。
- **分析文件:** 还会找到一个 `.metapickle` 文件、一个 `.h5` 文件，以及可能的一个 `.csv` 文件，这些包含了详细的分析数据。
- **查看 "plot-poses" 子文件夹:** 这个文件夹包含视频分析的视觉输出结果。

![Plot poses](https://images.squarespace-cdn.com/content/v1/57f6d51c9f74566f55ecf271/1717779600836-YOWM5T2MBY0JN1LB537B/plot-poses.png?format=500w)

## 创建带有标记的视频（Labeled Video）

1. **转到 ‘Create Labeled Video’ 标签页:** 此时应该已经选择了之前分析过的视频。
2. 如果尚未选择，请选择您的视频。
3. 点击 **`Create Videos`**。

## 查看带有标记的视频

- 带有标记的视频将位于原始视频的文件夹中，文件名会在原始视频名称后加上模型详细信息和后缀 ‘labeled’。
- 播放该视频以评估模型标记的准确性。

## 享受 DeepLabCutting 的乐趣！
- 查阅更高级的用户指南以了解更多选项！
```