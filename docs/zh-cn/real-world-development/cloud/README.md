---
slug: zh-cn/real-world-development/cloud
title: "GitHub Copilot 云智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

**[GitHub Copilot 云智能体](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent)** 可让 GitHub Copilot 在云端异步工作。你可以在 GitHub 上分配任务，云智能体会在后台探索存储库、进行更改并创建拉取请求，而你可以继续处理其他工作。

在本系列练习中，你将添加云智能体会遵循的自定义指令，然后分配 GitHub 议题并让它实现任务。你还将审查并使用自定义智能体来引导工作方式，在智能体控制面板中监控和引导会话，最后审查云智能体创建的拉取请求并迭代结果。

## 练习

| 练习 | 主题 | 说明 |
| ------ | ----- | ---- |
| [0. 先决条件][ex0] | 设置 | 创建存储库和 Codespace |
| [1. 自定义指令][ex1] | 上下文 | 添加云智能体会遵循的自定义指令 |
| [2. 云智能体][ex2] | 异步智能体 | 将议题分配给 GitHub Copilot 云智能体 |
| [3. 自定义智能体][ex3] | 专业智能体 | 审查并使用自定义智能体 |
| [4. 管理智能体][ex4] | 监控 | 监控并引导智能体会话 |
| [5. 迭代][ex5] | 审查 | 审查 PR、迭代 Copilot 的工作并决定后续步骤 |

## 先决条件

参加本工作坊前，请确保具备以下条件：

- [ ] 拥有有效的 **Copilot Student、Pro、Pro+、Business 或 Enterprise** 计划
- [ ] 已为帐户或组织启用 Copilot 云智能体

> [!TIP]
> 没有付费计划？经过验证的学生可通过 [GitHub Education][callout-student-plan-education] 免费使用 GitHub Copilot。**Copilot Student** 计划包含本工作坊使用的智能体、MCP、代码审查和 Copilot CLI 功能，可用于完成所有环境的工作坊。

[callout-student-plan-education]: https://github.com/education/students

> [!NOTE]
> 云智能体适用于 **Copilot Student、Pro、Pro+、Business 和 Enterprise**，不包括 Copilot Free。对于 Copilot Business 或 Enterprise，管理员必须为组织启用云智能体。

> [!NOTE]
> 本环境不介绍 MCP。若要了解如何在 Copilot 中使用 MCP 服务器，请查看 [CLI 环境][cli]或 [VS Code 环境][vscode]。

## 开始

**[从练习 0：先决条件开始 →][ex0]**

[ex0]: 0-prerequisites/
[ex1]: 1-custom-instructions/
[ex2]: 2-cloud-agent/
[ex3]: 3-custom-agents/
[ex4]: 4-managing-agents/
[ex5]: 5-iterating/
[cli]: ../cli/
[vscode]: ../vscode/
