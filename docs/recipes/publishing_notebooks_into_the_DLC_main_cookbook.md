# 将 Notebook 发布到主 DLC 烹饪手册 (Cookbook)
### 为 DLC 烹饪手册贡献的食谱指南

## 引言
嗨，DLC 爱好者！🌟 准备好将您的独门秘籍贡献到主 DLC 烹饪手册了吗？无论您是引入一道新的精彩“菜肴”，还是对旧食谱进行改进，本指南都能为您提供帮助。我们将引导您完成发布新 Notebook 或更新现有 Notebook 的整个过程。让我们开始“烹饪”吧！🍲📘

## 预先检查
### 检查现有食谱或教程
   - **搜索和审阅**: 在开始撰写新食谱之前，请先浏览现有的 DLC Jupyter Book，确保您打算介绍的主题尚未被现有教程或食谱覆盖。
   - **扩展现有内容**: 如果您的内容与现有主题（例如 I/O 操作）相关，请考虑扩展或完善该部分，而不是创建全新的食谱。这有助于保持 Jupyter Book 的简洁性，并将相关信息集中在一个地方。
      - **定位和审阅**: 导航到您希望在 DLC Jupyter Book 中更新的特定食谱或教程。
      - **考虑小改动还是大改动**: 如果您正在添加一个新章节或实质性地修改当前内容，最好在食谱的开头或结尾说明所做的更改，以保持清晰度。
      - **保持一致性**: 确保您的更新符合现有内容的风格、语气和结构，以保持无缝的阅读体验。


## 食谱结构
   在撰写您的食谱时，请遵循以下结构：
   - **引言 (Introduction)**: 以一段介绍性段落开始，强调该食谱的重要性及意义。这为读者奠定了基础并提供了背景信息。
    
   - **示例/工作流程 (Examples/Workflow)**: 提供分步说明或工作流程，并辅以示例支持。这使得读者易于理解和遵循。
    
   - **结论 (Conclusion)**: 以总结或强调食谱关键要点结束。您也可以提供参考资料或进一步阅读的链接。


现在，让我们深入了解将您的内容贡献到 DLC Jupyter Book 的具体流程。
## 步骤

1. **设置本地环境。** 您需要安装 `deeplabcut[docs]`：
   您可以通过运行以下命令来完成此操作：
   ```    
   pip install deeplabcut[docs]
   ```

此命令会安装 DeepLabCut 及其构建文档所需的依赖项。

