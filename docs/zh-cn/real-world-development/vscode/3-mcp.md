---
title: "练习 3 - 使用 Playwright MCP 服务器测试功能"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：代理模式][previous-lesson] · [下一课：自定义代理 →][next-lesson]

你刚刚使用代理模式构建了筛选功能。创建拉取请求前，应先在浏览器中确认功能正常。本练习将连接 **Playwright MCP 服务器**，让 Copilot 驱动真实浏览器测试功能，然后发布分支并创建 PR。

本练习将学习 MCP 的作用、如何将 Playwright MCP 添加到工作区、如何让 Copilot 在浏览器中手动测试筛选功能，以及如何发布分支并创建拉取请求。

## 什么是 Model Context Protocol (MCP)？

当代理模式可访问编辑器外部资源时，会更加强大。Model Context Protocol (MCP) 是 Copilot 与外部工具和服务通信的标准方式。

![代理模式及其与上下文、LLM 和工具的交互示意图，包括 MCP 服务器和 VS Code 扩展提供的工具](../../../_images/ex1-mcp-diagram.png)

[Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) 让 AI 代理可以访问外部资源、获取最新信息并代表你执行操作。MCP 服务器充当代理与外部工具之间的桥梁，每个服务器提供不同的工具和资源，例如 [GitHub MCP Server](https://github.com/github/github-mcp-server) 和 [Playwright MCP Server](https://github.com/microsoft/playwright-mcp)。GitHub 还维护 [MCP 注册表](https://github.com/mcp)，帮助发现和贡献服务器。

> [!CAUTION]
> 将 MCP 服务器视为项目依赖项。使用前检查源代码、确认发布者并评估安全影响；只使用可信服务器，谨慎授予敏感资源或操作的访问权限。

## 检查 MCP 配置

`.vscode/mcp.json` 配置此 VS Code 工作区可用的 MCP 服务器。

1. 在 Codespace 中打开 `.vscode/mcp.json`。
2. 项目模板已配置 `github` 服务器：

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        }
      }
    }
    ```

它使用远程 GitHub MCP 服务器，因此无需本地安装。VS Code 通过 HTTP 连接，并在 Copilot 首次使用工具时要求你登录 GitHub。

## 添加 Playwright MCP 服务器

1. 在 `.vscode/mcp.json` 中，在 `github` 旁添加 `playwright`：

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        },
        "playwright": {
          "command": "npx",
          "args": ["@playwright/mcp@latest", "--headless"]
        }
      }
    }
    ```

2. 保存文件。Playwright 是本地服务器，VS Code 会运行 `npx @playwright/mcp@latest`；`--headless` 让浏览器不显示窗口，适用于没有桌面环境的 Codespace。

> [!NOTE]
> 项目已使用 Playwright 进行端到端测试，通常已安装浏览器。如果 Copilot 报告缺少浏览器，请运行 `npx playwright install chromium` 后重试。

## 启动并信任服务器

VS Code 会按需启动 MCP 服务器，并要求你确认是否信任服务器。

1. 在 `.vscode/mcp.json` 中，选择 `playwright` 项上方的 **Start** 操作，确认配置正确。
2. 如果 VS Code 询问是否信任服务器，请检查配置并选择信任。

手动启动是可选的；若跳过，VS Code 会在后续 Copilot 首次需要工具时启动服务器并询问是否信任。

## 测试筛选功能

连接服务器并启动应用后，使用以下提示让 Copilot 测试筛选功能：

```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

Copilot 会通过 Playwright MCP 启动浏览器、逐项测试并报告结果。对照 issue 的验收要求检查报告；发现问题时，可继续提问或让 Copilot 修复代码。

> [!NOTE]
> 测试时应用必须运行在 `http://localhost:4321`。如果开发服务器已停止，请先重新启动。首次使用 Playwright MCP 时可能需要下载浏览器；若提示缺少浏览器，运行 `npx playwright install chromium` 后重试。

Playwright MCP 会读取页面的[无障碍树][playwright-mcp-server]，而不是截图，因此代理会使用带标签的结构化元素进行推理。这也可以作为简单的无障碍检查。

## 发布分支并创建拉取请求

1. 在 Codespace 中打开 **Source Control** 面板并审查 Copilot 所做的变更。
2. 选择 **+** 暂存变更。
3. 使用 **Sparkle** 按钮生成提交消息。
4. 选择 **Publish** 推送分支。

接着让 Copilot 创建 PR：

1. 打开 Copilot Chat 并选择 **New Chat**。
2. 确认选择 **Agent**，以便 Copilot 使用 GitHub 工具。
3. 发送提示：

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. 按需选择 **Continue**，允许 Copilot 获取信息并执行操作。首次使用 GitHub 工具时可能需要登录。
5. 选择 Copilot 返回的链接审查 PR，但暂时不要合并。

## 总结和后续步骤

你了解了 MCP，并在工作区添加 Playwright MCP 服务器，通过浏览器手动测试筛选功能，发布分支并创建了 PR。接下来[创建自定义代理][next-lesson]，简化无障碍审查等专门任务。

## 资源

- [MCP 是什么，为什么人人都在谈论它？][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [GitHub MCP Server][github-mcp-server]
- [GitHub MCP 注册表][mcp-registry]
- [VS Code 中的 MCP 服务器][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
