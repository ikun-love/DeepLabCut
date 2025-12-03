(governance-model)=
# DeepLabCut 的治理模型
（改编自 https://napari.org/stable/community/governance.html）

## 摘要

本文档旨在规范 `DeepLabCut` 项目所使用的治理流程，阐明决策的制定方式以及社区各个组成部分之间的互动方式。

这是一个基于共识的社区项目。任何对该项目感兴趣的人都可以加入社区，为项目设计做出贡献，并参与决策过程。本文描述了这种参与如何进行，如何达成共识，以及如何解决僵局。

## 角色与职责

### 社区 (The Community)

`DeepLabCut` 社区包括任何以任何方式使用或参与该项目的人员。

### 贡献者 (Contributors)

社区成员可以通过与项目进行具体的直接互动，成为贡献者，例如：

- 通过 GitHub 的 [GitHub 发起请求 (pull request)](https://github.com/DeepLabCut/DeepLabCut/pulls) 提议对代码进行修改；
- 在我们的 [GitHub 问题页面 (issues page)](https://github.com/DeepLabCut/DeepLabCut/issues) 上报告问题；
- 通过 [GitHub 发起请求 (pull request)](https://github.com/DeepLabCut/DeepLabCut/pulls) 提议对 [文档](https://deeplabcut.github.io/DeepLabCut/README.html) 进行修改；
- 在现有的 [Issue](https://github.com/DeepLabCut/DeepLabCut/issues) 和 [Pull Request](https://github.com/DeepLabCut/DeepLabCut/pulls) 中讨论 `DeepLabCut` 的设计或其教程；
- 在带有 `#deeplabcut` 标签的 [image.sc 论坛](https://forum.image.sc/tags/DeepLabCut) 上讨论示例或用例；或者
- 审查 [待处理的 Pull Request](https://github.com/DeepLabCut/DeepLabCut/pulls) 等。

任何社区成员都可以成为贡献者，我们鼓励所有人都这样做。通过为项目做贡献，社区成员可以直接帮助塑造其未来。

我们鼓励贡献者阅读 [贡献指南 (contributing guide)](https://github.com/DeepLabCut/DeepLabCut/blob/main/CONTRIBUTING.md)。

### 核心开发者 (Core developers)

核心开发者是那些通过持续贡献向项目证明了其持续承诺的社区成员。他们已经证明了自己可以负责任地维护 `DeepLabCut`。成为核心开发者，允许贡献者合并已批准的 Pull Request，对 Pull Request 的合并投赞成票或反对票，并参与决定 API 的重大更改，从而更轻松地继续进行与项目相关的活动。核心开发者会出现在 `DeepLabCut` [GitHub 组织](https://github.com/orgs/DeepLabCut/people) 的组织成员列表中，并且属于我们的 [@deeplabcut/core-developers](https://github.com/orgs/DeepLabCut/teams/core-developers) GitHub 团队。核心开发者需要审查代码贡献，同时遵守核心开发者指南。新的核心开发者可以由任何现有的核心开发者提名，有关该流程的详细信息请参阅我们的核心开发者指南。

### 指导委员会 (Steering Council)

指导委员会 (SC) 的成员是具有额外职责以确保项目顺利运行的核心开发者。期望 SC 成员参与战略规划，批准治理模型的变更，并对授予项目的资金做出决定。（给予社区成员的资金由他们自行争取和管理）。SC 的目的是从大局角度确保项目顺利推进。影响整个项目的变更需要基于对项目和更广泛生态系统长期经验的分析。当核心开发者社区（包括 SC 成员）在合理的时间内未能达成此类共识时，SC 实体将解决该问题。

指导委员会的成员在 [DeepLabCut GitHub 组织](https://github.com/DeepLabCut/) 中还拥有 "所有者" 角色，并最终负责管理 DeepLabCut GitHub 账户、[@DeepLabCut](https://twitter.com/DeepLabCut) 推特账户、[DeepLabCut 网站](http://www.DeepLabCut.org) 以及其他类似的 DeepLabCut 所有资源。

DeepLabCut 当前的指导委员会由最初的开发者组成：

- [Mackenzie Mathis](https://github.com/mmathislab)
- [Alexander Mathis](https://github.com/alexemg)

SC 的成员资格每年一月重新审查。期望那些没有积极履行 SC 职责的成员辞职。新成员由核心开发者提名。被提名者应已证明对项目及其[使命和价值观](mission-and-values) 具有长期的持续承诺。提名将导致不超过一个月的讨论，之后通过共识接纳为 SC 成员。在此期间，SC 的僵局投票将被推迟，直到新成员加入并可以进行另一次投票。

DeepLabCut 指导委员会的联系邮箱是 `admin@deeplabcut.org`。

## 决策制定过程

关于项目未来的决策是通过与所有社区成员的讨论来制定的。所有非敏感的项目管理讨论都在 [Issue 跟踪器](https://github.com/deeplabcut/deeplabcut/issues) 上进行。偶尔，敏感的讨论可能会在私有的核心开发者媒介上进行。

决策应根据 DeepLabCut 项目的[使命和价值观](mission-and-values) 来制定。

DeepLabCut 采用“寻求共识”的过程来制定决策。该小组会尝试找到一个在核心开发者中没有公开反对意见的解决方案。核心开发者应区分对提案的基本反对意见和他们可以接受的次要缺陷，而不应因后者而阻碍决策过程。如果找不到一个没有反对意见的选项，决策将升级到 SC，SC 也会使用寻求共识的方式来达成决议。在极少数僵局仍然存在的情况下，如果该提案获得 SC 简单多数的支持，则可以推进。

除上述添加核心开发者和 SC 成员外，决策根据以下规则制定：

- **次要文档更改**，例如拼写错误修复，或句子添加/更正，需要核心开发者的批准 *并且* 核心开发者在 Issue 或 Pull Request 页面上没有提出异议或要求更改（**惰性共识**）。核心开发者应在对 Pull Request 发表意见后，给予“合理时间”让其他人就该 Pull Request 的最终状态发表意见，如果他们不确定其他人是否会同意。

- **代码更改和主要文档更改**需要 *一位* 核心开发者的同意 *并且* 核心开发者在 Issue 或 Pull Request 页面上没有提出异议或要求更改（**惰性共识**）。对于所有此类更改，核心开发者在批准后和合并前，应给予“合理时间”让其他人在 Pull Request 的最终状态下进行权衡。

- **API 原则的更改**需要在我们的 [Issue 跟踪器](https://github.com/DeepLabCut/DeepLabCut/issues) 上有一个专门的 Issue，并遵循上述概述的决策过程。

- **本治理模型或我们的使命、愿景和价值观的更改**需要在我们的 [Issue 跟踪器](https://github.com/DeepLabCut/DeepLabCut/issues) 上有一个专门的 Issue，并遵循上述概述的决策过程，*除非* 核心开发者对该更改达成一致同意，在这种情况下可以更快地推进。

如果在惰性共识中提出了反对意见，提案人可以向社区和核心开发者申诉，并且该更改可以通过升级到 SC 来批准或否决。