---
title: "练习 1 - 自定义指令（VS Code）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：先决条件][previous-lesson] · [下一课：代理模式 →][next-lesson]

使用生成式 AI 时，上下文至关重要。可通过[指令文件][instruction-files]等方式提供 Copilot 生成代码所需的项目背景和指导。

本练习将学习如何：

- 通过存储库自定义指令和按路径限定的指令文件，让 Copilot 获取项目上下文、编码指南和文档标准。
- 在当前指令下生成筛选功能的数据切片（发行方辅助函数）。
- 在 `.github/copilot-instructions.md` 中添加全局存储库标准。
- 再次运行相同提示，观察生成代码如何应用新标准。
- 将指令和筛选代码提交到 `main`，供下一练习使用。

> [!CAUTION]
> 生成的代码可能不符合所有标准。Copilot 的输出具有非确定性。本练习旨在观察更新指令前后的行为趋势，而不是逐字匹配结果。

## 指令文件

### 场景

Tailspin Toys 的开发指南包括：数据层始终要有单元测试；UI 使用现代暗色模式；通过 TSDoc 文档注释说明代码；每个文件开头添加文件用途注释。

### 自定义指令

自定义指令可以向 Copilot 提供编码风格、库、注释偏好等上下文。`.github/copilot-instructions.md` 适用于存储库的每个请求；`.github/instructions/*.instructions.md` 可针对特定任务或文件类型提供说明。

> [!NOTE]
> 在 IDE 中，指令文件仅用于 Copilot Chat 的代码生成，不用于代码补全或下一步编辑建议。Copilot Chat、Copilot CLI 和云代理会在生成代码时使用存储库指令及带 `applyTo` frontmatter 的指令文件。Copilot 还支持 AGENTS.md 和 CLAUDE.md 等格式。

### 管理指令文件的最佳实践

- 将 `copilot-instructions.md` 用于项目说明、结构和全局编码标准。
- 使用 `*.instructions.md` 为文件类型或特定任务提供指导。
- 使用清晰的自然语言，并提供正反示例。

> [!TIP]
> 指令文件可链接到其他指令文件和项目文件。项目中已有针对 [UI][ui-instructions] 和 [Astro][astro-instructions] 等任务的指引。也可以从 [awesome-copilot][awesome-copilot] 查找模板。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## 浏览本项目中的自定义指令文件

1. 打开 `.github/copilot-instructions.md`，查看 **Agent notes**、**Code standards**、**Scripts** 和 **Repository Structure** 等部分。
2. 浏览 `.github/instructions` 文件夹中的 Astro、Drizzle、测试等说明。
3. 打开 `.github/instructions/unit-tests.instructions.md`，查看限定 TypeScript 测试文件的 `applyTo` 字段。
4. 打开 `.github/instructions/drizzle.instructions.md`，查看指向其他指令和项目示例的链接。

> [!NOTE]
> **Code formatting requirements** 部分尚未要求在代码中添加文档。接下来会加入 TSDoc 注释和文件级注释头。

## 创建变更分支

1. 返回 Codespace 并打开终端。
2. 创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions
   ```

## 更新指令前先使用 Copilot Chat

> [!TIP]
> **打开 Copilot Chat**
>
> 返回 Codespace，打开 Copilot Chat 面板并选择 **New Chat**。各练习使用的模式和模型不同，具体步骤会在相应位置说明。

1. 关闭之前练习留下的编辑器选项卡。
2. 打开 `src/lib/publishers.ts`，让 Copilot 知道辅助函数的位置。
3. 在 Chat 视图中选择 **Agent**。
4. 发送以下提示：

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot 会探索项目并修改文件，通常还会更新测试。
6. 注意生成的辅助函数通常会接收 `db` 客户端并返回类型化数组，这是 `.github/instructions/drizzle.instructions.md` 的数据层约定。
7. 注意生成的代码缺少 TSDoc 注释和文件级注释头。

> [!CAUTION]
> Copilot 具有概率性，有时即使未明确要求也会添加文档注释。这不影响本练习通过更新指令提高一致性的目标。

## 添加新的存储库标准

1. 重新打开 `.github/copilot-instructions.md`。
2. 找到 **Code formatting requirements** 部分（通常在第 27 行附近）。
3. 在现有标准下添加以下两行：

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. 保存文件。

## 再次运行提示并观察变化

1. 返回 Copilot Chat 并选择 **New Chat**。
2. 再次打开 `src/lib/publishers.ts` 并发送与之前相同的提示。
3. 检查文件开头是否新增说明用途的注释块，以及函数是否添加了说明用途、参数和返回值的 TSDoc 注释。

通过更新项目标准，你引导 Copilot 遵循新要求，并将其应用到真实代码中。

## 提交指令并推送分支

1. 在终端运行 `git status`，检查指令和辅助函数的修改。
2. 暂存并提交指令及相关文件：

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # If git status shows additional supporting updates (for example tests), add those files too.
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

3. 推送分支：

   ```bash
   git push -u origin custom-instructions
   ```

## 创建并合并拉取请求

1. 在 Codespace 中打开 Copilot Chat 并选择 **Agent**。
2. 要求 Copilot 找到与更新指令有关的 issue，并从当前分支创建 PR，说明指令更新和发行方辅助函数：

   ```
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

3. 按提示审查并允许 Copilot 执行 GitHub 操作。
4. PR 创建后，要求 Copilot 将其合并到 main，并更新本地分支：

   ```
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

5. 按提示审查并允许相应操作。

## 总结和后续步骤

你了解了 Copilot 如何读取指令，并在 VS Code 中使用 Copilot Chat：

- 发送代码生成提示，查看现有指令下 Copilot 的输出。
- 在 `.github/copilot-instructions.md` 中添加全局标准。
- 再次运行相同提示，观察代码如何应用新标准。
- 将指令和发行方辅助函数提交到 `main`，供下一练习使用。

接下来，在[代理模式][next-lesson]中运用这些指令，让 Copilot 跨代码库添加功能。

## 资源

- [GitHub Copilot 自定义指令文件][instruction-files]
- [创建自定义指令的最佳实践][instructions-best-practices]
- [编写更好的 Copilot 自定义指令的 5 个技巧][copilot-instructions-five-tips]
- [GitHub Copilot 个人自定义指令][personal-instructions]
- [Awesome Copilot — 指令文件和其他资源集合][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
