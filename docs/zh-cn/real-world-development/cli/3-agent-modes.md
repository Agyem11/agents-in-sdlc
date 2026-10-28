---
title: "第 3 课 - 智能体模式：Plan 和 Autopilot"
description: "使用 Plan 确定方案、使用 Autopilot 根据议题构建筛选功能，并通过 Interactive 模式审查结果。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

之前我们为项目添加了一个小功能，但更大的更改需要更稳健的流程。GitHub Copilot CLI 可以融入组织现有的工作流，帮助我们以正确的方式构建正确的功能。本课将开启一系列课程，介绍典型的智能体驱动开发流程：从议题着手创建新功能，确保代码有效、功能符合预期，最终成功合并到项目中。

> [!NOTE]
> 后续功能工作将沿用同一对话和分支。通常会为不同类型的文件创建不同分支或 PR，但本课程会简化流程，以便聚焦核心概念。

本课将介绍如何：

- 根据 GitHub 议题启动新的 Copilot 对话。
- 在 Plan 模式中确定需求。
- 使用 Autopilot 模式实现新功能。
- 审查代码。
- 在转发的浏览器中手动验证功能。

继续开发此功能时，你还将更新仓库说明、自定义现有的 `quality-checks` 技能、添加 MCP 验证、创建 QA 智能体，并创建功能 PR。

## 场景

Tailspin Toys 的游戏目录不断扩展，访客需要按类别和发行商筛选游戏。待办列表中的议题描述了此功能，但组合类别等细节需要在编码前确定。你将使用 Plan 模式解决这些问题，然后授权 Autopilot 在明确范围内实现功能。

## 背景

将 AI 编码智能体引入开发流程，并不会改变基本原则。事实上，这些原则变得更加重要。大多数开发人员的工作流程大致如下：

1. 从已提交的议题开始，确认要完成的工作。
2. 制定实现方案。
3. 编写并审查代码。
4. 运行测试以验证代码。
5. 手动验证新功能。
6. 创建拉取请求（PR）。
7. 代码审查完成且持续集成流程成功后，合并代码。

> [!NOTE]
> 具体流程会因团队和组织而异，但大多数流程都与上述步骤类似。

遵循标准流程可以确保 AI 生成的代码满足需求，并像手写代码一样经过审查。

## 对话模式

**对话模式**决定智能体可以自主执行多少工作。按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 可在模式之间切换：

- **Interactive**：你与智能体协作。智能体提出更改建议，并等待你的输入后再继续。
- **Plan**：智能体先制定计划，但不能编辑项目文件。
- **Autopilot**：智能体自主工作，编写代码、运行测试并不断迭代，直至任务完成。

先切换到 Plan 模式并审查计划，然后使用 Autopilot 实现计划。

## 从议题开始

开始筛选功能之前，返回 Codespace 并确认仓库和终端已准备就绪。

1. 返回 Codespace。如果它已停止，请重新启动。
2. 确认星级评分 PR 已合并。
3. 如果终端尚未打开，按 <kbd>Control</kbd>+<kbd>\`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux）。
4. 更新 `main`，然后为筛选功能创建分支：

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. 启动 Copilot CLI：

   ```bash
   copilot --yolo
   ```

6. 按两次 <kbd>Tab</kbd> 打开 **Issues** 选项卡。
7. 按 <kbd>A</kbd> 显示所有议题。
8. 使用方向键选中标题为 **Allow users to filter games by category and publisher** 的议题。
9. 按 <kbd>C</kbd> 将议题添加到提示中，并返回 **Session** 选项卡。

现在提示会以 `#7`（或类似编号）开头。`#` 可将 GitHub 议题或拉取请求添加到上下文中。

## 规划筛选功能

规划可以在交给 Copilot 前先确定实现功能或执行任务的方法。对于复杂任务，花时间规划通常很有帮助。现在切换到 Plan 模式，让 Copilot 制定计划。

1. 按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 切换到 Plan 模式。确认提示下方的模式指示器显示 **Plan**。
2. 在上一步添加的议题引用后输入以下提示：

   ```plaintext
   Create a plan for implementing this feature.
   ```

Copilot 会先探索项目，再确定最佳实现方案。

3. Copilot 可能会询问筛选功能的具体行为。根据自己的偏好回答，没有唯一正确答案。
4. 计划完成后，按 <kbd>Control</kbd>+<kbd>E</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>E</kbd>（Windows/Linux）展开计划。
5. 滚动查看并审查计划。
6. 如果计划中的任何内容与你的决定不符，请要求 Copilot 修改。

## 批准 Autopilot

计划制定并审查完成后，就可以开始实现。现在使用 Autopilot 让 Copilot 完成工作。

Autopilot 会持续处理任务，直到它认为工作已完成。

1. 选择 **Accept plan and build on autopilot (recommended)**，或当前版本中含义相同的选项。
2. 确认提示下方的模式指示器显示 **Autopilot**。
3. 观察 Copilot 如何按照计划生成代码并运行测试。

> [!NOTE]
> 批准后可能会立即开始实现，因此请先审查计划。如果 Copilot 报告缺少依赖项或端口冲突，请先解决环境问题，再将检查视为已完成。

## 审查并验证实现

与其他代码一样，生成的代码也必须在合并前审查。现在检查代码并运行网站，确认一切正常。

1. 按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 切换到 Interactive 模式。确认模式指示器不再显示 **Plan** 或 **Autopilot**。
2. 输入 `/diff`，检查筛选功能的实现和测试。
3. 将结果与议题和规划时作出的决定进行比较。
4. 审查完代码后，按 <kbd>Esc</kbd> 退出差异视图。
5. 查看项目检查结果，并要求 Copilot 修复失败项。

## 体验新功能

代码看起来没问题，但实际能运行吗？像之前一样启动应用，并通过 Codespaces 转发端口在网站中查看。

1. 让 Copilot 启动应用：

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Codespaces 报告端口 `4321` 可用时，选择 **Open in Browser**。
3. 尝试按类别筛选、按发行商筛选，以及计划中确定的组合筛选。
4. 确认重置和无结果时的行为符合议题要求和你的决定。
5. 返回 Codespace，并让 Copilot 停止它启动的开发服务器。

## 总结和后续步骤

你使用不同的对话模式构建并审查了功能。本课中，你：

- 根据 GitHub 议题启动了新的 Copilot 对话。
- 在 Plan 模式中确定了需求。
- 使用 Autopilot 模式实现了新功能。
- 审查了代码。
- 在转发的浏览器中手动验证了功能。

接下来深入了解如何生成代码，并确保代码遵循书面实践：[使用自定义说明][next-lesson]。

## 资源

- [GitHub Copilot CLI 中的 Autopilot][autopilot]
- [Copilot CLI 命令参考][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
