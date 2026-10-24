---
title: "练习 4 - 监控和管理智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

前两项练习中，你已让 Copilot 云智能体处理三项改善用户体验和增加功能的任务。云智能体虽然异步自主工作，但监控任务进度仍然很重要。

可以使用多种工具管理分配给云智能体的任务，包括 GitHub.com 上的[agents 页面][agents-page]。在这个控制中心可以查看所有拥有未关闭 PR 的智能体任务、查看执行步骤，甚至引导正在运行的会话。

在本课中，你将：

- 浏览 agents 页面并监控云智能体任务。
- 引导正在进行的会话，要求添加更多功能。

## 场景

将高对比度模式任务分配给智能体后，团队意识到此时也适合添加浅色模式。网站样式和切换功能都在更新，因此应引导智能体同时添加浅色模式。

## 查看 Copilot 云智能体任务

查看分配给 Copilot 云智能体的任务状态。

1. 打开 [Copilot agents 页面](https://github.com/copilot/agents)。
2. 查看主面板和左侧面板中的任务列表，其中应包含更新代码文档、生成游戏修改 API，以及为网站添加高对比度模式等任务。
3. 选择一个正在运行的任务，审查 Copilot 已完成的工作，例如检出存储库、准备环境、设置 MCP 服务器和执行任务步骤。

> [!NOTE]
> 显示的具体步骤会根据 Copilot 的工作进度和实现方式有所不同。

4. 查看右侧的 PR 面板，可在其中查看 PR 和已更改的文件。

## 引导云智能体

现在已了解活动任务，接下来请 Copilot 在实现高对比度模式时也添加浅色模式切换开关。

1. 选择标题与高对比度模式相关的会话。具体标题会因 Copilot 的命名和任务状态而异。

   ![控制中心中的无障碍会话](../../../_images/ex6-accessibility-session.png)

2. 等待几分钟，直到会话完成设置并开始工作。此时会出现表示任务已开始的消息。
3. 在 **Steer active session while Copilot is working** 对话框中输入：

   ```plaintext
   While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
   ```

   ![agents 页面中的云智能体任务，突出显示 Steer active session while Copilot is working 对话框](../../../_images/ex6-steer-cloud-agent-task.png)

4. 按 <kbd>Enter</kbd> 发送提示。
5. 留意 Copilot 如何确认收到提示，并将新要求加入工作流程。

## 让 Copilot 完成工作

与之前一样，Copilot 会继续处理更新后的任务。收到消息后，它会先完成当前步骤，再将新要求纳入工作流程。

这需要几分钟，可以利用这段时间回顾已学内容。

## 总结和后续步骤

本课探索了 Copilot agents 页面，即监控和引导 GitHub Copilot 云智能体任务的中心位置。你可以跟踪正在运行和已完成的任务、审查执行内容，并调整正在进行任务的范围或提供额外指导。

本课介绍了：

- 浏览控制中心和 agents 页面，监控云智能体任务。
- 重定向正在进行的会话，要求添加更多功能。

Copilot 正在完成无障碍功能，接下来[迭代它创建的拉取请求][next-lesson]。

## 资源

- [Copilot agents 页面][agents-page]
- [关于自定义智能体][custom-agents]

---

| [← 上一课：自定义智能体][previous-lesson] | [下一课：迭代 Copilot 的工作 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
