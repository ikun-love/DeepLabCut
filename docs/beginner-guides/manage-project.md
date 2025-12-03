# 设置要跟踪的关键点

<img src="https://images.squarespace-cdn.com/content/v1/57f6d51c9f74566f55ecf271/1572296495650-Y4ZTJ2XP2Z9XF1AD74VW/ke17ZwdGBToddI8pDm48kMulEJPOrz9Y8HeI7oJuXxR7gQa3H78H3Y0txjaiv_0fDoOvxcdMmMKkDsyUqMSsMWxHk725yiiHCCLfrh8O1z5QPOohDIaIeljMHgDF5CVlOqpeNLcJ80NK65_fV7S1UZiU3J6AN9rgO1lHw9nGbkYQrCLTag1XBHRgOrY8YAdXW07ycm2Trb21kYhaLJjddA/DLC_logo_blk-01.png?format=1000w" width="150" title="DLC-live" alt="DLC LIVE!" align="right" vspace = "50">

**编辑配置文件**

创建 DeepLabCut（DLC）项目后，您需要进入主 GUI 窗口，并在“项目管理”（Project Management）选项卡中开始管理您的项目。

**访问配置文件**

- **定位配置文件：** 在主窗口的顶部，您会找到配置文件的文件路径。
- **编辑文件：** 点击 **`Edit config.yaml`**。此操作允许您：
  - 定义希望跟踪的身体部位（bodyparts）。
  - 勾勒出骨架结构（可选操作！）。

此时会打开一个 **`Configuration Editor`**（配置编辑器）窗口，显示所有的配置细节。您需要修改其中一些设置，以符合您的研究需求。

## 编辑配置的步骤

### 1. 定义身体部位（Bodyparts）

- **定位身体部位部分：** 在配置编辑器中，找到 **`bodyparts`** 类别。
- **修改列表：** 点击 **`bodyparts`** 旁边的箭头以展开列表。在这里，您可以：
  - 使用与您的研究相关的身体部位名称来更新列表。
  - 通过右键单击行号并选择 **`Insert`**（插入）来添加更多条目。

![Editing Bodyparts in DeepLabCut's Config File](https://images.squarespace-cdn.com/content/v1/57f6d51c9f74566f55ecf271/1717779624617-CIVZCM23U69NYK9BO3GY/bodyparts.png?format=500w)

### 2. 定义骨架（Skeleton）

- **导航到骨架部分：** 向下滚动到 **`skeleton`** 类别。
- **调整骨架列表：** 点击箭头展开此部分。然后您可以：
  - 更新身体部位的配对，以定义模型的骨架结构。

![Defining the Skeleton Structure in Config File](https://images.squarespace-cdn.com/content/v1/57f6d51c9f74566f55ecf271/1717779598505-HQNECHIKSQ6XL033JX8M/skeleton.png?format=500w)

> 💡 **提示：** 如果您是 DeepLabCut 的新手，建议花些时间思考如何有效地连接所选的身体部位，以形成一个连贯的骨架。

### 保存更改

- **保存配置：** 对修改满意后，点击 **`Save`**（保存）。这将保存您的更改并返回主 GUI 窗口。

## 接下来，请转到初学者指南中的 [数据标注](labeling) 部分