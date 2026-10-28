---
title: "练习 5：迭代改进 GitHub Copilot 的工作"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：管理智能体][previous-lesson] |
|:--|

## 审查工作

在本实验中，你与 GitHub Copilot 一起完成了多项任务，以改进用户体验并添加功能。你让 Copilot 为代码添加文档、为设计团队构建可迭代的相关游戏功能，并实现包含高对比度和浅色模式切换开关的无障碍功能。现在查看代码更改；如有需要，向 Copilot 提供反馈以改进结果。

### 场景

如前所述，生成式 AI 不会改变软件设计和 DevOps 的基本原则。我们始终需要审查生成的代码，并遵循常规 DevOps 流程。接下来，在为团队启用代码审查前，先审查 GitHub Copilot 为代码文档、相关游戏功能和无障碍功能提出的更改。

## GitHub Copilot 云智能体的安全性

Copilot 云智能体会异步且自主地执行任务，因此需要采取一些安全限制：

- Copilot 对仓库只有读取权限，对其工作分支才有写入权限。
- 云智能体在 GitHub Actions 中运行，并在独立的临时环境中开展工作。
- 任何 GitHub Actions 工作流都必须先获得人工批准才能运行。
- 默认情况下，[对外部资源的访问受到限制](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall)，包括 MCP 服务器。

## 审查生成的文档

先查看 GitHub Copilot 云智能体创建的第一个拉取请求（PR）：为代码添加文档。你将在 GitHub.com 上使用标准 PR 界面完成审查。

> [!NOTE]
> 查看 PR 时，可能会看到 GitHub Copilot 被防火墙阻止的警告。这是**预期行为**，因为 Copilot 默认只能有限访问外部资源，包括外部 MCP 服务器。你可以选择[自定义或禁用 Copilot 云智能体的防火墙][agent-firewall]。

1. 返回 GitHub.com 上的仓库。
2. 选择 **Pull Requests**，打开拉取请求列表。
3. 打开标题类似 **Add missing documentation** 的拉取请求。

> [!NOTE]
> 如果 Copilot 仍在处理任务，PR 标题中会带有 **[WIP]** 标记。请等待 Copilot 完成工作，这可能需要几分钟。等待时可以休息一下，或回顾目前学到的内容。

4. PR 准备好后，选择 **Files changed** 选项卡并审查更改。

   ![Files changed 选项卡](../../../_images/shared-pr-files-changed.png)

5. 查看新添加到代码中的 TSDoc 文档注释和其他文档。具体更改可能会有所不同。

   查看更改时，留意 TSDoc 文档注释、TypeScript 约定和文件注释头。这些内容来自你在练习 2 开始时查看的自定义说明文件。

6. 审查完成且确认没有问题后，返回 **Conversation** 选项卡并向下滚动。
7. 查看是否有工作流正在等待批准。
8. 如果有，请选择 **Approve and run workflows**。

   ![批准并运行工作流](../../../_images/shared-approve-workflows.png)

9. 工作流会在 PR 的检查区域中排队。如果一切顺利，单个 Astro 应用的项目检查将通过。此过程可能需要几分钟。

## 要求 GitHub Copilot 更改

与 Copilot 协作并非单向过程。你可以像 @ 提及团队成员一样，在 PR 评论或代码行内评论中提及 Copilot。Copilot 会读取评论并启动新会话来处理请求。由于结果具有非确定性，我们无法提供固定的请求文本。你可以要求 Copilot：

- 在每个代码文件顶部添加简要说明文件用途的注释头。
- 为 TypeScript 和 Astro 文件添加 TSDoc 文档注释。
- 创建 README，介绍 Astro 应用结构。

1. 添加评论，要求修改生成的文档，并像提及用户一样标记 **@copilot**。你可以采用上面的建议，也可以提出其他文档改进要求。
2. 选择 **View Session**，查看 Copilot 如何开展工作。它会启动新会话来进行更新。
3. 选择 **Back to pull request** 返回 PR。

   ![返回拉取请求](../../../_images/ex7-back-to-pr.png)

4. Copilot 完成更改后，PR 中会出现新的提交。
5. 选择 **Files changed** 选项卡审查更改。

你可以继续迭代，直到满意为止。之后，将 PR 从草稿状态改为准备好审查，并合并到 main 分支。

![将 PR 标记为准备好审查](../../../_images/ex7-ready-for-review.png)

## 审查相关游戏功能

现在查看 Copilot 为“在游戏详情页显示相关游戏”议题创建的 PR。

1. 返回 GitHub.com 上的仓库。
2. 选择 **Pull Requests** 选项卡。
3. 选择标题类似 **Show related games on the game details page** 的 PR。
4. 选择 **Files changed** 选项卡，审查生成的代码。
5. 审查完成且确认没有问题后，返回 **Conversation** 选项卡并向下滚动。
6. 查看是否有工作流正在等待批准。
7. 如果有，请选择 **Approve and run workflows**。

   ![批准并运行工作流](../../../_images/shared-approve-workflows.png)

