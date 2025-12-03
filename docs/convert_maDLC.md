(convert-maDLC)=
# 如何将 2.2 之前的项目转换为 DeepLabCut 2.2 及以上版本兼容的项目

<img src="https://images.squarespace-cdn.com/content/v1/57f6d51c9f74566f55ecf271/1572296495650-Y4ZTJ2XP2Z9XF1AD74VW/ke17ZwdGBToddI8pDm48kMulEJPOrz9Y8HeI7oJuXxR7gQa3H78H3Y0txjaiv_0fDoOvxcdMmMKkDsyUqMSsMWxHk725yiiHCCLfrh8O1z5QPOohDIaIeljMHgDF5CVlOqpeNLcJ80NK65_fV7S1UZiU3J6AN9rgO1lHw9nGbkYQrCLTag1XBHRgOrY8YAdXW07ycm2Trb21kYhaLJjddA/DLC_logo_blk-01.png?format=1000w" width="150" title="DLC" alt="DLC!" align="right" vspace = "50">


如果你有一个预先存在的 2.2 之前的项目（即 `labeled-data` 文件夹），该项目**只包含单个个体（animal）**，但你希望将其用于 DLC 2.2 或更高版本中的多动物项目（即使用旧数据来训练新的多任务深度神经网络），你需要执行以下操作。

(1) 我们建议您备份您的项目文件夹。

(2) 打开您的 `config.yaml` 文件（可以使用任何文本编辑器，或 Python IDE，如 PyCharm, Spyder, VScode, atom 等）。

<p align="center">
<img src= https://images.squarespace-cdn.com/content/v1/57f6d51c9f74566f55ecf271/1587946828128-VQQRJYYF4I5Q4TK4R7NF/ke17ZwdGBToddI8pDm48kDUwYPb5NcTX7SbsUW3p69pZw-zPPgdn4jUwVcJE1ZvWQUxwkmyExglNqGp0IvTJZUJFbgE-7XRK3dMEBRBhUpz5alHTAeHWMjsyxt20uNzeb3sgcN8_6mzgExgMZEG-xs3TaY24DmEIA6oEFne2xjs/Screen+Shot+2020-04-23+at+10.32.53+PM.png?format=750w width="80%">
 </p>

- 在 `task`, `scorer`, `date`, `project_path` 之后，请添加以下内容（例如，在上面的图片中，您将在第 6 行下方开始添加）：注意，顺序并不重要，但为了与模板保持一致，建议保持一致：

```yaml
multianimalproject: true
individuals:
uniquebodyparts: []
multianimalbodyparts:
identity: false/true
```
- 现在，请在 `individuals` 下为该动物赋予一个新名称，例如：
```yaml
individuals:
- mouse1
```

- `"uniquebodyparts: []` 可以保持为空，除非您有其他希望估计的标记项（可以考虑它们类似于预 2.2 版本中的 bodyparts）；例如，箱子的角落等。所有唯一的身体部位不应与您最终创建的骨架中多动物身体部位相关联。请参阅下方的“高级选项”。

- 请将您原来的 `"bodyparts:"` 移动到 `"multianimalbodyparts:"`（身体部位的名称必须保持不变！）。这些是将被始终完全互连的部分！
```yaml
multianimalbodyparts:
- snout
- leftear
- rightear
- tailbase
```
然后您可以设置 `bodyparts: MULTI!`

(3) 保存 `config.yaml`（请务必仔细检查空格或拼写错误！），然后运行：
```python
deeplabcut.convert2_maDLC(path_config_file, userfeedback=True)
```

现在您会看到 `labeled-data` 中的数据已转换为新格式，并且单动物格式已作为备份保存到一个名为 `CollectedData_ ...singleanimal.h5` 和 `.csv` 的新文件中！

(4) 我们强烈建议您首先运行 `check_labels` 来验证转换是否符合预期，然后再创建多动物训练数据集。例如，您可以将此项目的 `config.yaml` 加载到项目管理器 GUI 中并检查标签，然后使用以下命令创建多动物训练集：
```python
deeplabcut.create_multianimaltraining_dataset(path_config_file)
```
即可开始训练。

**高级选项：** 您也可以将以前的 `bodyparts` 分配给 `uniquebodyparts` 或 `multianimalbodyparts`（你甚至可以把一些留空，这意味着它们将在转换过程中被丢弃）。

示例：假设您有一个项目，其中包含月亮和一个带有两个部位标记的火箭：
`bodyparts: [moon, rocket_tip,rocket_bottom]`

现在您希望使用这个旧项目（`labeled-data`）并在一个新数据集（视频）中处理一个“月亮”但有多个（3 个）火箭的情况。那么请按如下方式转换：
```yaml
individuals: [rocket1, rocket2, rocket3]
uniquebodyparts: [moon]
multianimalbodyparts: [rocket_tip,rocket_bottom]
skeleton: [[[rocket_tip,rocket_bottom]]]
```
在不常见的情况下，如果您的数据中也有多个“月亮”（例如，现在是在木星周围进行测量），但只有一个火箭：
```yaml
individuals: [Io, Europa, Ganymede, Callisto]
uniquebodyparts: [rocket_tip,rocket_bottom]
multianimalbodyparts: [moon]
```
请注意，您可以将单目标跟踪器用于这种情况。如果既有多个“月亮”又有多个“火箭”呢？