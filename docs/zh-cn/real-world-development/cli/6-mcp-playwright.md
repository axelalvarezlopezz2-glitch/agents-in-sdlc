---
title: "第 6 课 - 使用 Playwright MCP 验证功能"
description: "在 Copilot CLI 中检查或配置 Playwright MCP，并使用它在浏览器中体验筛选功能。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

正如前面所说，编写代码不只是写代码。我们还要处理数据、外部服务，并允许 Copilot 使用更多自动化工具。MCP 服务器可以让 Copilot CLI 使用内置功能之外的工具和服务。

本课将学习如何：

- 了解 Model Context Protocol (MCP) 及 Copilot CLI 对它的使用方式。
- 如果尚未配置，则添加 Playwright MCP 服务器。
- 让代理驱动浏览器并体验筛选功能。

## 场景

单元测试和端到端测试很重要，但要验证 UI 更新，仍需实际与 UI 交互。现在允许 Copilot 像用户一样使用网站，进一步自动化变更验证，提升对功能的信心。

## 什么是 Model Context Protocol (MCP)？

[Model Context Protocol (MCP)][mcp-blog-post] 为 AI 代理提供了与外部工具和服务通信的方式。通过 MCP，代理可以实时访问最新信息，并代表你执行操作。

MCP 服务器充当 AI 代理与外部工具及服务之间的桥梁，每个服务器都提供一组可供代理使用的工具和资源。常见的服务器包括：

- **[GitHub MCP Server][github-mcp]**：提供管理 GitHub 存储库、issue 和拉取请求的 API。
- **[Playwright MCP Server][playwright-mcp-server]**：使用 Playwright 提供浏览器自动化能力。

GitHub 还提供 [MCP 注册表][mcp-registry]，帮助发现并贡献其他 MCP 服务器。

> [!CAUTION]
> 将 MCP 服务器视为项目依赖项。使用前检查源代码、确认发布者，并考虑安全影响。

## 添加 Playwright MCP 服务器

如果 Copilot CLI 尚未配置 Playwright MCP 服务器，请按以下步骤添加。

> [!NOTE]
> 如果 Copilot CLI 中已经配置了 Playwright MCP，可跳过设置步骤并继续下一节。

1. 返回 Codespace。
2. 在 Copilot CLI 中输入以下命令，打开添加 MCP 服务器对话框：

   ```plaintext
   /mcp add
   ```

3. 名称输入 `playwright`，然后按 <kbd>Tab</kbd>。
4. 按 <kbd>Enter</kbd> 确认服务器类型为 **STDIO**，然后按 <kbd>Tab</kbd>。
5. 在 **Command** 对话框中粘贴以下命令：

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. 按 <kbd>Control</kbd>+<kbd>S</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>S</kbd>（Windows/Linux）保存新的 MCP 服务器。
7. 按 <kbd>Esc</kbd> 退出 MCP 对话框。

## 让 Copilot 使用 Playwright 体验功能

你已经手动确认功能符合预期。现在使用 Copilot 中的 Playwright MCP 服务器执行同样的操作。

1. 使用以下提示，让 Copilot 调用 Playwright MCP 服务器验证功能：

   ```plaintext
   Start the app and use Playwright MCP to check filtering against the issue and our plan. Tell me what works and what doesn't, without making changes. Stop the server you started when you're done.
   ```

> [!NOTE]
> Copilot 通常会自动找到 MCP 服务器，不一定需要明确指出。但说明要使用哪个服务器有助于获得更一致的结果，也能节省一些 token。

2. 观察 Copilot 在浏览器中执行的步骤。
3. 阅读报告，确认功能行为符合预期。

Copilot 会启动服务器、使用 Playwright 操作网站、停止服务器并提供报告。

## 总结和后续步骤

恭喜，你已通过 Copilot CLI 中的 Playwright MCP 服务器，在真实浏览器中体验功能。本课中，你：

- 了解了 Model Context Protocol (MCP) 及 Copilot CLI 对它的使用方式。
- 添加了 Playwright MCP 服务器（如果之前尚未配置）。
- 让代理驱动浏览器并体验筛选功能。

接下来，[创建 QA 自定义代理][next-lesson]，将技能与浏览器工具整合到专用角色中。

## 资源

- [MCP 是什么，为什么人人都在谈论它？][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [将 MCP 服务器添加到 Copilot CLI][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers
