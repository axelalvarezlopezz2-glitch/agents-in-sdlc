---
title: "练习 4 - 自定义代理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## 什么是自定义代理？

GitHub Copilot 中的[自定义代理][custom-agents-concept]是针对开发工作流中特定任务或领域的专用 AI 助手。通过在 `.github/agents` 文件夹中创建 Markdown 文件，可以提供专用指令、最佳实践、编码模式和领域知识。团队可以将专业知识整理成可复用代理，例如无障碍、安全或测试代理。

自定义代理定义在项目的 `.github/agents` 文件夹中，也可以全局放在 `~/.copilot/agents`。文件包含至少具有 `name` 和 `description` 的 YAML frontmatter，以及用于定义代理行为、专业知识和指令的 Markdown 提示。

### 自定义代理与代理技能

自定义代理和[代理技能][agent-skills-concept]都通过 Markdown 文件定义，并说明 AI 如何执行工作。最简单的区分方式是：**自定义代理是工作者，技能是工具**。代理有自己的上下文窗口，可协调技能或其他代理。本实验中的无障碍代理会检查并更新网站，也可以调用 PR 或测试技能。

> [!NOTE]
> 编写自定义代理没有唯一的正确方式。请针对实际环境和场景测试并迭代。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## 场景

Tailspin Toys 希望所有用户都能访问其众筹平台。用户反馈指出暗色主题的文本与背景对比度不足，因此设计团队要求添加可切换的高对比度模式。本课将使用自定义代理快速实现该功能。

本练习将学习如何审查现有无障碍自定义代理，并在 Copilot Chat 中使用它实现高对比度模式。

## 审查无障碍自定义代理

先返回 Codespace，为无障碍工作从 `main` 创建新分支（筛选 PR 保持独立）：

```bash
git checkout main
git pull
git checkout -b accessibility-vscode
```

1. 打开 `.github/agents/accessibility.md`。
2. 查看包含 `name` 和 `description` 字段的 YAML frontmatter。

> [!CAUTION]
> 自定义代理必须包含 `name` 和 `description` frontmatter。

3. 浏览其无障碍职责、最佳实践、HTML/CSS/JavaScript 示例以及常见问题。

## 在 Copilot Chat 中使用自定义代理

VS Code 会在 Copilot Chat 代理下拉菜单中显示 `.github/agents` 中定义的代理。选择代理后，当前聊天就会使用该代理的指令和工具。

> [!TIP]
> **打开 Copilot Chat**
>
> 返回 Codespace，打开 Copilot Chat 并选择 **New Chat**。各练习使用的模式和模型不同，具体步骤会在相应位置说明。

1. 在 Chat 视图的代理下拉菜单中选择 **Agent**。
2. 再次打开代理下拉菜单，并选择 **Accessibility agent**。
3. 发送以下提示：

    ```plaintext
    Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
    ```

4. Copilot 会探索代码库、提出修改并应用到项目文件。每次编辑都会在聊天中显示文件路径和差异供你审查。
5. 通过 **Files changed** 指示器查看代理修改的文件。

> [!NOTE]
> 这可能需要几分钟。Copilot 会修改 Astro 组件、CSS 和相关测试等文件。

下一课将审查并引导这项正在进行的工作。

## 总结和后续步骤

你探索了 GitHub Copilot 自定义代理，并使用它在 Copilot Chat 代理模式中实现高对比度模式。接下来[监控并引导代理工作][next-lesson]，审查变更并在同一会话中添加浅色模式切换。

## 资源

- [关于自定义代理][custom-agents]
- [在 IDE 中创建自定义代理][creating-custom-agents-ide]
- [VS Code 中的自定义代理][custom-agents-vscode]
- [自定义代理配置][custom-agents-config]
- [awesome-copilot 中的自定义代理][awesome-copilot-agents]

---

| [← 上一课：使用 Playwright MCP 服务器测试功能][previous-lesson] | [下一课：监控和管理代理 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
