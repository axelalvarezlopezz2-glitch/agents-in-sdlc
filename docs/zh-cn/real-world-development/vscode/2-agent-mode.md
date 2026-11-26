---
title: "练习 2 - 使用 Copilot 代理模式添加新功能"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

即使是简单的应用更新，通常也涉及多个文件以及运行测试等操作。开发者需要找到相关文件、修改代码、运行测试、调试并反复修正。Copilot 代理模式可以在 IDE 中更自主地处理这些工作：探索项目、协调修改、运行测试并修正发现的问题。

> [!NOTE]
> 代理模式和云代理虽然名称相似，但适用于不同场景。代理模式在 IDE 中运行，便于快速反馈和交互；云代理则像团队成员一样，通过 issue 和拉取请求异步工作。

本练习将学习 Copilot 代理模式如何探索项目、识别文件、跨 UI 和数据层实现功能，以及如何审查生成的变更和测试。

## 场景

游戏列表不断增长，需要允许用户按类别和发行方筛选。上一课已添加发行方辅助函数，现在将使用 Copilot 代理模式完成其余的数据层、UI 和测试工作。

## 运行 Tailspin Toys 网站

Tailspin Toys 是带有开发者主题的桌游众筹网站，可显示游戏列表和详情。网站是单体 Astro 应用，会在构建时生成静态 HTML。页面通过 Drizzle ORM 在 frontmatter 中直接查询本地 SQLite 数据库，没有单独的后端 API 或客户端 UI 框架。可复用的数据访问辅助函数放在 `src/lib/`，交互功能由使用标准 DOM API 的 Astro `<script>` 实现。

### 启动网站

1. 返回 Codespace，确认上一课后已回到 `main`。
2. 打开终端并创建筛选功能分支：

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

3. 启动网站：

   ```bash
   npm run dev
   ```

4. 等待 Astro 开发服务器就绪，然后按住 <kbd>Command</kbd>（Mac）或 <kbd>Ctrl</kbd>（Windows/Linux），选择 `http://localhost:4321`。

> [!NOTE]
> 在 Codespace 中打开 localhost 链接时，会自动转到 `https://<your-codespace-name>-4321.app.github.dev/`，这是指向 Codespace 的私有隧道。

网站主页显示游戏列表；选择游戏后可在详情页查看标题、简介、发行方和类别。

## 使用 Copilot 浏览待办事项

> [!TIP]
> **打开 Copilot Chat**
>
> 返回 Codespace，打开 Copilot Chat 并选择 **New Chat**，开始新的对话。模式和模型选择因练习而异。

1. 在 Chat 视图中选择 **Agent**，让代理自主规划并跨文件修改、运行命令和调用工具。
2. 选择 **Claude Sonnet 4.5** 模型。

> [!CAUTION]
> 本研讨会使用 Claude Sonnet 4.5 是为了让代码建议相对一致，并不代表作者偏好该模型。LLM 具有概率性，实际建议可能不同，这是正常现象。

> [!NOTE]
> Copilot 可能使用不同的 MCP 命令，但仍应能完成任务。

3. 发送提示：

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. 选择 **Continue** 运行列出 issue 的命令，然后审查结果及 Copilot 提出的优先级。

## 审查指令文件

开始生成代码前，先查看用于提供上下文的指令文件。项目中的 [UI 指令](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md)介绍了如何为网站添加功能。

1. 在 Codespace 中打开 `.github/instructions/ui.instructions.md`。
2. 查看架构、组件设计、可测试性、无障碍访问，以及指向 Astro、Tailwind CSS 和 Drizzle 指令的链接。

> [!TIP]
> 指令文件可引用其他指令文件和项目文件，路径相对于指令文件本身。这样可以拆分较大的说明，并复用示例和模板。

## 实现筛选功能

需要完成的工作至少包括：更新 `src/lib/` 中的数据层筛选逻辑、添加或更新筛选测试，以及更新游戏列表页的筛选 UI。测试必须在合并前通过。

1. 继续当前 Copilot 对话，或选择 **New Chat**。
2. 选择 **Add Context**、**Instructions**，然后选择 `ui` 指令文件。
3. 确认选择了 **Agent** 和 **Claude Sonnet 4.5**。
4. 发送以下提示：

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

5. 观察 Copilot 探索项目、找到数据层、UI 和测试文件，并修改代码、运行测试。
6. 按提示选择 **Continue** 运行测试。过程中测试可能暂时失败，Copilot 会继续迭代，直到不再发现错误。
7. 检查生成的代码，回到浏览器试用新功能。确认工作正常并审查代码后，在 Copilot Chat 中选择 **Keep**。

> [!CAUTION]
> 始终审查 Copilot 或其他 AI 工具生成的代码。

## 总结和后续步骤

你使用 GitHub Copilot 代理模式为 Tailspin Toys 添加了功能，并学习了它如何跨 UI 和数据层协调修改、探索项目，以及在合并前审查生成的变更和测试。

接下来使用 [Playwright MCP 服务器测试功能][next-lesson]，并创建拉取请求。

### 额外探索练习 – 实现分页

游戏列表增长后可能需要分页。使用本练习学到的技能，让 Copilot 添加分页，并考虑是否允许用户选择每页大小、如何编写提示以及是否需要多次迭代。

## 资源

- [Copilot ask、edit 和 agent 模式：功能及适用场景][choose-mode]
- [VS Code 中的代理模式][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
