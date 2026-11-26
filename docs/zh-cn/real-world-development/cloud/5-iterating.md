---
title: "练习 5：迭代 GitHub Copilot 的工作"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：管理代理][previous-lesson] |
|:--|

## 审查工作

在整个实验中，你与 GitHub Copilot 一起处理了多个改善用户体验和添加功能的任务：为代码添加文档、实现供设计团队迭代的相关游戏功能，以及添加高对比度和浅色模式切换等无障碍功能。现在检查代码变更，必要时向 Copilot 提供反馈，然后再为团队启用审查。

### 场景

生成式 AI 不会改变软件设计和 DevOps 的基本原则。我们仍需要审查生成的代码并遵循正常的 DevOps 流程。接下来审查 GitHub Copilot 创建的文档、相关游戏功能和无障碍功能，然后再启用团队审查。

## 安全和 GitHub Copilot 云代理

Copilot 云代理会异步且自主地执行任务，因此会受到一些安全限制：

- Copilot 只能读取存储库，并且只能写入它使用的代码分支。
- 云代理在 GitHub Actions 中运行，并在独立的临时环境中工作。
- GitHub Actions 工作流必须经过人工批准后才能运行。
- [默认情况下，外部资源访问受到限制](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall)，包括 MCP 服务器。

## 审查生成的文档

先通过 GitHub.com 上的常规 PR 界面，查看 GitHub Copilot 云代理创建的第一个拉取请求（PR）：为代码添加文档。

> [!NOTE]
> 查看 PR 时，可能会看到 GitHub Copilot 被防火墙阻止的警告。这是预期行为，因为默认情况下 Copilot 只能有限访问外部资源，包括外部 MCP 服务器。你可以[自定义或禁用云代理防火墙][agent-firewall]。

1. 返回 github.com 上的存储库。
2. 选择 **Pull Requests** 选项卡。
3. 打开标题类似 **Add missing documentation** 的 PR。

> [!NOTE]
> 如果 Copilot 仍在处理任务，PR 会包含 **[WIP]** 标记。请等待任务完成，这可能需要几分钟。

4. PR 准备就绪后，选择 **Files changed** 选项卡并审查变更。
5. 查看新增的 TSDoc 文档注释和其他文档。实际变更会有所不同；留意 TSDoc、TypeScript 约定和注释头，它们来自练习 2 中查看的自定义指令。
6. 审查完成后，返回 **Conversation** 选项卡并向下滚动。
7. 查看是否有等待批准的工作流。
8. 如有，选择 **Approve and run workflows**。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

工作流会在 PR 的检查区域排队运行。检查应针对单体 Astro 应用通过，可能需要几分钟。

## 请求 GitHub Copilot 修改

可以在 PR 评论或代码行内评论中提及 Copilot，让它启动新会话处理反馈。由于结果具有非确定性，无法指定固定文本。可以要求 Copilot：

- 为每个代码文件添加简短的文件注释头。
- 为 TypeScript 和 Astro 文件添加 TSDoc 文档注释。
- 创建 README，介绍 Astro 应用结构。

1. 添加评论并像提及其他用户一样标记 **@copilot**，提出文档方面的修改要求。
2. 选择 **View Session**，观察 Copilot 启动新会话并执行更新。
3. 选择 **Back to pull request** 返回 PR。
4. Copilot 完成修改后，PR 中会显示新提交。
5. 选择 **Files changed** 选项卡并审查修改。可继续迭代；满意后将 PR 从草稿转换为可审查状态，再合并到 main 分支。

![将 PR 转为可审查状态](../../../_images/ex7-ready-for-review.png)

## 审查相关游戏功能

