---
title: "练习 0：先决条件"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

开始 Copilot 云智能体练习前，需要先做好准备。你将创建 Tailspin Toys 存储库的副本，并启动一个用于编辑指令文件和审查云智能体工作成果的 [Codespace][codespaces]。

## 设置练习存储库

从[模板存储库][template-repository]创建自己的副本。新存储库包含练习所需文件，后续会一直使用它。

1. 在新的浏览器窗口中打开 [Tailspin Toys 模板][tailspin-template]。
2. 在练习存储库页面选择 **Use this template**，然后选择 **Create a new repository**。

    ![Use this template button](../../../_images/ex0-use-template.png)

3. 如果你正在参加由 GitHub 或 Microsoft 主办的活动，请遵循导师提供的说明。否则，可在自己有权使用 GitHub Copilot 的组织中创建存储库。

    ![Input the repository template settings](../../../_images/ex0-repository-settings.png)

4. 记下新建存储库的路径（**organization-or-user-name/repository-name**），后续练习会用到。

## 创建 Codespace

接下来使用 Codespace 完成练习。

[GitHub Codespaces][codespaces] 是云端开发环境，可让你直接在浏览器中编写、运行和调试代码。它提供支持多种编程语言、扩展和工具的完整 IDE。

1. 打开刚创建的存储库。
2. 选择绿色的 **Code** 按钮。

    ![Select the Code button](../../../_images/ex0-code-button.png)

3. 选择 **Codespaces** 选项卡，再选择 **+** 按钮创建新的 Codespace。

    ![Create a new codespace](../../../_images/ex0-create-codespace.png)

创建 Codespace 需要几分钟，但比手动安装所有服务快得多。等待期间，可以探索 GitHub Copilot 的其他功能。

> [!CAUTION]
> 后续练习还会继续使用此 Codespace。暂时不要关闭它，请将其保留在浏览器标签页中。

> [!NOTE]
> 本工作坊可在 Codespace 或本地 [dev container][dev-containers] 中运行，两者都包含练习所需的先决条件。如果要在本地运行，请在 VS Code 中打开克隆的存储库，并在出现提示时选择 **Reopen in Container**。VS Code 会构建与 Codespace 相同的开发容器。

[codespaces]: https://github.com/features/codespaces
[dev-containers]: https://code.visualstudio.com/docs/devcontainers/containers

## 总结

恭喜，你已创建练习存储库副本并开始创建 Codespace，后续会在其中配合 Copilot 云智能体工作。

## 下一步

接下来添加云智能体会遵循的自定义指令。继续学习[练习 1 - 自定义指令][next-lesson]。

## 资源

- [GitHub Codespaces 概述][codespaces]
- [从模板创建存储库][template-repository]
- [开始使用 Codespaces][codespaces-quickstart]

[tailspin-template]: https://github.com/github-samples/tailspin-toys
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[codespaces-quickstart]: https://docs.github.com/codespaces/getting-started/quickstart
[next-lesson]: ../1-custom-instructions/
