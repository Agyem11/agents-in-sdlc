---
title: "练习 1 - 自定义说明（云智能体）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：先决条件][previous-lesson] · [下一课：Copilot 云智能体 →][next-lesson]

使用生成式 AI 时，上下文至关重要。如果任务需要按照特定方式完成，或者 Copilot 需要了解一些背景信息，就要确保它能访问这些信息。[说明文件][instruction-files]用于提供这些指导，让 Copilot 不仅了解要做什么，也了解应该如何完成。

本练习将介绍如何：

- 通过仓库自定义说明和路径范围说明文件，为 Copilot 提供项目上下文、编码指南和文档标准。
- 在 `.github/copilot-instructions.md` 中添加一条适用于整个仓库的新标准。

> [!NOTE]
> 与 VS Code 和 CLI 工作流不同，本练习不会比较提示修改前后的结果。Copilot 云智能体会异步处理 GitHub 议题，因此难以实时并排展示说明的影响。后续审查云智能体创建的拉取请求时，你会看到说明文件发挥的作用。

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

- `.github/copilot-instructions.md` 是单个说明文件，会应用于仓库中的**每个**请求。文件应包含适用于大多数请求的项目级信息。
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
- 使用 `*.instructions.md` 文件提供针对文件类型或任务的特定说明。
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
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses

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

## 添加新的仓库标准

这是本练习中唯一需要编辑的内容：添加一条项目级规则，要求在代码中使用 TSDoc 文档注释并添加文件级注释头。云智能体在后续练习中处理议题时会读取这条规则。

编辑前，先创建一个分支。云智能体会从议题目标分支读取说明，但仍建议单独提交这项更改：

1. 在 Codespace 终端中创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions-cloud
   ```

`.github/copilot-instructions.md` 用于向 Copilot 提供项目级信息。我们来补充仓库编码标准，以改进代码建议。

1. 重新打开 `.github/copilot-instructions.md`。
2. 找到 **Code formatting requirements** 部分，通常在第 27 行附近。该部分记录了项目编码标准，但还没有规定代码文档要求。
3. 在现有标准下方添加以下 Markdown 内容，要求 Copilot 添加文件注释头和 TSDoc 文档注释：

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. 保存 `copilot-instructions.md`。

> [!TIP]
> 如上一课所示，说明文件可以放在仓库级文件 `.github/copilot-instructions.md` 中，提供全局指南；也可以放在 `*.instructions.md` 文件中，针对语言、文件类型或任务提供说明。项目级文件适合存放代码注释规则等全局标准。

## 提交、推送并合并说明更新

云智能体会从议题目标分支读取说明。下一课将把议题分配给 Copilot，而它会从 `main` 创建分支。因此，说明更改必须先合并到 `main`，云智能体才能使用。

1. 暂存并提交更改：

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. 推送分支：

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. 在 github.com 上创建从 `custom-instructions-cloud` 合并到 `main` 的拉取请求并合并。下一课中，云智能体处理议题时便会读取这些说明。

> [!TIP]
> 如果希望在本研讨会中直接使用 `main`，可以跳过分支步骤并直接提交到 `main`。此处创建分支是为了模拟真实项目中的工作方式。

## 总结和后续步骤

你了解了 Copilot 如何读取本项目的说明文件，并在 `.github/copilot-instructions.md` 中添加了新的仓库级标准。后续课程中，你会看到云智能体生成的拉取请求如何遵循该标准。

接下来，将第一个议题[分配给 Copilot 云智能体][next-lesson]。

## 资源

- [GitHub Copilot 自定义说明文件][instruction-files]
- [创建自定义说明的最佳实践][instructions-best-practices]
- [编写更好的 Copilot 自定义说明的 5 个技巧][copilot-instructions-five-tips]
- [awesome-copilot——说明文件及其他资源集合][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
