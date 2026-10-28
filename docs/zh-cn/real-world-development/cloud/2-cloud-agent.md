---
title: "练习 2 - GitHub Copilot 云智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：自定义说明][previous-lesson] |
|:--|

几乎所有组织都面临技术债务，例如尚未解决的安全问题、需要更新的旧代码，或因缺少时间而长期积压的功能请求。GitHub Copilot 云智能体可以自主执行更新代码和添加功能等任务。完成后，它会创建草稿 PR，供开发人员审查。这样可以将繁琐任务交给智能体，加快开发进度，让开发人员专注于更重要的工作。

本课将介绍如何使用 Copilot 云智能体：

- 自定义代码生成环境。
- 确保安全地执行操作。
- 编写范围明确的议题。
- 将议题分配给 Copilot。

## 场景

Tailspin Toys 有一些技术债务需要处理。最初开发网站的承包商没有留下完善的文档，事实上，代码完全缺少文档。团队希望首先为应用中的所有导出函数添加 TSDoc 文档注释。

此外，设计团队希望改进游戏发现体验。他们想在每款游戏的详情页上显示相关游戏，也就是同类别的其他游戏，以便访客继续浏览。团队目前不要求完善的实现，只需要可用于验收测试的初步用户体验。这项工作目前受其他更高优先级任务的影响，暂时无法开始。

这些任务很容易被降级到待办列表较低的位置，因此适合分配给 Copilot 云智能体。它可以异步处理任务，开发人员则可以继续处理其他工作，之后再回来审查结果。

## 介绍 GitHub Copilot 云智能体

[GitHub Copilot 云智能体](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent)可以像开发人员一样在后台执行任务。可以通过多种方式启动任务，例如[将 GitHub 议题分配给 Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions)。分配任务后，Copilot 会创建草稿拉取请求以跟踪进度、设置环境并开始工作。会话进行期间或完成后，你都可以查看其过程。解决方案准备好供你审查时，Copilot 会在拉取请求中提及你。

## 明确任务范围的重要性

GitHub Copilot 并非魔法——你仍然需要思考。即使看似简单的操作，深入分析后也会发现其复杂性，因此请注意为 Copilot 云智能体设定[明确的任务范围][cloud-agent-best-practices]。将它视为 AI 结对编程助手：分阶段工作、学习、试验并不断调整。引入生成式 AI 并不会改变软件开发的基本原则。

## 本仓库中的自定义说明

前面的练习介绍了自定义说明及其如何引导 Copilot。如果已经完成自定义说明练习，就会看到 `.github/copilot-instructions.md` 的作用；如果尚未学习，现在可以快速查看。分配任务前，先只读查看仓库中现有的说明文件，之后便能观察它们的影响。

在 GitHub 网页界面或已运行的 Codespace 中打开以下文件：

- `.github/copilot-instructions.md`：查看 **Code standards** 部分，尤其是 TypeScript 约定和 TSDoc 文档注释要求。
- `.github/instructions/unit-tests.instructions.md`：查看 `applyTo` frontmatter，了解这些说明如何应用于 `**/*.test.ts` 文件。

下一部分将把 `Code lacks documentation` 议题分配给云智能体。查看生成的拉取请求时，留意 TSDoc 文档注释、TypeScript 约定和注释头；这些要求来自上述说明文件。后续练习还会再次审查这些 PR。

## 为 Copilot 云智能体设置开发环境

无论由谁编写代码，通常都需要准备特定环境并运行一些设置脚本，确保一切就绪。将任务分配给 Copilot 时也是如此，因为它会像软件工程师一样执行工作。

云智能体使用 [GitHub Actions][github-actions] 作为工作环境。你可以通过 `.github/workflows/copilot-setup-steps.yml` 中的[专用设置工作流][setup-workflow]自定义环境，在智能体开始工作前安装所需工具和依赖项。实验已预先配置此工作流，以确保 Copilot 可以使用 Node.js、项目依赖项、端到端测试所需的浏览器二进制文件，以及已迁移并填充数据的 SQLite 数据库（此项目是单个 Astro 应用）：

```yaml
name: "Copilot Setup Steps"

# 允许你从仓库的 "Actions" 选项卡测试设置步骤
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # 将权限设置为 *你的步骤* 所需的最低级别。Copilot 会使用自己的令牌执行操作。
    permissions:
      # 如果要在设置步骤中克隆仓库（例如安装依赖项），需要 `contents: read` 权限。如果未在设置步骤中克隆仓库，步骤完成后 Copilot 会自动为你克隆。
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # 前端/应用设置 - Node.js（整个应用现已使用 Astro + Drizzle/libSQL）
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # 迁移并填充本地 SQLite 数据库，以便构建和测试时有可用数据。
      - name: Set up the database
        run: npm run db:setup
```