1. 返回 GitHub.com 上的存储库并选择 **Pull Requests**。
2. 选择标题类似 **Show related games on the game details page** 的 PR。
3. 在 **Files changed** 选项卡中审查生成的代码。
4. 确认变更合理后，返回 **Conversation** 选项卡并向下滚动。
5. 如有工作流等待批准，选择 **Approve and run workflows**。
6. 检查 PR 中排队运行的工作流。针对单体 Astro 应用的项目检查应通过。
7. **可选：**在 Codespace 中手动测试相关游戏功能。将 `<branch-name>` 替换为 Copilot 创建的分支名称（例如 **copilot/fix-8**）：

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

如需进一步修改，例如调整相关游戏数量，或添加文件注释头和 TSDoc 文档注释（该 issue 在自定义指令更新前分配），可在 **Conversation** 选项卡中添加评论，Copilot 会启动新会话。

## 审查无障碍功能

最后审查无障碍自定义代理实现的功能。PR 应同时包含练习 3 中要求的高对比度模式，以及练习 4 中在 Mission Control 请求的浅色模式。

1. 返回 GitHub.com 上的存储库，选择 **Pull Requests**。
2. 选择标题类似 **Add high contrast mode to website** 的 PR。
3. 如果 PR 仍带有 **[WIP]** 标记，请等待 Copilot 完成。
4. 在 **Files changed** 选项卡中审查代码，重点检查模式切换 UI、使用本地存储保留用户偏好、样式修改、ARIA 属性和键盘导航，以及管理模式切换的 JavaScript/TypeScript 代码。
5. 审查完成后，返回 **Conversation** 选项卡。
6. 如果有工作流等待批准，选择 **Approve and run workflows**。
7. 等待 PR 的项目检查通过。
8. **可选：**在 Codespace 中检出代理分支并手动测试无障碍功能：

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

   启动应用，在浏览器中测试高对比度和浅色模式切换，并确认页面重新加载后设置仍保留。

无障碍代理帮助 Copilot 遵循无障碍最佳实践实现功能。发现问题或改进点时，可以像前面一样在评论中提及 **@copilot**，要求它更新。

## 可选练习 — 继续委派

云代理适合处理实际待办事项，让你专注于其他工作。可以在存储库中创建更多 issue 并分配给 Copilot，例如：

- 在游戏详情页创建支持者兴趣表单。
- 为游戏列表页添加分页。
- 为数据访问辅助函数添加输入验证和错误处理。

## 总结

你已完成云代理环境中的全部练习。过程中你：

- **检查了存储库自带的自定义指令文件**，了解它们如何影响云代理的输出。
- **将 issue 分配给 Copilot 云代理**，并观察它异步设置环境、制定计划和执行任务。
- **创建并使用自定义代理**实现无障碍功能，添加高对比度和浅色模式切换。
- **使用 Copilot agents 页面作为 Mission Control**，监控并引导无障碍会话。
- **审查并迭代云代理的拉取请求**，通过 `@copilot` 请求修改并批准工作流。

## 回顾和后续步骤

你已完成云代理练习。其他环境可以补充本次实践：

- 🖥️ **[VS Code 环境](../../vscode/)** — 在 IDE 中体验 Copilot Chat 代理模式和 MCP 集成。
- 💻 **[CLI 环境](../../cli/)** — 从终端交付两个经过审查的变更，并使用 Plan 和 Autopilot 模式、代理技能、自定义 QA 代理、Playwright MCP、Agent Merge，以及可选的 `/delegate` 云代理桥接。

还可以在自己的存储库中尝试以下后续任务：

- 将重构或测试覆盖率 issue 分配给云代理。
- 为其他领域创建自定义代理。
- 为不同技术栈设置 `copilot-setup-steps.yml`。

## 资源

- [GitHub Copilot][github-copilot]
- [关于 Copilot 代理][copilot-agents]
- [将 GitHub issue 分配给 Copilot][assign-issue]
- [Copilot 云代理设置工作流最佳实践][cloud-agent-best-practices]
- [配置 Copilot 云代理防火墙][agent-firewall]

---

| [← 上一课：管理代理][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall
[previous-lesson]: ../4-managing-agents/