2. **Fork (派生) DLC 仓库**:
   - 访问 DeepLabCut 的 GitHub 仓库：[https://github.com/DeepLabCut/DeepLabCut](https://github.com/DeepLabCut/DeepLabCut)


   - 点击页面右上角的 `Fork` 按钮。这将在您自己的 GitHub 账户中创建一个仓库副本。
3. **Clone (克隆) 您的派生仓库**:
   - 导航到您在 GitHub 上的派生仓库。
   - 点击 `Code` 按钮并复制 URL。
   - 将仓库克隆到您的本地机器：
   ```
   git clone [REPO_URL]
   ```
4. **创建一个新分支 (Branch)**:
   为每个新功能或更改创建一个新分支是一个良好的实践：
   ```    
    cd [YOUR_REPO_DIRECTORY]
    git checkout -b my-new-notebook
   ```
5. **创建新 Notebook 或更新现有 Notebook**。
   - **创建新 Notebook**
      - **明智地选择主题**: 在开始之前，请确保您的主题适合 DLC Jupyter Book 的主题，并能为读者带来价值。一个新颖的主题或对现有主题的独特阐述会特别有影响力。
      - **精心制作**: 请记住，您的 Notebook 将是许多人的参考资料。从引人入胜的介绍开始，然后是结构良好的内容，最后以结论收尾。
      - **交互式元素**: Jupyter Notebook 的优势之一在于能够组合代码、视觉效果和叙述。使用交互式绘图、小部件（Widgets）或任何其他工具来增强内容，使其更具吸引力。
      - **定期保存**: Jupyter 会自动保存您的工作，但养成频繁手动保存 Notebook 的习惯是个好习惯，尤其是在进行重大更改之后。
      - **命名约定**: 以反映其内容并与其他 DLC Jupyter Book 中的 Notebook 标题保持一致的方式命名您的 Notebook。这使读者能够一眼了解主题。      
   - **更新现有 Notebook** 
      - 导航到目录中现有食谱的位置： 
          ```
          [YOUR_REPO_DIRECTORY]/docs/recipes/
          ```
      - 打开您希望更新的相应 Jupyter Notebook（.ipynb 文件）。
      - 对内容进行必要的更改或添加。
      - 完成更新后，保存 Notebook。
      - 继续执行 **步骤 6**（校对）和 **步骤 9**（测试文档）（跳过步骤 7 和 8）。
6. **校对 (Proofread)**：使用 Jupyter Notebook 的拼写检查扩展程序 `spellchecker`（或您首选的拼写检查器）仔细检查拼写和语法错误。
   ```
   jupyter nbextension enable spellchecker/main
   ```
   安装后，重启您的 Notebook，当您再次加载 Notebook 时，拼写错误的单词将以红色突出显示。
7. **将您的 Notebook 添加到** `[YOUR_REPO_DIRECTORY]/docs/recipes/` **食谱目录中**

    - 导航到 Jupyter Book 中存储 Jupyter Notebook 的适当目录。
    - 将您的 Jupyter Notebook（.ipynb 文件）添加到此目录。
    
    通过终端复制：
    
    - 基于 Unix 的操作系统用户
    
      ```
      cp [YOUR_NOTEBOOK_FILENAME].ipynb [YOUR_REPO_DIRECTORY]/docs/recipes
      ```
    
    - Windows 用户：
      ```
      copy new_recipe.ipynb [YOUR_REPO_DIRECTORY]\docs\recipes

      ```

8. **更新 `[YOUR_REPO_DIRECTORY]/_toc.yml`**，在 *教程与烹饪手册 (Tutorials & Cookbook)* 部分下添加一个**新行**，其中包含指向您的 Notebook 的路径。这将在主 DLC Book 侧边栏中创建指向您 Notebook 的链接。

    * 例如：
      ```      
      - file: docs/recipes/[YOUR_NOTEBOOK_FILENAME]
      ```

9. **测试文档：**

    - 将您的 Notebook 构建到 DLC 食谱书中
      ```
      jupyter book build [YOUR_REPO_DIRECTORY]
      ```
    - 构建成功后，新构建的书籍可以在 `[YOUR_REPO_DIRECTORY]/_build/html/` 访问。
    - 打开 `index.html`，检查您的 Notebook 是否正确渲染以及链接是否工作。

10. **提交更改 (Commit your changes)：**
    当一切就绪后，将您的更改提交到您的分支。如果没有，请编辑您的文件并返回步骤 1。
    
    ```
    git add [YOUR_NOTEBOOK_FILENAME]
    git commit -m "Added a new notebook about [YOUR_TOPIC]"
    ```

11. **将您的分支推送到您的派生仓库 (fork)：**

    ```
    git push origin my-new-notebook
    ```


12. **提交一个拉取请求 (Pull Request, PR)：**

    - 导航到 GitHub 上您的派生仓库。
    - 您很可能会看到一条消息，提示您从最近推送的分支创建拉取请求。点击 `Compare & pull request`。
    - 填写 PR 表单，提供描述性的标题和评论来描述您的 Notebook。这将帮助维护人员理解您 Notebook 的背景和目的。
    - 点击 `Create pull request`。

13. **进行必要的更改**：DeepLabCut 维护人员随后将审查您的 PR 并提供反馈。如果需要更改，请在本地分支上进行必要的修改，提交它们，然后再次推送分支。PR 将自动更新。

14. **🎉PR 批准：🎉** 一旦您的 PR 获得批准，维护人员将将其合并到主仓库中。您的 Notebook 将成为 DeepLabCut Jupyter Book 的一部分！太棒了！

请始终查看 [DLC 贡献指南](https://github.com/DeepLabCut/DeepLabCut/blob/main/CONTRIBUTING.md)。


## 总结 🎉
好了！🌟 此时，您已经掌握了为 DeepLabCut Jupyter Book 增添亮点的操作手册。请记住，重点不仅是烹制新食谱，也包括改进旧食谱。参与进来，享受过程，让我们一起将这本书打造成所有 DLC 爱好者的丰盛“风味盛宴”。为加入这场盛会干杯！🙌🎈