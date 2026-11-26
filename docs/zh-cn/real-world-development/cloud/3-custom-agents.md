---
title: "练习 3 - 自定义代理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## 什么是自定义代理？

GitHub Copilot 中的[自定义代理][custom-agents-concept]是面向开发工作流中特定任务或领域的专用 AI 助手。通过在存储库的 `.github/agents` 文件夹中创建 Markdown 文件，可以提供聚焦的指令、最佳实践、编码模式和领域知识，帮助 Copilot 更有效地完成工作。团队可以将专业知识整理成可复用代理，例如检查 [WCAG][wcag] 合规性的无障碍代理、遵循安全编码实践的安全代理，或保持测试模式一致的测试代理。

自定义代理通过项目 `.github/agents` 文件夹中的 Markdown 文件定义，也可以全局放在 `~/.copilot/agents` 中。每个文件都包含至少具有 `name` 和 `description` 的 YAML frontmatter，以及用于说明代理行为、专长和指令的 Markdown 提示。

### 自定义代理与代理技能

自定义代理和[代理技能][agent-skills-concept]有些相似：两者主要通过 Markdown 文件定义，并告诉 AI 如何执行操作。最简单的区分方式是：**自定义代理是工作者，技能是工具**。

自定义代理拥有独立的上下文窗口，可协调技能甚至其他代理完成任务。本实验中的无障碍自定义代理会依据无障碍指南审查并更新网站；它可以调用拉取请求工作流技能，或运行和管理测试的技能。

> [!NOTE]
> 编写自定义代理没有唯一的正确方式。与 AI 的其他用途一样，需要通过测试和迭代找到适合环境和场景的方法。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

本课将了解：

- 如何定义自定义代理。
- 如何将任务分配给自定义代理。

## 场景

Tailspin Toys 致力于让所有用户都能使用其众筹平台，无论用户的视觉能力或偏好如何。近期反馈显示，一些用户觉得当前暗色主题难以阅读，因为文本与背景之间的对比度不足。设计团队因此要求实现可切换的高对比度模式。

无障碍访问至关重要，因此需要尽快实现。你将使用自定义代理生成此功能。

## 审查无障碍自定义代理

项目已为无障碍访问创建了自定义代理。返回 Codespace，打开代理文件并了解它如何指导 Copilot。

1. 打开 `.github/agents/accessibility.md`。
2. 查看包含 `name` 和 `description` 字段的 YAML frontmatter。

> [!CAUTION]
> 自定义代理必须包含 `name` 和 `description` frontmatter。

3. 浏览后续部分，了解生成无障碍网站代码时的核心职责、最佳实践、HTML/CSS/JavaScript 示例，以及常见问题。

## 创建并分配 issue

Mission control 是管理环境中代理的中心位置。可以在其中将任务分配给 Copilot 云代理、监控任务并提供进一步指导。现在创建 issue，让 Copilot 实现高对比度模式。

1. 打开存储库。
2. 选择 **Issues** 选项卡，再选择 **New issue**。
3. 选择 **Blank issue**。
4. 将标题设为 `Add high contrast mode to website`。
5. 将说明设为：

    ```plaintext
    We need a high contrast mode for the site. There should be a toggle for high contrast which the user can set. It should store the setting in local storage on the browser.
    ```

6. 选择 **Create** 创建 issue。
7. 在右侧选择 **Assign to Copilot**，再从自定义代理列表中选择 **Accessibility agent**。
8. 选择 **Assign**。

    ![分配云代理时，突出显示自定义代理和无障碍代理](../../../_images/ex5-select-custom-agent.png)

Copilot 会在后台开始处理任务。

## 总结和后续步骤

本课介绍了 [GitHub Copilot 自定义代理][custom-agents]，也就是针对特定任务和领域的专用 AI 助手。自定义代理可以将团队专业知识和标准整理成可复用配置，指导 Copilot 更有效地完成工作。

你学习了如何定义自定义代理，以及如何分配任务。接下来从 Mission Control [监控并引导代理会话][next-lesson]。

## 资源

- [关于自定义代理][custom-agents]
- [在组织中使用自定义代理的准备工作][org-custom-agents]
- [在企业中使用自定义代理的准备工作][enterprise-custom-agents]

---

| [← 上一课：GitHub Copilot 云代理][previous-lesson] | [下一课：监控和管理代理 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
