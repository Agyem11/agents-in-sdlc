---
title: "第 5 课 - 自定义并使用 quality-checks 技能"
description: "查看现有的 quality-checks 技能，自定义其报告格式，并用它验证筛选功能。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

编写代码不只是写代码。我们已经手动验证代码能够运行，也通过说明文件确保代码符合标准。测试、Lint 以及持续集成 (CI) 中的其他步骤又该如何处理？

这类任务很适合使用**智能体技能**。技能可以帮助 Copilot 正确执行这些操作。

本课将介绍如何：

- 查看现有的 `quality-checks` 技能。
- 自定义技能结果的报告格式。
- 重新加载并运行技能。

## 场景

Tailspin Toys 使用 `quality-checks` 技能运行单元测试、Lint 和类型检查。团队希望改进报告，让结果更易于阅读。

## 说明、脚本和资源

智能体技能会打包可重复使用的任务说明、可执行脚本和支持资源，供智能体按需加载。技能的核心是一个以技能名称命名的文件夹，其中包含名为 `SKILL.md` 的 Markdown 文件。该文件的 frontmatter 包含技能名称和描述，正文则说明技能的用途以及何时调用。技能文件夹还可以包含供技能使用的脚本和其他资源。

> [!NOTE]
> 技能不一定需要额外的文件夹或文件。Tailspin Toys 的 `quality-checks` 技能只包含 `SKILL.md`，因为它使用项目现有的命令。

技能可以放在项目的 `.github/skills` 文件夹中，作为团队可共享和复用的仓库资源；也可以放在用户技能文件夹 `~/.copilot/skills` 中。

## 查看技能

我们来看看 Tailspin Toys 团队创建的 `quality-checks` 技能，它用于运行单元测试、Lint 和类型检查。

1. 返回 Codespace，在 Codespaces 编辑器中打开 `.github/skills/quality-checks/SKILL.md`。
2. 阅读开头的 `name` 和 `description`。描述有助于 Copilot 判断何时调用此技能。
3. 阅读说明，了解它如何引导 Copilot 执行测试和 Lint。
4. 注意当前技能还没有 **Results output formatting** 部分。

## 修改前先运行技能

可以直接通过 Copilot CLI 调用技能，也可以使用自然语言。现在让 Copilot 运行这三项检查。

1. 返回处于 Interactive 模式的筛选会话。
2. 使用以下提示：

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. 查看最后生成的报告。

## 自定义报告

我们希望报告能说明运行了哪些检查、是否通过，以及工具实际报告的结果。接下来更新技能，使其生成这样的报告。

1. 返回 `.github/skills/quality-checks/SKILL.md`。
2. 在文件末尾添加以下部分：

   ```markdown
   ## Results output formatting

   Upon completion, report each command that ran and whether it passed, failed, or was blocked. Include test counts, durations, errors, warnings, and other metrics only when the tool reports them. Identify the next action for any failure or blocker, and never describe a skipped or incomplete check as passed.
   ```

3. 文件会自动保存。

## 运行更新后的技能

完成更改后，看看它的效果。Copilot CLI 可以在不重启会话的情况下重新加载已编辑的技能。

1. 输入：

   ```plaintext
   /skills reload
   ```

2. 使用与之前完全相同的提示：

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. 查看报告，并与首次运行的报告进行比较。

## 总结和后续步骤

你已自定义并使用现有的智能体技能。本课中，你：

- 查看了用于单元测试、Lint 和类型检查的 `quality-checks` 技能。
- 自定义了技能结果的报告格式。
- 重新加载并运行了技能。

这项更改会与筛选功能一起进入功能 PR。接下来，通过 [Playwright MCP 服务器][next-lesson]让 Copilot 直接与网站交互。

## 更多技能示例

以下社区示例仅供参考，不是额外的任务：

- [Agent Skills 规范][skill-spec]
- [贡献工作流：`make-repo-contribution`][contribution-example]
- [需求文档：`prd`][prd-example]
- [图表和配套导出脚本：`drawio`][drawio-example]
- [浏览器测试：`webapp-testing`][browser-example]

[previous-lesson]: ../4-custom-instructions/
[next-lesson]: ../6-mcp-playwright/
[skill-spec]: https://agentskills.io/specification
[contribution-example]: https://github.com/github/awesome-copilot/tree/main/skills/make-repo-contribution
[prd-example]: https://github.com/github/awesome-copilot/tree/main/skills/prd
[drawio-example]: https://github.com/github/awesome-copilot/tree/main/skills/drawio
[browser-example]: https://github.com/github/awesome-copilot/tree/main/skills/webapp-testing
