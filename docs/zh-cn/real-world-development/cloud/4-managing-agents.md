---
title: "练习 4 - 监控和管理智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

在前几课中，你已让 Copilot 云智能体处理了三项任务，目标是改善用户体验并添加功能。云智能体虽然能够异步自主工作，但监控这些任务仍然很重要。

你可以使用多种工具管理分配给云智能体的任务，包括 GitHub.com 上的[智能体页面][agents-page]。在这个任务控制中心，可以查看所有包含未关闭拉取请求（PR）的智能体任务，也可以了解智能体执行的操作，甚至引导进行中的会话。

本课将学习如何：

- 使用智能体页面监控云智能体任务。
- 引导正在进行的会话，要求添加其他功能。

## 场景

分配高对比度模式后，团队意识到也可以添加浅色模式。由于智能体已经在更新网站样式并添加切换功能，因此也可以一并实现此功能。你将引导智能体，同时添加浅色模式和高对比度模式。

## 查看 Copilot 云智能体任务

查看当前分配给 Copilot 云智能体的所有任务的状态。

1. 打开 [Copilot 智能体页面](https://github.com/copilot/agents)。
2. 查看主窗格和左侧窗格中的任务列表。列表中应包含你分配给 Copilot 的任务，例如：
   - 为代码库补充文档。
   - 生成用于修改游戏的 API。
   - 为网站添加高对比度模式。
3. 选择一项正在运行的任务，查看 Copilot 已执行的工作，包括：
   - 从存储库检出代码。
   - 为 Copilot 创建环境。
   - 设置 MCP 服务器。
   - 执行完成所分配任务的各种操作。

> [!NOTE]
> 显示的具体步骤会因 Copilot 任务的状态和处理方式而异。

4. 同时留意右侧的拉取请求（PR）窗格，可以在此查看 PR 及已更改的文件。

## 引导云智能体

现在你已查看正在进行的任务，可以让负责高对比度模式的 Copilot 同时添加浅色模式。

1. 选择与添加高对比度模式相关的会话。确切标题取决于 Copilot 使用的名称和当前工作状态。

   ![任务控制中心中的无障碍会话](../../../_images/ex6-accessibility-session.png)

2. 观察会话几分钟，直到设置完成并开始执行任务。此时会出现类似下文的消息。
3. 在 **Steer active session while Copilot is working** 对话框中，输入以下提示：

   ```plaintext
   添加高对比度模式的同时，也添加浅色模式。浅色模式也应有切换开关，供用户选择所需的显示模式。
   ```

   ![智能体页面中的云智能体任务，突出显示 Steer active session while Copilot is working 对话框](../../../_images/ex6-steer-cloud-agent-task.png)

4. 按 <kbd>Enter</kbd> 发送提示。
5. 留意 Copilot 如何确认提示并将其纳入工作流程。

## 等待 Copilot 完成工作

与之前一样，Copilot 会继续处理更新后的任务！它会在完成你发送消息时正在执行的具体步骤后，将新要求纳入工作流程。

这同样需要几分钟。此时可以暂停一下，回顾至今学到和体验的内容。

## 总结和后续步骤

本课介绍了 Copilot 智能体页面，它是监控和引导 GitHub Copilot 云智能体任务的中心位置。你可以跟踪所有进行中和已完成的任务、审查智能体正在执行的工作，以及重定向进行中的任务来调整范围或补充指导。

你了解了以下内容：

- 使用任务控制中心和智能体页面监控云智能体任务。
- 重定向正在进行的会话，请求添加其他功能。

Copilot 正在完成无障碍功能。接下来，[迭代改进 Copilot 创建的拉取请求][next-lesson]。

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
