---
title: "练习 1 - 自定义说明（VS Code）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：先决条件][previous-lesson] · [下一课：智能体模式 →][next-lesson]

使用生成式 AI 时，上下文至关重要。如果任务需要按照特定方式完成，或者 Copilot 需要了解一些背景信息，就要确保它能访问这些信息。可以通过多种方式向 Copilot 提供特定上下文，其中一种重要方式是使用[说明文件][instruction-files]指导代码生成。

本练习将介绍如何：

- 通过仓库自定义说明和路径范围说明文件，为 Copilot 提供项目上下文、编码指南和文档标准。
- 在当前说明文件生效时生成筛选功能的第一个数据层切片（发行商辅助函数）。
- 在 `.github/copilot-instructions.md` 中添加一条适用于整个仓库的新标准。
- 再次发送相同提示，观察生成的代码如何遵循新标准。
- 将说明更新和筛选功能切片提交到 `main`，供 Copilot 在下一练习中使用。

> [!CAUTION]
> 生成的代码可能不完全遵循设定的标准。Copilot 的输出具有概率性。本练习旨在观察更新说明后行为的*整体变化趋势*，而不是要求输出完全一致。

## 说明文件

### 场景

Tailspin Toys 团队制定了一套开发指南和要求，包括：

- 数据层始终需要单元测试。
- UI 应采用暗色模式并具有现代感。
- 应使用 TSDoc 文档注释为代码添加说明。
- 每个文件开头都应添加注释块，说明文件用途。

通过说明文件，可以确保 Copilot 了解这些要求，并据此完成任务。

### 自定义说明

自定义说明可以向 Copilot 提供上下文和偏好，帮助它了解你的编码风格和要求。你可以指定编码约定、库以及注释风格，也可以为整个仓库或特定文件类型创建说明。

说明文件有两种类型：

- `.github/copilot-instructions.md` 是单个说明文件，会应用于仓库中的**每个**请求。文件应包含适用于大多数 Copilot Chat 或 CLI 请求的项目级信息，例如技术栈、项目概述、最佳实践和全局指南。
- `.github/instructions/*.instructions.md` 可针对特定任务或文件类型提供指南，例如 TypeScript、Astro、UI 组件或单元测试。

> [!NOTE]
> 在 IDE 中，说明文件仅用于 Copilot Chat 的代码生成，不用于代码补全或下一步编辑建议。
>
> Copilot Chat、Copilot CLI 和 Copilot 云智能体在生成代码时，都会使用仓库级说明文件以及带有 `applyTo` frontmatter 的 `*.instructions.md` 文件。
>
> Copilot 还支持 [AGENTS.md 和 CLAUDE.md 等其他标准的说明文件][custom-instructions-support]。

### 管理说明文件的最佳实践

完整讨论如何创建说明文件超出了本练习的范围。示例项目展示了一种有代表性的方法：

- 在 `copilot-instructions.md` 中集中放置项目级指导，例如项目说明、结构和全局编码标准。
- 使用 `*.instructions.md` 文件提供针对文件类型或任务的特定说明，例如单元测试、Astro 组件或数据层。
- 使用自然语言，保持说明清楚，并提供正反示例。

说明文件没有唯一的编写方式，AI 的使用方式也不止一种。通过实践可以找到适合项目的方法。

> [!TIP]
> 每个使用 GitHub Copilot 的项目都应配备完善的说明文件。浏览项目中的说明文件时，你会看到其中包含 [UI 更新][ui-instructions]和 [Astro][astro-instructions]等任务的指南。
>
> Copilot 也可以帮你生成说明文件。不同产品界面的操作有所不同，例如 VS Code 中的 **Configure Chat → Generate Agent Instructions**，或 Copilot CLI 中的 `/init`。相关课程会在适当位置介绍具体操作。
>
> 需要模板或起点？请查看 [awesome-copilot][awesome-copilot]，其中收集了说明文件、自定义智能体和其他资源。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/

## 查看项目中的自定义说明文件

花些时间查看仓库附带的说明文件：一个核心 `copilot-instructions.md` 文件，以及一组针对不同任务的 `*.instructions.md` 文件。你可以在编辑器或 GitHub 网页界面中打开这些文件。

1. 打开 `.github/copilot-instructions.md`。
2. 查看项目简介以及 **Agent notes**、**Code standards**、**Scripts** 和 **Repository Structure** 等部分。注意其中的 **Code standards** 下还有 **GitHub Actions Workflows** 指南。这些内容适用于你与 Copilot 的所有交互。
3. 打开 `.github/instructions` 文件夹，浏览其中有关 Astro、Drizzle 数据层、测试等内容的说明。
4. 打开 `.github/instructions/unit-tests.instructions.md`。查看顶部的 `applyTo` 字段；它设置相对于仓库根目录的 glob。此处匹配 TypeScript 测试文件，例如符合 `**/*.test.ts` 的文件。
5. 查看该文件中针对本项目单元测试的说明。
6. 打开 `.github/instructions/drizzle.instructions.md` 并滚动到底部。注意其中指向其他说明文件（例如 `unit-tests.instructions.md`）和项目现有文件的链接。这样可以将较大的说明集拆成可复用的小文件，并让 Copilot 参考已有示例。路径相对于说明文件所在位置，而不是仓库根目录。

> [!NOTE]
> `copilot-instructions.md` 中的 **Code formatting requirements** 部分介绍了项目编码标准，但尚未要求在代码中添加文档。接下来会添加 TSDoc 文档注释和文件注释头的规则。

## 为更改创建分支

遵循最佳实践，为本次更改创建分支。

1. 返回上一练习中使用的 Codespace。
2. 按 <kbd>Control</kbd>+<kbd>\`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux）打开终端。
3. 创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions
   ```

