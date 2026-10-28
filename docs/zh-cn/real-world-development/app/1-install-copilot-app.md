---
title: "第 1 课 - 安装 GitHub Copilot app"
description: "安装 GitHub Copilot app，连接通过模板创建的仓库，熟悉工作区并尝试快速聊天。"
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

**[GitHub Copilot app][about-copilot-app]** 是一款用于智能体驱动开发的桌面应用。它基于 GitHub Copilot CLI 构建，并与 GitHub 原生集成，因此仓库、分支和 CI 管道都可以直接使用。它适用于同时指挥多个智能体的工作流：每个智能体都在独立工作区中运行，让你无需亲自完成所有工作，并可自动执行重复性任务。安装 Node.js 并准备好项目副本后，下一步是安装应用并连接仓库。

本课将介绍如何：

- 安装 GitHub Copilot app 并登录。
- 从 GitHub 仓库将项目添加到应用。
- 熟悉工作区，包括模板创建的待办事项。
- 通过快速聊天了解应用本身。

## 场景

团队正采用 AI 智能体处理不断增长的待办事项。Copilot app 提供统一的工作位置，可领取议题、运行智能体、审查更改并合并拉取请求。本课将帮助你完成安装和连接，并熟悉如何开始讨论项目。

> [!NOTE]
> 需要使用符合条件的 Copilot 计划：Copilot Free、Copilot Student 或任一付费计划（Pro、Pro+、Business 或 Enterprise）。如果使用 Copilot Business 或 Copilot Enterprise，管理员必须启用 **GitHub Copilot app** 策略。

## 安装并配置 GitHub Copilot app

要使用 GitHub Copilot app，第一步自然是安装它。应用支持 Windows、macOS 和 Linux。现在安装应用、完成身份验证，并将 Tailspin Toys 仓库添加到应用。

1. 在浏览器中打开 [GitHub Copilot app 主页][download-app]。
2. 下载适用于当前平台的应用，并按照主页上的说明进行安装。
3. 安装完成后打开应用。
4. 选择 **Sign in to GitHub**，并按照提示完成身份验证。如果使用启用了数据驻留的 GitHub Enterprise Cloud，请选择 **Use GitHub Enterprise**，并在提示时输入 `*.ghe.com` 主机名。GitHub Enterprise Server 不受支持。
5. 如果引导流程中提供仓库选择，请选择从模板创建的 Tailspin Toys 仓库。也可以跳过仓库选择，并在完成引导流程后添加项目。
6. 按提示选择主题，然后完成引导流程。
7. 在侧边栏的 **Sessions** 下查找仓库。如果已经列出，则无需再次添加。
8. 如果仓库尚未连接，选择 **Sessions** 旁的 **+** 创建新会话，然后选择 **Add GitHub repository**。输入先决条件课程中记录的所有者和仓库名称。
9. 确认仓库出现在侧边栏中。当前布局会将 `tailspin-toys` 等仓库文件夹归在 **Sessions** 下，每个仓库下方再列出独立会话。

## 熟悉工作区

连接项目后，花一点时间熟悉工作区。应用会将功能组织在侧边栏的几个区域中：

- **New**：在此启动与 Copilot 的新聊天会话。
- **Pull requests**：通过应用与 GitHub 的原生集成显示拉取请求。在此可以浏览和筛选 PR、检查 CI 状态并审查 PR，全程无需离开应用。
- **Issues**：以同样的方式显示议题。在此可以浏览和筛选议题，并从议题启动会话。
- **Automations**：保存的智能体任务，可按计划或按需运行。适合管理待办列表、定期项目维护或其他重复性工作。总结课程会将其作为后续学习内容，而不是额外的研讨会练习。
- **Customize**：通过 MCP 服务器、插件、技能和其他组件，为 Copilot app 添加功能。你将使用此区域配置 Playwright MCP。
- **Chats**：用于提问和集思广益的轻量对话，无需为其创建分支或工作区。首次聊天后，此区域会显示出来。如果未显示，请选择 **Sessions** 旁的 **+** 启动第一次聊天。
- **Sessions**：用于分组显示仓库文件夹及其智能体会话。选择仓库名称（例如 `tailspin-toys`）可打开仓库视图；选择仓库下方的会话可返回相应对话。每个会话都在独立工作区运行，因此可以同时启动多个会话，且更改不会相互冲突。添加星级评分时会启动第一个会话。

完成研讨会的过程中，你会逐步了解工作区。

> [!TIP]
> 不确定时就问 Copilot！如果不清楚如何操作，或某件事能否实现，可以向 Copilot 提问，让它提供指导。

### 查找模板创建的待办事项

几乎每个项目都有待办列表，Tailspin Toys 也不例外。现在查看创建项目时由模板生成的待办事项。

1. 在侧边栏的 **Sessions** 下，选择 Tailspin Toys 仓库文件夹，而不是其下方的某个会话。然后在仓库顶部导航中选择 **Issues**。
2. 按标题查找以下议题，不要假设它们的议题编号：

   - Allow users to filter games by category and publisher
   - Update our repository coding standards

3. 选择议题阅读详情。每个议题也可以作为智能体会话的启动点。完成快速的首次更改后，你会从筛选议题启动会话。

> [!NOTE]
> **Issues** 和 **Pull requests** 中的列表会自动筛选，只显示添加到 Copilot app 的仓库中的项目。若要查看其他仓库的工作项，请将相应仓库添加到应用。

## 尝试快速聊天

熟悉应用的一种好方法是用它来了解*应用本身*。**快速聊天**正适合这种场景：无需创建分支或工作树即可提问或集思广益，非常适合不需要会话的一次性问题。

1. 在侧边栏中，选择 **Chats** 旁的 **+** 打开新聊天。如果尚未显示 **Chats**，请先选择 **Sessions** 旁的 **+** 开始第一次聊天；之后 **Chats** 区域就会出现。
2. 询问应用自身的会话工作方式：

   ```plaintext
   How does the GitHub Copilot app use worktrees?
   ```

3. 在对话视图中阅读回复。每个会话都在独立的 Git 工作树中运行，因此可以并行运行多个智能体，而不会造成更改冲突。你可以随时继续对话或开始新聊天。

## 总结和后续步骤

恭喜！你已安装 GitHub Copilot app、连接项目并探索工作区。本课中，你了解了如何：

- 安装应用并登录 GitHub。
- 从 GitHub 仓库添加项目。
- 熟悉工作区，并在 **Issues** 中查找模板创建的待办事项。
- 使用快速聊天提出一次性问题。

接下来[启动第一个智能体会话][next-lesson]，并用它在游戏卡片上显示星级评分。

## 资源

- [关于 GitHub Copilot app][about-copilot-app]
- [GitHub Copilot app 入门][getting-started]
- [在 GitHub Copilot app 中使用智能体会话][agent-sessions]

[next-lesson]: ../2-add-star-rating/
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
[getting-started]: https://docs.github.com/copilot/how-tos/github-copilot-app/getting-started
[agent-sessions]: https://docs.github.com/copilot/how-tos/github-copilot-app/agent-sessions
[download-app]: https://gh.io/app
