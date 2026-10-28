---
title: "第 8 课 - 创建并合并功能 PR"
description: "一并审查筛选功能、说明、技能更新、QA 配置和测试，然后创建 PR 并使用 Agent Merge。"
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

筛选功能实现、说明更新、技能更新、质量保证 (QA) 配置和测试都保存在同一分支上。现在一起审查这些内容并创建拉取请求。你之前手动合并了星级评分拉取请求（PR）；这次将让 **Agent Merge** 管理合并流程。

> [!NOTE]
> 通常会将功能、说明更新、技能更新和 QA 智能体拆分到几个独立的 PR 中。为了简化研讨会流程，筛选和质量工作流共用一个会话和分支，所有工作都放在此 PR 中。

本课将介绍如何：

- 了解 Agent Merge 以及它如何自动管理合并生命周期。
- 检查完整的功能 PR 和验证证据。
- 仅在审查后授权 Agent Merge，并确认 PR 已合并。

## 场景

在筛选工作流中，你使用 Copilot 规划、实现并验证了功能。Tailspin Toys 现在希望自动处理剩余 PR 工作，同时由开发人员掌握合并授权。

## 介绍 Agent Merge

**Agent Merge** 可在 GitHub Copilot app 中自动完成合并拉取请求所需的剩余工作。启用后，应用会话会读取 PR 并处理阻塞项：修复失败的持续集成 (CI) 检查、回复审查意见，以及在需要时变基；GitHub 允许后，它会立即合并。此功能在后台运行，应用重启后仍会继续，并在 PR 合并后自动关闭。

之前你一直手动选择 **Merge pull request**。Agent Merge 可以代为处理，但它编辑代码和合并的能力仍需要明确授权。授予合并权限前，请审查其允许执行的操作和工作内容。

## 使用 Agent Merge 管理 PR

代码已全部完成并审查。现在让 Agent Merge 管理 PR 流程。

1. 使用智能体选择器选择 **Default agent**。
2. 选择 **Create PR** 旁的下拉菜单。
3. 选择 **Agent Merge**。按钮会更改为 **Agent Merge**。
4. 选择 **Agent Merge** 按钮以启动流程。仅从下拉菜单中选择选项并不会启动流程。

Agent Merge 随即开始工作。它会：

- 创建包含标题和说明的拉取请求。
- 如果会话从议题启动，则在 PR 正文中引用相关议题。
- 对目标分支执行变基或处理潜在合并冲突。
- 监控 CI 流程，确保所有检查通过。
- 监控 PR 中其他开发人员或 Copilot 代码审查提供的反馈，并进行更新以解决这些意见。
- 可以选择在所有操作成功后自动合并 PR。

让 Agent Merge 在所有检查通过后合并 PR。

5. 授予合并权限前，审查 PR 差异和验证证据。
6. 选择 **Agent Merge** 旁的下拉菜单。
7. 确认 **Merge pull request** 旁有勾选标记。
8. 明确要求智能体合并：

   ```plaintext
   Merge the PR once all required checks and approvals pass. Do not bypass repository protections.
   ```

9. 跟进进度并解决报告的阻塞项。继续之前，确认 PR 状态为 **Merged**；仅启用合并权限或智能体正在运行，并不代表已完成合并。

> [!IMPORTANT]
> Agent Merge 不会绕过仓库保护或权限限制。请先解决这些阻塞项，再继续操作。

## 总结和后续步骤

你已自动化开发流程的多个环节，包括生成代码、测试和验证代码，以及管理拉取请求。本课中，你：

- 了解了 Agent Merge 以及它如何自动管理合并生命周期。
- 检查了完整的功能 PR 和验证证据。
- 仅在审查后授权 Agent Merge，并确认 PR 已合并。

接下来[使用现有画布并创建分类画布][next-lesson]，探索与智能体一起检查、规划和可视化工作的更多方式。

## 资源

- [使用 GitHub Copilot app 管理议题和拉取请求][managing-issues-prs]
- [关于 GitHub Copilot app][about-copilot-app]

[next-lesson]: ../9-canvases/
[managing-issues-prs]: https://docs.github.com/copilot/how-tos/github-copilot-app/managing-issues-and-pull-requests
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
