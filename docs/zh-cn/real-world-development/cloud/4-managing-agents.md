---
title: "练习 4 - 监控和管理智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

前两项练习中，你让 Copilot 云智能体处理了三个任务，改进用户体验并添加新功能。云智能体虽然会异步自主工作，但监控这些任务仍然很重要。

你可以使用多种工具管理分配给云智能体的任务，包括 GitHub.com 上的[智能体页面][agents-page]。在这个任务控制中心，可以查看所有带有未关闭拉取请求（PR）的智能体任务，检查智能体执行的操作，还可以引导正在进行的会话。

本课将介绍如何：

- 浏览智能体页面并监控云智能体任务。
- 在会话进行期间引导智能体添加其他功能。

## 场景

将任务分配给智能体实现高对比度模式后，团队意识到也可以趁此机会添加浅色模式。网站样式和切换功能都在更新，因此可以一并实现。你将引导智能体同时添加浅色模式和高对比度模式。

## 查看 Copilot 云智能体任务

查看当前分配给 Copilot 云智能体的任务状态。

1. 前往 [Copilot 智能体页面](https://github.com/copilot/agents)。
2. 查看主面板和左侧面板中的任务列表。你应该能看到分配给 Copilot 的任务，包括：
   - 更新代码文档。
   - 生成用于修改游戏的 API。
   - 为网站添加高对比度模式。
3. 选择一个正在运行的任务，查看 Copilot 已执行的操作，例如：
   - 从仓库检出代码。
   - 为 Copilot 创建环境。
   - 设置 MCP 服务器。
   - 执行完成所分配任务所需的其他步骤。

> [!NOTE]
> 列出的具体步骤会根据 Copilot 的工作状态和执行方式而变化。

4. 同时查看右侧显示的拉取请求（PR）面板。你可以在此查看 PR 和更改的文件，以便进一步监控。

## 引导云智能体

查看当前正在执行的任务后，要求 Copilot 在实现高对比度模式的同时加入浅色模式切换开关。

1. 选择与添加高对比度模式相关的会话。具体标题会因 Copilot 使用的名称和当前工作状态而异。

   ![任务控制中心中的无障碍会话](../../../_images/ex6-accessibility-session.png)

2. 观察会话几分钟，直到 Copilot 完成准备工作并开始处理任务。出现类似下方的消息时，就表示已经开始工作。
3. 在 **Steer active session while Copilot is working** 对话框中，输入以下提示：

   ```plaintext
   While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
   ```

   ![智能体页面中的云智能体任务，突出显示了“Steer active session while Copilot is working”对话框](../../../_images/ex6-steer-cloud-agent-task.png)

4. 按 <kbd>Enter</kbd> 发送提示。
5. 注意 Copilot 会确认收到提示，并将其纳入任务流程。

## 让 Copilot 继续工作

和之前一样，Copilot 会继续处理更新后的任务。它会在完成发送提示时正在执行的步骤后，将新要求纳入工作流程。

这可能需要几分钟。等待期间，可以回顾目前学到和探索的内容。

## 总结和后续步骤

本课介绍了 Copilot 智能体页面，它是监控和引导 GitHub Copilot 云智能体任务的中心位置。在此可以跟踪所有正在进行和已完成的任务、查看工作内容，还可以重定向正在运行的任务以调整范围或补充指导。

本课介绍了：

- 使用任务控制中心和智能体页面监控云智能体任务。
- 重定向正在运行的会话以请求添加功能。

Copilot 正在完成功能。接下来审查 [Copilot 创建的拉取请求][next-lesson]并对其进行迭代。

## 资源

- [Copilot 智能体页面][agents-page]
- [关于自定义智能体][custom-agents]

---

| [← 上一课：自定义智能体][previous-lesson] | [下一课：迭代改进 Copilot 的工作 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
