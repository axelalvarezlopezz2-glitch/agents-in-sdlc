---
title: "练习 1 - 自定义指令（云代理）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：先决条件][previous-lesson] · [下一课：GitHub Copilot 云代理 →][next-lesson]

使用生成式 AI 时，上下文至关重要。如果任务需要以特定方式完成，或者 Copilot 需要了解一些背景信息，就要确保它能够获取这些上下文。[指令文件][instruction-files]可提供相关指导，帮助 Copilot 理解你希望完成什么，以及应该如何完成。

本练习将学习如何：

- 通过存储库自定义指令和按路径限定的指令文件，让 Copilot 获取项目上下文、编码指南和文档标准。
- 在 `.github/copilot-instructions.md` 中添加新的全局存储库标准。

> [!NOTE]
> 与 VS Code 和 CLI 环境不同，本练习不会运行前后对比提示。Copilot 云代理会异步处理 GitHub issue，因此难以实时并排展示指令的影响。之后审查云代理创建的拉取请求时，你会看到这些指令在本环境中的实际效果。

## 指令文件

### 场景

Tailspin Toys 有一套开发实践指南和要求，包括：

- 数据层始终需要单元测试。
- UI 应采用暗色模式并具有现代感。
- 应使用 TSDoc 文档注释为代码添加说明。
- 每个文件开头都应添加一段注释，说明文件的用途。

通过指令文件，可以让 Copilot 获取这些要求，并按团队的实践完成任务。

### 自定义指令

自定义指令可以向 Copilot 提供上下文和偏好设置，帮助它理解你的编码风格和要求。你可以通过它引导 Copilot 提供更符合需求的建议和代码片段，包括指定编码约定、库，以及偏好的注释形式。你可以为整个存储库创建指令，也可以针对特定类型的文件创建指令。

指令文件有两种：

- `.github/copilot-instructions.md` 是一个适用于存储库每次请求的指令文件，内容应是与大多数请求相关的项目级信息。
- `.github/instructions/*.instructions.md` 文件可用于特定任务或文件类型的指导，例如 TypeScript、Astro、UI 组件或单元测试。

> [!NOTE]
> 在 IDE 中，指令文件只用于 Copilot Chat 的代码生成，不用于代码补全或下一步编辑建议。
>
> Copilot Chat、Copilot CLI 和 Copilot 云代理在生成代码时都会使用存储库级指令和带有 `applyTo` frontmatter 的 `*.instructions.md` 文件。
>
> Copilot 还支持其他标准的指令文件，例如 AGENTS.md 和 CLAUDE.md，详情请参阅[自定义指令支持参考][custom-instructions-support]。

### 管理指令文件的最佳实践

本研讨会不详细介绍如何编写指令文件，但示例项目展示了一种有代表性的方法。总体而言：

- 将 `copilot-instructions.md` 用于项目级指导，例如项目内容、项目结构和全局编码标准。
- 使用 `*.instructions.md` 为文件类型（单元测试、Astro 组件、数据层）或特定任务提供指导。
- 使用自然语言，确保说明清晰，并给出代码示例，展示正确和错误的写法。

编写指令文件没有唯一的正确方式，使用 AI 也是如此。通过实践找到适合项目的方法。

> [!TIP]
> 每个使用 GitHub Copilot 的项目都应该有一套完善的指令文件。浏览本项目中的文件时，你会看到针对[界面更新][ui-instructions]和 [Astro][astro-instructions]等任务的指令。
>
> Copilot 也可以帮助生成指令文件。不同产品界面的操作方式不同，例如 VS Code 中的 **Configure Chat → Generate Agent Instructions**，或 Copilot CLI 中的 `/init`。相关练习会介绍对应界面的操作方式。
>
> 如需模板或入门示例，请浏览 [awesome-copilot][awesome-copilot]，其中包含指令文件、自定义代理和其他资源。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## 浏览本项目中的自定义指令文件

花些时间阅读此存储库自带的指令文件：一个核心 `copilot-instructions.md` 文件，以及一组面向不同任务的 `*.instructions.md` 文件。可以在编辑器或 GitHub 网页界面中打开这些文件。

