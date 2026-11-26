---
title: "练习 5 - 监控和管理代理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

在代理模式下，GitHub Copilot 会自主探索代码库、提出修改、编辑文件并运行命令。VS Code 中的 Copilot Chat 会实时显示工具调用和文件编辑。你可以查看每项差异、接受或撤销单独修改，也可以通过后续提示引导代理完善或扩展工作。

本练习将学习如何实时监控无障碍代理、审查并接受文件修改，以及在会话中引导代理添加浅色模式。

## 场景

[练习 4][previous-lesson]中的无障碍自定义代理正在实现高对比度模式。设计团队又提出新要求：为觉得默认主题太暗的用户添加浅色模式。与其从头开始新对话，不如引导现有会话扩展工作。

## 监控 Copilot 进度

Copilot Chat 会显示代理探索的文件、运行的命令和提出的修改。每项修改都以差异形式显示，并提供 **Keep** 和 **Undo** 控件。

1. 返回练习 4 中的 Copilot Chat 视图。
2. 浏览聊天，查看工具调用、文件读取、编辑和终端命令。
3. 对每项文件修改选择 **Keep** 接受，或选择 **Undo** 撤销并让代理重试。
4. 选择聊天顶部的 **Files changed**，查看本次会话修改的所有文件。
5. 等待代理完成高对比度模式，并查看它的最终总结。

> [!NOTE]
> 如果工具调用请求权限（例如运行终端命令），代理会暂停并显示 **Allow** 提示。只批准你信任的命令。

## 引导代理添加浅色模式

高对比度模式完成后，在相同对话中添加浅色模式。代理会保留之前的上下文，包括涉及的组件、样式方法和本地存储模式。

1. 在 Copilot Chat 输入以下后续提示：

    ```plaintext
    Nice work! Now add a light-mode toggle alongside the high-contrast toggle. Light mode should follow the same pattern — persist via local storage and be accessible from the same area of the UI. The user should be able to enable high-contrast and light mode independently.
    ```

2. 代理会检查已有修改并提出浅色模式方案。
3. 像审查高对比度修改一样，逐项使用 **Keep** 或 **Undo** 审查新变更。
4. 如果方向不合适（例如尝试重构切换 UI），使用输入框旁的 **Stop** 按钮停止，再发送澄清提示。

> [!TIP]
> 引导进行中的会话通常比从头开始更快。可以要求调整（“使用 CSS 变量简化 CSS”）、纠正方向（“将浅色模式整合到现有设置面板，不要添加第三个开关”），或扩展范围（“为两个开关添加键盘快捷键”）。

## 审查工作集

提交前快速审查代理修改的内容：

1. 在 VS Code 中打开 **Source Control** 视图。
2. 审查所有变更文件，包括 Astro 组件、样式和相关测试。
3. 打开部分文件检查差异，确认 ARIA 属性、键盘导航、语义 HTML 和本地存储等无障碍实践。
4. 确认后，在 Source Control 面板中暂存并提交更改。下一课会发布这些变更。

## 总结和后续步骤

你使用 Copilot Chat 代理模式监控并引导进行中的会话，审查每项文件修改、接受或撤销变更，并在保留上下文的情况下通过后续提示扩展工作。

接下来[发布无障碍功能并审查本地自定义代理生成的变更][next-lesson]。

## 资源

- [VS Code 中的 Copilot Chat][copilot-chat-vscode]
- [使用代理模式][agent-mode]

---

| [← 上一课：自定义代理][previous-lesson] | [下一课：迭代 Copilot 的工作 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
