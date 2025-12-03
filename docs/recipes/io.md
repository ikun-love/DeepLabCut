# DeepLabCut 中的输入/输出操作

## 分析非常大的视频片段

分析长达数小时的视频可能需要一些时间，但此任务可以方便地分解为对更小视频片段的分析：

```python
import deeplabcut
import os
from deeplabcut.utils.auxfun_videos import VideoWriter

_, ext = os.path.splitext(video_path)
vid = VideoWriter(video_path)
clips = vid.split(n_splits=10)
deeplabcut.analyze_videos(config_path, clips, ext)
```

## 视频重新编码和预处理技巧

在计算机之间移动视频，或者从计算机移动到云存储时，您可能会因为视频损坏而遇到 `analyze_videos` 或 `create_labeled_video` 的问题。
此问题可能在这些步骤中出现，您需要仔细查看追踪信息（traceback）。有时看起来视频已分析完毕，但实际上分析在接近视频末尾时停止了（这是由于为实际帧数分配了更多索引时元数据损坏导致的）。
要解决此问题，最简单的方案可能是重新编码视频。这样做不仅可以解决损坏问题，如果选择得当，还可以在没有明显质量损失的情况下压缩视频。用于视频处理的常用软件包是 FFmpeg，您可以在 DEEPLABCUT 环境下的终端中使用它（无需进入 iPython）。
有许多视频编解码器可用于重新编码视频，如果您希望视频保留在相同的容器中（例如 `.avi`, `.mp4`, `.ts` 等），则应检查哪些编解码器支持对特定容器进行编码。例如，对于 `.avi` 文件是 MJPEG，对于 `.mp4` 文件是 H264 和 H265。
要重新编码视频，只需使用：
```
ffmpeg -i "path_to_video" -c:v codec_name "output_path"
```
例如，要以压缩方式重新编码为 `.mp4` 格式：
```
ffmpeg -i "path_to_video" -c:v h264 -crf 18 -preset fast "output_path"
```
`-crf` 是一个表示质量与大小的折衷参数，范围从 0 到 63，其中 0 表示最高质量但压缩率最低。理想情况下，您应使用 18-23 之间的值，以获得与原始视频相似的视觉质量。
`-preset` 是质量与速度的折衷参数。较高的值会使编码速度变快，但会导致文件尺寸更大和/或质量更差。
对于 `.avi` 文件，您需要更改编解码器和质量指标，因为 `crf` 用于 H264/265 编码器，而不是 MJPEG。例如，带有一定程度压缩的编码方式如下：
```
ffmpeg -i "path_to_video" -c:v mjpeg -q:v 10 "output_path"
```
`-q:v` 是一个质量指标，范围从 1 到 31，合理的值大约在 10 左右。
如果您想压缩所有录制内容以便于存储或迁移到云存储，您可以使用 `for` 循环来遍历目录中具有特定容器的所有视频。假设我们要将 `.avi` 视频转码为 `.mp4` 并减小它们的大小而不损失质量。请注意，该循环必须从包含这些视频的文件夹内部运行：
```
for %i in (*.avi) do ffmpeg -i "%i" -c:v libx265 -preset fast -crf 18 "%~ni.mp4" 
```
此命令会将所有视频重新编码到 `.mp4` 容器中，并以与原始文件相同的名称保存（不会覆盖原文件）。
此外，ffmpeg 还允许您裁剪或缩放视频，这可能有助于提高后续 DLC 工作流程中的推理速度。要裁剪或缩放，需要使用 `-filter:v` 参数，在该参数后添加用于裁剪的 `"crop=Xsize:Ysize:Xstart:Ystart"` 或用于缩放的 `"scale=Xsize:Ysize"`。请注意，在使用 “scale” 时，这些值必须是原始视频尺寸整数除法的结果。如果您想保持纵横比，可以将 X 或 Y 设置为 `-1` 并且只提供其中一个维度；或者可以使用 `“scale=iw/2:ih/2”`，这将使视频的两个维度的尺寸减半。例如，如果您有一个 1920x1080 分辨率的视频，并希望将其缩放到 960x540 以实现更快的推理，同时在循环中从 `.avi` 重新编码并进行一些压缩，命令可能如下所示：
```
for %i in (*.avi) do ffmpeg -i "%i" -c:v libx265 -preset fast -crf 18 -filter:v "scale= iw/2:ih/2" "%~ni.mp4"
```
如果视频中不需要音频，您还可以通过在指定输出文件名之前添加 `-an` 来请求编码器不编码任何音频流，从而节省一些空间，如下所示：
```
for %i in (*.avi) do ffmpeg -i "%i" -c:v libx265 -preset fast -crf 18 -filter:v "scale= iw/2:ih/2" -an "%~ni.mp4"
```