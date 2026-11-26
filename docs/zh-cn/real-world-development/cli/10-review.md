---
title: "第 10 课 - 总结和后续步骤"
description: "回顾 Copilot CLI 工作流、两个拉取请求、可复用的自定义项和更多资源。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

你使用 GitHub Copilot CLI 完成了 Tailspin Toys 的连续工作流。你：

- 准备了 Codespace、安装 Copilot CLI、探索项目并找到预置的筛选 issue。
- 添加星级评分，在转发浏览器中检查结果，并手动合并第一个拉取请求（PR）。
- 从筛选 issue 开始，在 Plan 模式中确定方案、通过 Autopilot 模式实现，并在 Interactive 模式中审查。
- 使用自定义指令引导代理、自定义现有 `quality-checks` 技能，并运行单元测试、lint 和类型检查。
- 添加 Playwright Model Context Protocol (MCP) 服务器，并用它在真实浏览器中体验筛选功能。
- 创建并选择质量保证（QA）自定义代理，检查需求、测试覆盖率、技能结果和浏览器证据。
- 审查完整的筛选变更，并授权 Agent Merge 合并筛选 PR。
- 探索用于上下文、模型、共享和可选云代理委派的斜杠命令。

## 已完成的工作

本研讨会有两个 PR 阶段，每个阶段都基于更新后的 `main`，并在各自分支上完成：

1. **星级评分：**在游戏卡片上显示现有 `starRating`，并明确显示未评分状态。
2. **筛选和质量工作流：**实现筛选、更新指令并将其应用到功能、自定义 `quality-checks` 报告、创建 QA 配置，并添加相关测试。

从规划筛选功能到创建 PR，你一直使用同一个对话和分支。为了简化研讨会，这些工作合并到了一个 PR 中。

## 不同类型的验证

你通过多种方式检查代码：自动化测试、亲自使用浏览器检查，以及通过 MCP 让 Copilot 在浏览器中验证。`quality-checks` 技能运行单元测试、lint 和类型检查，并按新格式报告结果。QA 汇总这些结果，并在 PR 创建前检查需求和测试覆盖率。

新增测试应解决实际缺口；QA 运行后不需要添加测试也可能是正确结果。授权合并前审查代码和证据，并在变更后更新受影响的证据。

## 最佳实践

提供给 Copilot 的上下文和工具会影响它的工作。在本研讨会中，你更新了指令、自定义技能、创建 QA 配置并配置 MCP 服务器。可以在不同对话中复用这些自定义项，并根据团队需求调整。指令定义标准，技能描述可重复任务，自定义代理定义专用角色，MCP 服务器连接外部工具。请审查实际变更和工具结果，而不只是代理总结。

根据任务选择合适的**模式和模型**。先用 **Plan** 规划方案；对范围明确的修改用 **Interactive** 保持协作；对边界清晰的任务可用 **Autopilot**。常规编辑可选择较快的模型，复杂任务则选择能力更强的模型。

上下文仍与基础设施同样重要。清晰说明要构建什么、为什么构建以及如何构建，会显著影响输出。

## 继续探索

核心工作流已经完成。还可以了解以下 CLI 功能：

- `/review`：让代码审查代理分析变更。
- `/rubber-duck`：讨论问题并获得不同视角。
- `/fleet`：并行协调相互独立的子任务。
- `/worktree`：隔离单独任务。
- `/delegate`：将任务发送给 Copilot 云代理。

## 后续步骤

提升工具使用能力的最佳方式就是持续实践！将它用于生产代码、兴趣项目，或一直想做却尚未着手的小应用。与团队分享经验，也向团队学习。最后，继续浏览文档。

若要从终端扩展 Tailspin Toys，可继续可选的 [Foundry Backer Concierge 系列][foundry]。若想比较不同环境，可以探索 [VS Code 研讨会][vscode]、[GitHub Copilot 应用研讨会][app]或 [Copilot 云代理研讨会][cloud]。

## 资源

- [关于 GitHub Copilot CLI][about-cli]
- [Copilot CLI 命令参考][cli-reference]
- [自定义 Copilot CLI][customize-cli]
- [使用 Copilot CLI 管理拉取请求][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
