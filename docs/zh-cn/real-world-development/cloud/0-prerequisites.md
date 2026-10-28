---
title: "练习 0：先决条件"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

开始 Copilot 云智能体练习前，需要先做好准备。你将创建 Tailspin Toys 仓库的副本，并启动一个 [Codespace][codespaces]，用于编辑说明文件和审查云智能体生成的工作。

## 设置实验仓库

要为练习中的代码创建仓库副本，请从[模板][template-repository]创建一个实例。新仓库包含实验所需的全部文件。

1. 在新的浏览器窗口中，转到 [Tailspin Toys 模板][tailspin-template]。
2. 在实验仓库页面上选择 **Use this template**，然后选择 **Create a new repository**。

   ![Use this template 按钮](../../../_images/ex0-use-template.png)

3. 如果你参加由 GitHub 或 Microsoft 举办的活动，请遵循导师提供的说明。否则，请在你有权使用 GitHub Copilot 的组织中创建新仓库。

   ![输入仓库模板设置](../../../_images/ex0-repository-settings.png)

4. 记下新建仓库的路径（**organization-or-user-name/repository-name**），稍后会在实验中用到。

## 创建 Codespace

接下来，你将使用 Codespace 完成本实验。

[GitHub Codespaces][codespaces] 是基于云的开发环境，可让你直接在浏览器中编写、运行和调试代码。它提供功能齐全的 IDE，并支持多种编程语言、扩展和工具。

1. 转到刚创建的仓库。
2. 选择绿色的 **Code** 按钮。

   ![选择 Code 按钮](../../../_images/ex0-code-button.png)

3. 选择 **Codespaces** 选项卡，再选择 **+** 按钮以创建 Codespace。

   ![创建 Codespace](../../../_images/ex0-create-codespace.png)

创建 Codespace 需要几分钟，但比手动安装所有服务快得多。等待期间，可以探索 GitHub Copilot 的其他功能，接下来我们会继续介绍。

> [!CAUTION]
> 后续练习还会用到此 Codespace。现在先不要关闭它，请将其保留在浏览器选项卡中。

> [!NOTE]
> 本研讨会可在 Codespace 或本地 [dev container][dev-containers] 中运行。两者都会安装顺利完成练习所需的先决条件。如果要在本地运行，请在 VS Code 中打开克隆的仓库，并在提示时选择 **Reopen in Container**。VS Code 将构建与 Codespace 相同的 dev container。

[codespaces]: https://github.com/features/codespaces
[dev-containers]: https://code.visualstudio.com/docs/devcontainers/containers

## 总结

你已创建实验仓库的副本，并开始创建 Codespace。之后，你将在 Codespace 中配合 Copilot 云智能体开展工作。

## 下一步

接下来为云智能体添加自定义说明。继续学习[练习 1 - 自定义说明][next-lesson]。

## 资源

- [GitHub Codespaces 概述][codespaces]
- [从模板创建仓库][template-repository]
- [Codespaces 入门][codespaces-quickstart]

[tailspin-template]: https://github.com/github-samples/tailspin-toys
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[codespaces-quickstart]: https://docs.github.com/codespaces/getting-started/quickstart
[next-lesson]: ../1-custom-instructions/
