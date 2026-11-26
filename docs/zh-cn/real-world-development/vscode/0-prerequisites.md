---
title: "练习 0：先决条件"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

开始 VS Code 练习前，需要先做好准备。你将创建 Tailspin Toys 存储库的副本，启动 [Codespace][codespaces]，并确认编辑器中的 GitHub Copilot Chat 已启动。

## 设置实验室存储库

从[模板][template-repository]创建存储库副本，新存储库包含实验所需文件。

1. 在新浏览器窗口中打开 [Tailspin Toys 模板][tailspin-template]。
2. 选择 **Use this template**，再选择 **Create a new repository**。

    ![Use this template 按钮](../../../_images/ex0-use-template.png)

3. 如果正在参加 GitHub 或 Microsoft 主办的活动，请遵循导师的说明。否则，在你有权使用 GitHub Copilot 的组织中创建存储库。

    ![输入存储库模板设置](../../../_images/ex0-repository-settings.png)

4. 记下存储库路径（**organization-or-user-name/repository-name**）。

> [!NOTE]
> **待办列表已准备就绪**
>
> 从模板创建存储库时，系统会自动创建一组 GitHub issue。整个研讨会都会使用这些 issue，无需自行创建。

## 创建 Codespace

Codespace 是基于云的开发环境，可让你直接在浏览器中编写、运行和调试代码。

1. 打开刚创建的存储库。
2. 选择绿色的 **Code** 按钮。

    ![选择 Code 按钮](../../../_images/ex0-code-button.png)

3. 选择 **Codespaces** 选项卡，再选择 **+** 创建 Codespace。

    ![创建新的 Codespace](../../../_images/ex0-create-codespace.png)

创建 Codespace 需要几分钟。等待时可以了解 GitHub Copilot 的其他功能。

> [!CAUTION]
> 后续练习会继续使用此 Codespace。现在先让它在浏览器选项卡中保持打开状态。

> [!NOTE]
> 本研讨会可在 Codespace 或本地 [dev container][dev-containers] 中运行。如果想在本地运行，请在 VS Code 中打开克隆的存储库，并在出现提示时选择 **Reopen in Container**。VS Code 将构建与 Codespace 相同的开发容器。

[codespaces]: https://github.com/features/codespaces
[dev-containers]: https://code.visualstudio.com/docs/devcontainers/containers

## 使用 GitHub Copilot Chat 和代理模式

需要在 IDE 中安装 GitHub Copilot Chat 扩展才能使用代理模式。使用 GitHub Codespace 时通常已安装该扩展。

> [!TIP]
> 如果尚未安装 GitHub Copilot Chat 扩展，可从 [Visual Studio Code Marketplace][copilot-chat-extension] 安装，也可以在 VS Code 的扩展视图中搜索 **GitHub Copilot Chat** 并选择 **Install**。

1. 返回 Codespace。
2. 如果编辑器右侧没有 Copilot Chat，请选择 Codespace 顶部的 **Copilot Chat** 图标。
3. 在 Copilot Chat 中输入类似“Hello world”的消息并按 Enter。
4. 如果尚未登录，请按照提示使用 GitHub 帐户进行身份验证。
5. 身份验证后，Copilot Chat 窗口会显示出来。

    ![Copilot Chat 身份验证提示示例](../../../_images/ex1-copilot-authentication.png)

## 总结

你已创建实验室存储库副本，并开始创建之后编写代码时要使用的 Codespace。

## 下一步

开始使用 Copilot。继续学习[练习 1 - 自定义指令][next-lesson]，了解项目约定。

## 资源

- [GitHub Codespaces 概述][codespaces]
- [从模板创建存储库][template-repository]
- [开始使用 Codespaces][codespaces-quickstart]

[tailspin-template]: https://github.com/github-samples/tailspin-toys
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[codespaces-quickstart]: https://docs.github.com/codespaces/getting-started/quickstart
[copilot-chat-extension]: https://marketplace.visualstudio.com/items?itemName=GitHub.copilot-chat
[next-lesson]: ../1-custom-instructions/
