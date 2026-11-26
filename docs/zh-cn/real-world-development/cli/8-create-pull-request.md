---
title: "第 8 课 - 创建并合并功能 PR"
description: "一起审查筛选功能、指令、技能更新、QA 配置和测试，然后创建 PR 并使用 Agent Merge。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

筛选功能实现、指令更新、技能更新、质量保证（QA）配置和测试都已保存在同一个分支中。现在一起审查这些内容并创建拉取请求。之前你手动合并了星级评分 PR；这次将让 **Agent Merge** 管理合并流程。

> [!NOTE]
> 实际项目通常会将功能、指令更新、技能更新和 QA 代理拆分为几个独立的 PR。为简化本研讨会，你在同一对话和分支中完成了筛选功能及质量工作流，并将所有内容放入此 PR。

本课将学习如何：

- 了解 Agent Merge 及其如何自动化合并生命周期。
- 检查完整的功能变更和验证证据。
- 创建筛选功能 PR。
- 审查后才启用 Agent Merge，并确认 PR 已合并。

## 场景

在整个筛选功能工作流中，你使用 Copilot 规划、实现并验证了功能。现在 Tailspin Toys 希望自动执行剩余的 PR 工作，同时让开发者掌控合并授权。

## 认识 Agent Merge

**Agent Merge** 会自动处理合并拉取请求所需的剩余工作。启用后，Copilot 会处理阻塞 PR 的问题，例如修复持续集成 (CI) 检查失败、回应审查意见，以及在需要时执行 rebase；之后，如果存储库允许，它会启用 GitHub 自动合并。

此前你一直自行选择 **Merge pull request**。Agent Merge 可以承担这项工作，但启用前仍要先审查并确认变更已准备就绪。

## 使用 Agent Merge 管理 PR

代码已全部完成，现在一起审查、创建 PR，并让 Agent Merge 处理剩余流程。

1. 返回 Codespace。
2. 输入 `/agent` 打开代理对话框。
3. 在选项中选择 **Default**，然后按 <kbd>Enter</kbd>。
4. 使用命令 `/pr create` 创建新的 PR。
5. 使用 `/pr agentmerge` 启用 Agent Merge。
6. Copilot 会监控 PR 的持续集成流程，所有检查成功后便会合并 PR。
7. 确认 Copilot 显示类似“PR #14 was squash-merged successfully.”的消息。

> [!IMPORTANT]
> Agent Merge 不会绕过必需审批、分支保护、合并队列、存储库设置或权限限制。如果流程受阻，请查看报告的原因，并在存储库允许时手动完成已审查的合并。

## 总结和后续步骤

你已经自动化了开发流程的多个环节，包括生成代码、测试和验证代码，以及管理拉取请求。本课中，你：

- 了解 Agent Merge 如何自动化合并生命周期。
- 检查了完整的功能变更和验证证据。
- 创建了筛选功能 PR。
- 审查后启用 Agent Merge，并确认 PR 已合并。

接下来，[探索更多实用的 Copilot CLI 斜杠命令][next-lesson]，了解上下文、模型、共享和可选的云代理委派功能。

## 资源

- [使用 Copilot CLI 管理拉取请求][manage-prs]
- [Copilot CLI 命令参考][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