8. 工作流会在 PR 的检查区域中排队。如果一切顺利，单个 Astro 应用的项目检查将通过。此过程可能需要几分钟。
9. **可选：**你也可以在 Codespace 中切换到此分支，手动测试相关游戏功能。打开 Codespace 终端，并运行以下命令（将 `<branch-name>` 替换为 Copilot 创建的分支名称，例如 **copilot/fix-8**）：

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

Copilot 已经构建了相关游戏功能。你可以像之前一样继续迭代云智能体的工作，例如要求 Copilot 调整相关游戏的数量，或添加文件注释头和 TSDoc 文档注释（请记住，分配此任务时，你还没有更新自定义说明）。同样，只需在 **Conversation** 选项卡中添加评论，Copilot 就会收到请求并启动新会话。

## 审查无障碍功能

最后，审查使用无障碍自定义智能体实现的功能。此 PR 应同时包含练习 3 中分配的高对比度模式，以及练习 4 在任务控制中心要求的浅色模式。

1. 返回 GitHub.com 上的仓库。
2. 选择 **Pull Requests** 选项卡。
3. 选择标题类似 **Add high contrast mode to website** 的 PR。

> [!NOTE]
> 如果 Copilot 仍在处理任务，PR 标题中会带有 **[WIP]** 标记。请等待 Copilot 完成工作，这可能需要几分钟。

4. 选择 **Files changed** 选项卡审查生成的代码。
5. 查看实现，重点检查：
   - 用于切换模式的 UI 组件。
   - 用于保存用户偏好的本地存储。
   - 高对比度和浅色模式相关的 CSS 或样式更改。
   - 无障碍属性（ARIA 标签、键盘导航等）。
   - 管理模式切换的 JavaScript/TypeScript 代码。

6. 审查完成且确认没有问题后，返回 **Conversation** 选项卡并向下滚动。
7. 查看是否有工作流正在等待批准。
8. 如果有，请选择 **Approve and run workflows**。

   ![批准并运行工作流](../../../_images/shared-approve-workflows.png)

9. 工作流会在 PR 的检查区域中排队。如果一切顺利，单个 Astro 应用的项目检查将通过。此过程可能需要几分钟。
10. **可选：**可以在 Codespace 中切换到此分支，手动测试无障碍功能。打开 Codespace 终端，并运行以下命令（将 `<branch-name>` 替换为 Copilot 创建的分支名称）：

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    然后启动应用，在浏览器中测试高对比度和浅色模式切换，并确认设置在页面重新加载后仍然有效。

无障碍自定义智能体帮助 Copilot 遵循无障碍最佳实践实现这些功能。如果发现无障碍问题或改进空间，可以像处理之前的 PR 一样，在评论中标记 **@copilot** 并要求更新。

## 可选练习 — 继续委派任务

云智能体非常适合处理待办事项，让你能专注于其他工作。可以在仓库中创建更多议题并分配给 Copilot，以培养这种工作方式。以下是一些想法：

- 在游戏详情页创建支持者意向表单。
- 在游戏列表页实现分页。
- 为数据访问辅助函数添加输入验证和错误处理。

## 总结

你已完成云智能体工作流。在这些课程中，你：

- **查看了仓库附带的自定义说明文件**，以便了解它们如何影响云智能体的输出。
- **将议题分配给 Copilot 云智能体**，并查看它如何设置环境、制定计划并异步执行任务。
- **创建并使用无障碍自定义智能体**，添加高对比度和浅色模式切换开关。
- **使用 Copilot 智能体页面作为任务控制中心**，监控并引导无障碍会话。
- **审查并迭代改进云智能体的拉取请求**，通过标记 `@copilot` 请求更改并批准工作流。

## 回顾和后续步骤

你已完成云智能体工作流。其他工作流可以帮助你继续探索：

- 🖥️ **[VS Code 工作流](../../vscode/)** — 直接在 IDE 中探索 Copilot Chat 智能体模式和 MCP 集成。
- 💻 **[CLI 工作流](../../cli/)** — 在终端中交付两项经过审查的更改，学习 Plan 和 Autopilot 模式、智能体技能、QA 自定义智能体、Playwright MCP、Agent Merge，以及可选的 `/delegate` 云智能体桥接。

你也可以在自己的仓库中尝试以下内容：

- 将重构或测试覆盖率议题分配给云智能体。
- 为其他领域创建自定义智能体。
- 为不同技术栈设置 `copilot-setup-steps.yml`。

## 资源

- [GitHub Copilot][github-copilot]
- [关于 Copilot 智能体][copilot-agents]
- [将 GitHub 议题分配给 Copilot][assign-issue]
- [Copilot 云智能体设置工作流最佳实践][cloud-agent-best-practices]
- [配置 Copilot 云智能体防火墙][agent-firewall]

---

| [← 上一课：管理智能体][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall
[previous-lesson]: ../4-managing-agents/
