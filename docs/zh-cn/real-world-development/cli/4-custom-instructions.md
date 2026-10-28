---
title: "第 4 课 - 使用自定义说明引导 Copilot"
description: "查看仓库说明、添加文档标准，并将其应用于筛选代码。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

使用生成式 AI 时，上下文至关重要。如果任务需要按照特定方式完成，就要确保 Copilot 能访问相应指导。[说明文件][instruction-files]不仅描述需要编写什么代码，也说明代码应如何组织。现在你已经实现了筛选功能，接下来查看 Copilot 使用的说明、添加文档标准并将其应用于代码。

本课将介绍如何：

- 查看仓库说明和路径范围说明文件如何提供上下文。
- 更新说明文件，确保遵循编码标准。
- 观察说明文件对代码的影响。

## 场景

Tailspin Toys 团队制定了一套开发指南和要求，包括：

- 注释应解释意图和不明显的决策，而不是复述代码。
- `db/` 和 `src/lib/` 中的导出函数应使用 TSDoc/JSDoc 说明用途、参数和返回值；如果有可注入的 `db` 参数，也要说明。
- 可复用 Astro 组件应说明其 `Props` 契约；相关代码更改时，注释也应保持更新。
- 保留现有格式和 Lint 指南。

借助说明文件，可确保 Copilot 了解这些实践要求并据此完成任务。

## 说明文件

自定义说明可以向 Copilot 提供上下文和偏好，帮助它了解你的编码风格和要求。你可以指定编码约定、库以及注释风格，也可以为整个仓库或特定文件类型创建说明。

说明文件有两种类型：

- `.github/copilot-instructions.md` 是单个说明文件，会应用于仓库中的**每个**请求。文件应包含适用于大多数仓库请求的项目级信息。
- `.github/instructions/*.instructions.md` 可针对特定语言、文件类型或任务提供指南。

> [!NOTE]
> 不同环境对其他说明格式的支持情况各异。采用特定格式前，请查看[自定义说明支持参考][custom-instructions-support]。

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## 查看项目中的自定义说明文件

入门项目已包含一组说明文件。先查看这些文件，了解现状，再进行更改。

1. 返回 Codespace。
2. 在 Codespaces 编辑器（不是终端）中打开 `.github/copilot-instructions.md`。
3. 查看文件中的项目简介和编码指南。这些说明适用于 Copilot 在此仓库中的每次交互。
4. 打开 `.github/instructions` 文件夹并查看其中的文件，包括 Astro、Drizzle 数据层和测试说明。
5. 打开 `.github/instructions/unit-tests.instructions.md`。查看顶部的 `applyTo` 字段，它设置了说明适用文件的 glob。
6. 打开 `.github/instructions/drizzle.instructions.md`，查看它对其他说明文件和项目现有文件的引用。这种方式便于拆分较大的说明集，并提供可参考的示例。

## 根据团队指南更新说明文件

现有文件已经很有帮助，但还需要补充一条规则。我们要修改核心 `copilot-instructions.md` 文件，确保新生成的 TypeScript 包含 TSDoc 注释。

1. 在 `.github/copilot-instructions.md` 中找到 **Code formatting guidance** 部分，通常在第 35 行附近。
2. 在该部分的最后添加以下条目：

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

文件会自动保存。

## 使用更新后的说明

Copilot CLI 会在对话开始时加载仓库说明。保存更新后的说明后，退出并恢复筛选会话，这样 Copilot 就能在保留功能上下文的同时加载新指南。

1. 如果筛选会话仍在运行，输入 `/exit` 退出。Copilot CLI 会自动保存对话。
2. 在仓库根目录的 `game-filters-cli` 分支上，恢复已保存的对话：

   ```bash
   copilot --resume
   ```

   如果系统提示，请选择筛选会话并确认当前检出的是 `game-filters-cli` 分支。
3. 让 Copilot 应用更新后的指南：

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. 输入 `/diff`，审查更改的 TypeScript 文件。留意新添加的 TSDoc 注释，并确认其准确说明了代码。

## 总结和后续步骤

你了解了 Copilot CLI 如何读取项目说明，并将新标准应用于功能。本课中，你：

- 查看了仓库说明和路径范围说明文件如何提供上下文。
- 更新了说明文件，确保遵循编码标准。
- 观察了说明文件对代码的影响。

接下来[自定义并运行可复用的 quality-checks 技能][next-lesson]，确保一致地运行 Lint 和测试。

## 资源

- [为 Copilot CLI 添加自定义说明][instruction-files]
- [自定义说明支持][custom-instructions-support]
- [创建自定义说明的最佳实践][instructions-best-practices]
- [awesome-copilot——说明文件及其他资源集合][awesome-copilot]

[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
