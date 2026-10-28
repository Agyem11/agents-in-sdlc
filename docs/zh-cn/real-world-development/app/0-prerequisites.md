---
title: "第 0 课 - 先决条件"
description: "为 GitHub Copilot app 课程做好准备：为 Tailspin Toys 项目安装 Node.js，并通过模板创建自己的仓库副本。"
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

GitHub Copilot app 是 Copilot 和 GitHub 的桌面中心枢纽。它支持快速访问议题和拉取请求，也支持使用 GitHub Copilot 进行构建。本研讨会中，你将在本地使用 GitHub Copilot app 更新基于 Astro 构建的 Tailspin Toys 应用。开始前，先确认本地已安装 Node.js，然后再安装 Copilot app。

本课将介绍如何：

- 安装 Node.js，以便在本机运行项目测试。
- 通过模板创建 Tailspin Toys 项目的副本。

## 安装 Node.js

多节课程会要求智能体构建功能，并在本地运行 Tailspin Toys 测试套件。这需要 **[Node.js][nodejs]**，它是项目唯一需要的运行时。请安装当前的 **LTS** 版本。

所有平台上最简单的方式都是使用官方安装程序：

1. 在操作系统中使用 Windows Terminal、macOS 终端或常用工具打开终端窗口。
2. 运行以下命令，检查已安装的 Node.js 版本：

   ```shell
   node --version
   npm --version
   ```

3. 如果两个命令都能成功运行，并且 Node.js 满足项目 README 和 `package.json` 中的要求，可以跳到下一节。

> [!TIP]
> 仅当尚未安装 Node，或需要更新版本时，才需要完成以下步骤。

4. 打开 [Node.js 下载页面][node-download]。
5. 下载适用于当前操作系统的 **LTS** 版本。
6. 运行安装程序并接受默认设置。在 Windows 上，保持 **Add to PATH** 选项处于选中状态。
7. 安装完成后，打开新的终端窗口。
8. 在新终端窗口中运行以下命令，确认安装成功：

   ```bash
   node --version
   npm --version
   ```

9. 屏幕上应显示 Node.js 和 npm 的版本。

> [!IMPORTANT]
> Copilot app 中的智能体也必须能够运行 `node --version` 和 `npm --version`。普通终端中运行成功，并不表示应用也能访问这些命令。安装 Node.js 或更改 PATH 后，请完全退出并重新打开应用。如果会话中仍无法运行其中任一命令，请先按照[会话环境故障排除步骤][session-environment]操作。

> [!IMPORTANT]
> 每个工作树也需要项目依赖项和用于 E2E 检查的 Playwright Chromium。准备工作树时，请遵循 Tailspin Toys 仓库 README，并在批准前审查安装请求。

## 设置实验仓库

你将在自己的 Tailspin Toys 项目副本中工作。现在从[模板仓库][template-repository]创建副本。新仓库包含实验所需的全部文件，安装应用时会连接该仓库。

1. 在新的浏览器窗口中，转到本实验的 GitHub 仓库：`https://github.com/github-samples/tailspin-toys`。
2. 在实验仓库页面上选择 **Use this template**，然后选择 **Create a new repository**。

   ![模板菜单中的 Use this template 按钮，已选择 Create a new repository](../../../_images/app-0-use-template.png)

3. 如果你参加由 GitHub 或 Microsoft 举办的活动，请遵循导师提供的说明。否则，可以在你有权使用 GitHub Copilot 的组织中创建新仓库。

   ![Create a new repository 表单，已将 github-samples/tailspin-toys 设为模板并填写仓库名称](../../../_images/app-0-create-repository.png)

4. 记下新建仓库的路径（**organization-or-user-name/repository-name**），稍后会在实验中用到。

> [!NOTE]
> 从模板创建仓库时，系统会自动为你创建 GitHub 议题待办列表。整个研讨会都会使用这些议题，无需自行创建。

请使用全新的研讨会模板副本。模板包含仓库说明、应用代码、测试、quality-checks 技能和现有画布扩展。你将在研讨会中自定义技能并创建 QA 智能体。如果使用旧副本，请向指导人员确认其中包含所需文件。

## 总结和后续步骤

环境已准备就绪。本课中，你：

- 安装了 Node.js，以便在本机构建和测试项目。
- 从模板创建了 Tailspin Toys 仓库的副本。

接下来[安装 GitHub Copilot app][next-lesson]、连接刚创建的仓库并熟悉工作区。

## 资源

- [下载 Node.js][node-download]
- [从模板创建仓库][template-repository]
- [关于 GitHub Copilot app][about-copilot-app]

[next-lesson]: ../1-install-copilot-app/
[nodejs]: https://nodejs.org/
[node-download]: https://nodejs.org/en/download
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
[session-environment]: ../3-agent-modes/#从议题启动会话