1. 打开 `.github/copilot-instructions.md`。
2. 浏览项目简介，以及 **Agent notes**、**Code standards**、**Scripts** 和 **Repository Structure** 等部分。注意 **Code standards** 中的 **GitHub Actions Workflows** 子部分。这些内容适用于在此存储库中与 Copilot 的所有交互。
3. 打开 `.github/instructions` 文件夹，浏览其中的 Astro、Drizzle 数据层、测试等指令文件。
4. 打开 `.github/instructions/unit-tests.instructions.md`。注意顶部的 `applyTo` 字段，它指定了相对于存储库根目录的 glob；这里会匹配 TypeScript 测试文件，例如符合 `**/*.test.ts` 的文件。
5. 阅读有关本项目单元测试的说明。
6. 最后打开 `.github/instructions/drizzle.instructions.md` 并滚动到底部。注意其中指向其他指令文件（如 `unit-tests.instructions.md`）和项目现有文件的链接。这样可以将较大的指令集拆分成较小且可复用的部分，并提供示例供 Copilot 参考。（这些路径相对于指令文件，而不是存储库根目录。）

> [!NOTE]
> `copilot-instructions.md` 中的 **Code formatting requirements** 部分介绍了项目编码标准，但还没有要求在代码中添加文档。接下来会添加 TSDoc 文档注释和文件级注释头的规则。

## 添加新的存储库标准

接下来要编辑一次文件：添加项目级规则，要求在代码中使用 TSDoc 文档注释和文件级注释头。稍后在本环境中分配 issue 时，云代理会遵循这项规则。

编辑前先创建一个分支（云代理会从 issue 所指向的分支读取指令，但仍建议干净地提交这些编辑）：

1. 在 Codespace 终端中创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions-cloud
   ```

如前所述，`.github/copilot-instructions.md` 用于向 Copilot 提供项目级信息。现在为存储库编码标准添加一项规则，帮助生成更符合要求的代码。

1. 重新打开 `.github/copilot-instructions.md`。
2. 找到 **Code formatting requirements** 部分，通常在第 27 行附近。注意该部分记录了项目编码标准，但还没有代码文档规则，这也是之前生成的辅助函数没有文档注释的原因。
3. 在现有标准下方添加以下 Markdown 内容，要求 Copilot 添加文件注释头和 TSDoc 文档注释：

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. 保存 `copilot-instructions.md`。

> [!TIP]
> 如上一课所示，可以在存储库级文件（`.github/copilot-instructions.md`）中编写全局指引，也可以使用 `*.instructions.md` 文件提供针对语言、文件类型或任务的说明。存储库级文件适合放置全局标准，例如刚添加的文档注释规则。

## 提交、推送并合并指令更新

云代理会读取 issue 目标分支中的指令。下一练习将 issue 分配给 Copilot 时，它会从 `main` 分支创建新分支，因此必须先将指令更新合并到 `main`，云代理才能使用这些指令。

1. 暂存并提交更改：

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. 推送分支：

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. 在 github.com 上创建从 `custom-instructions-cloud` 到 `main` 的拉取请求并将其合并。之后的练习中，云代理处理分配给它的 issue 时就能读取这些指令。

> [!TIP]
> 如果希望在本研讨会中直接使用 `main`，可以跳过分支步骤并直接提交到 `main`。这里保留分支步骤是为了模拟真实项目中的工作方式。

## 总结和后续步骤

你了解了 Copilot 如何获取本项目指令文件中的上下文，并在 `.github/copilot-instructions.md` 中添加了新的全局存储库标准。之后的练习中，你会看到云代理创建的拉取请求如何遵循该标准。

接下来，将第一个 issue [分配给 Copilot 云代理][next-lesson]。

## 资源

- [GitHub Copilot 自定义指令文件][instruction-files]
- [创建自定义指令的最佳实践][instructions-best-practices]
- [编写更好的 Copilot 自定义指令的 5 个技巧][copilot-instructions-five-tips]
- [Awesome Copilot — 指令文件和其他资源集合][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