此文件与其他 GitHub 工作流文件类似，但有几点需要注意：

- 它只包含名为 `copilot-setup-steps` 的作业。Copilot 开始处理 PR 前，GitHub Actions 会运行此作业。
- 工作流使用 `workflow_dispatch` 触发器，因此可以在仓库的 **Actions** 选项卡中手动运行。无需等到 Copilot 启动工作流即可验证设置是否成功。

## 添加文档

大多数项目都存在信息过时或缺少文档的问题。这类技术债务往往长期无人处理，降低工作效率，也让维护代码库或引入新成员变得更加困难。Copilot 擅长生成文档，因此非常适合处理这类议题。它会在后台生成所需文档，后续练习中再回来审查。

1. 在新的浏览器选项卡中打开 github.com 上的仓库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新议题对话框。
4. 选择 **Blank issue** 创建空白议题。
5. 将 **Title** 设置为 `Code lacks documentation`。
6. 将 **Description** 设置为：

   ```plaintext
   Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
   ```

7. 选择 **Create** 创建议题。
8. 在右侧选择 **Assign to Copilot**，打开分配对话框。

   ![将议题分配给 Copilot](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。
10. 选择 **Pull Requests** 选项卡。
11. 打开新生成的拉取请求（PR），其标题类似 `[WIP]: Code lacks documentation`。如果列表中没有新 PR，请稍等片刻并刷新浏览器。
12. 几分钟后，Copilot 应会创建待办列表。

> [!NOTE]
> Copilot 可能需要几分钟才能在 PR 中显示待办列表。它正在创建环境（运行前面介绍的工作流）、分析项目并确定解决问题的最佳方法。

13. 查看待办列表和即将执行的任务。
14. 向下滚动 PR 时间线，查看 Copilot 已开始处理议题的更新。
15. 选择 **View session** 按钮。

   ![Copilot 会话视图](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 可能需要刷新窗口才能看到更新后的状态指示器。

16. 浏览实时会话，了解 Copilot 如何解决问题，包括探索代码和理解项目状态、暂停思考并确定方案，以及生成代码。

这可能需要几分钟。Copilot 云智能体的主要优势之一就是异步执行任务，让我们能专注于其他工作。接下来利用这一点，再分配一项任务给云智能体，同时开始为应用编写代码。

## 在游戏详情页添加相关游戏部分

如前所述，GitHub Copilot 云智能体的一大优势是可以拆分工作：你专注于一组任务，智能体同时处理另一组任务。为设计团队添加相关游戏部分可能不会花太久，但这段时间仍可用于其他工作。现在将任务分配给 Copilot 云智能体。

1. 返回 github.com 上的仓库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新议题对话框。
4. 选择 **Blank issue** 使用空白模板。
5. 将 **Title** 设置为 `Show related games on the game details page`。
6. 将 **Description** 设置为：

   ```markdown
   We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
   ```

7. 选择 **Create** 创建议题。
8. 在右侧选择 **Assign to Copilot**，打开分配对话框。

   ![将议题分配给 Copilot](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。

不久后，议题的第一条评论中会出现一组 👀，表示 Copilot 已开始处理。

![Copilot 使用 eyes 表情表示正在处理议题](../../../_images/ex4-issue-eyes-emoji.png)

Copilot 正在处理第二项请求。云智能体的工作方式类似软件工程师，无需持续监控，完成后再审查即可。现在可以继续创建并使用自定义智能体。

## 总结和后续步骤

本课介绍了 [GitHub Copilot 云智能体][copilot-agents]。可以将议题分配给云智能体，由它异步处理技术债务、创建新功能，或协助将代码迁移到其他框架。

本课介绍了：

- 自定义代码生成环境。
- 确保安全地执行操作。
- 编写范围明确的议题。
- 将议题分配给 Copilot。

云智能体在后台处理任务时，你可以开始创建和使用自定义智能体。[Copilot 云智能体也可以使用 MCP 服务器][cloud-agent-mcp]，并且能够读取前面模块介绍的自定义说明。

## 资源

- [关于 Copilot 云智能体][copilot-agents]
- [将 GitHub 议题分配给 Copilot][assign-issue]
- [Copilot 云智能体设置工作流最佳实践][cloud-agent-best-practices]

| [下一课：自定义智能体 →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