## 更新说明前先使用 Copilot Chat

> [!TIP]
> **打开 Copilot Chat**
>
> 开始以下练习前，返回 Codespace 并打开 Copilot Chat 面板，然后选择 **New Chat** 开始新的会话。每个练习会在相关步骤说明所需的模式和模型。

先在当前说明文件生效时发送提示。之后更新说明并重新发送相同提示，观察结果有什么不同。

1. 关闭之前练习中打开的编辑器选项卡，避免 Copilot 获取不必要的上下文。
2. 打开 `src/lib/publishers.ts`，让 Copilot 知道辅助函数应放在哪里。
3. 在 Chat 视图的智能体下拉列表中选择 **Agent**，以便 Copilot 应用文件更改。

   ![Chat 视图中的智能体选择器](../../../_images/shared-chat-mode-selector.png)

4. 发送以下提示：

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot 会探索项目并应用代码更新，通常会同时修改辅助函数和测试。
6. 注意新辅助函数是一个带类型的函数，以 `db` 客户端作为第一个参数，并返回带类型的发行商数组。这来自 `.github/instructions/drizzle.instructions.md` 中适用于 `src/lib/*.ts` 的数据层约定。
7. 注意生成的代码**缺少** TSDoc 文档注释和文件级注释头。

> [!CAUTION]
> Copilot 的输出具有概率性，因此它有时可能会在没有明确要求的情况下添加文档注释。即使如此也没有关系；本练习关注的是更新说明后行为是否变得更加一致。

## 添加新的仓库标准

如前所述，`.github/copilot-instructions.md` 用于向 Copilot 提供项目级信息。现在补充仓库编码标准，以改进代码建议。

1. 重新打开 `.github/copilot-instructions.md`。
2. 找到 **Code formatting requirements** 部分，通常在第 27 行附近。注意该部分尚未规定代码文档要求。
3. 在现有标准下方添加以下 Markdown 内容，要求 Copilot 添加文件注释头和 TSDoc 文档注释：

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. 保存 `copilot-instructions.md`。

> [!TIP]
> 如上一课所示，说明文件可以放在仓库级文件 `.github/copilot-instructions.md` 中，提供全局指南；也可以放在 `*.instructions.md` 文件中，针对语言、文件类型或任务提供说明。项目级文件适合存放代码注释规则等全局标准。

## 重新发送提示并观察变化

1. 返回 Copilot Chat，选择 **New Chat** 清除当前对话内容。
2. 再次打开 `src/lib/publishers.ts`，让 Copilot 聚焦正确的文件。
3. 发送与之前**完全相同的提示**：

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 注意新文件现在会以类似以下注释块开头：

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. 注意函数现在包含类似以下的 TSDoc 注释：

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

你刚才引导 Copilot 遵循新的项目标准，并将其应用到真实代码中，供下一练习继续使用。

## 提交说明并推送分支

说明文件和其他仓库资源一样，都应使用源代码管理。现在提交并推送分支。

1. 按 <kbd>Control</kbd>+<kbd>\`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux）打开新终端。
2. 在终端中运行以下命令，确认说明更新和辅助函数更改已存在：

   ```bash
   git status
   ```

3. 暂存并提交说明更新以及 Copilot 为辅助函数基础所做的所有更改：

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # If git status shows additional supporting updates (for example tests), add those files too.
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. 将分支推送到仓库：

   ```bash
   git push -u origin custom-instructions
   ```

## 创建并合并拉取请求

分支推送完成后，创建拉取请求并关联待办列表中的文档标准议题。Copilot 可以通过已连接到项目的 GitHub 工具帮你查找议题、创建关联 PR 并合并。

> [!NOTE]
> 这是首次让 Copilot 代表你在 GitHub 上操作。项目模板已经连接 GitHub 工具，无需额外设置。Copilot 首次使用工具时，VS Code 可能会提示你登录 GitHub。按照提示完成操作即可。后续练习会介绍这种连接如何工作（Model Context Protocol）。

1. 在 Codespace 中打开 Copilot Chat。
2. 按 <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd>（Windows/Linux）打开 Copilot Chat 视图，并确认智能体选择器中已选择 **Agent**。
3. 让 Copilot 查找与更新说明文件相关的议题，并从当前分支创建 PR，说明其中包含说明更新和新增的发行商辅助函数基础：

   ```
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot 会查找议题并创建 PR。
5. Copilot 提示你允许它代表你执行 GitHub 操作时，请先审查命令，再酌情选择 **Allow**。
6. PR 创建后，要求 Copilot 合并 PR 并将本地分支切回 main：

   ```
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. Copilot 提示你允许它执行 GitHub 操作和 shell 命令时，请先审查命令，再酌情选择 **Allow**。

你现在已在 GitHub Copilot 的帮助下创建并合并了拉取请求。

## 总结和后续步骤

你了解了 Copilot 如何读取本项目的说明文件，并通过 VS Code 中的 Copilot Chat：

- 发送代码生成提示，观察 Copilot 在*现有*说明下生成的内容。
- 在 `.github/copilot-instructions.md` 中添加适用于整个仓库的新标准。
- 再次发送相同提示，观察生成的代码如何遵循新标准。
- 将说明更新和辅助函数基础提交到 `main`，供下一练习使用。

接下来，在[智能体模式][next-lesson]中运用这些说明，让 Copilot 跨代码库添加新功能。

## 资源

- [GitHub Copilot 自定义说明文件][instruction-files]
- [创建自定义说明的最佳实践][instructions-best-practices]
- [编写更好的 Copilot 自定义说明的 5 个技巧][copilot-instructions-five-tips]
- [GitHub Copilot 个人自定义说明][personal-instructions]
- [awesome-copilot——说明文件及其他资源集合][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
