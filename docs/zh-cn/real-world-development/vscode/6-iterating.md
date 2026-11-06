---
title: "练习 6 - 迭代改进 GitHub Copilot 的工作"
authors:
  - geektrainer
lastUpdated: 2026-06-30
next: false
---

| [← 上一课：监控和管理智能体][previous-lesson] |
| :-- |

## 审查工作

在本实验中，你使用 GitHub Copilot 完成了多个改善用户体验的任务：使用智能体模式在客户端和服务器端添加筛选功能，通过 Playwright MCP 服务器在浏览器中手动测试，再使用自定义智能体实现包括高对比度和浅色模式切换开关在内的无障碍功能，并在会话进行中引导智能体扩展工作。现在该发布本地更改，并按照团队的方式进行审查了。

### 场景

引入生成式 AI 并不会改变软件设计和 DevOps 的基本原则。Copilot 生成的任何成果仍然需要完整的审查流程。接下来，将 Codespace 中的无障碍更改推送到远程、创建拉取请求，并在邀请团队其他成员参与前查看差异。

## 发布无障碍功能

在[练习 4][exercise-4]和[练习 5][exercise-5]中，使用无障碍自定义智能体实现的高对比度和浅色模式开关已作为提交保存在 Codespace 中。接下来，将它们推送到分支并创建拉取请求，供团队其他成员审查。

1. 返回 Codespace。
2. 在 VS Code 中打开 **Source Control** 视图。
3. 确认无障碍更改已提交。如果练习 5 中还有未提交的更改，现在将其暂存并提交，使用描述性消息，例如 `Add high-contrast and light-mode toggles`。
4. 选择 **Publish Branch** 发布分支（或使用 **...** 菜单 → **Push**）。
5. VS Code 会提示在 github.com 上打开新分支。接受提示，或手动前往存储库，在分支横幅上选择 **Compare & pull request**。
6. 设置清晰的标题（例如 `Add high-contrast and light-mode toggles`），并用简短说明概括所做的更改及其原因。
7. 选择 **Create pull request**。
8. PR 创建后，选择 **Files changed** 选项卡，完整审查工作。重点关注：
   - 用于切换模式的 UI 组件。
   - 用于保存用户偏好的 local storage。
   - 高对比度和浅色模式对应的 CSS 或样式更改。
   - 无障碍属性（ARIA 标签、键盘导航等）。
   - 管理模式切换的 JavaScript/TypeScript 代码。
9. 返回 **Conversation** 选项卡。
10. 如果工作流正在等待批准，选择 **Approve and run workflows**。

    ![批准并运行工作流](../../../_images/shared-approve-workflows.png)

11. 等待工作流完成。如果一切正常，所有工作流都应通过。

> [!TIP]
> 想为无障碍改进再获取一份意见？在 PR 评论中提及 `@copilot`，并提出请求，例如“审查此 PR，找出其他 WCAG 问题”或“建议如何改进键盘导航”。Copilot 会启动新会话来处理这条评论。

## 可选练习 - 继续在本地探索

在 IDE 中与智能体迭代协作是一项技能，只有反复练习才能掌握。以下是一些可从 VS Code 发起的后续会话思路：

- 在游戏详情页添加支持者意向表单。
- 在游戏列表页实现分页。
- 为 `src/lib/` 中的数据访问辅助函数添加输入验证和错误处理。
- 扩展无障碍智能体的工作范围，例如审查整个网站的键盘焦点顺序。

## 总结

恭喜，已完成 VS Code 练习系列！通过本实验，你：

- **使用 Playwright MCP 手动测试了功能。** 添加 Playwright MCP 服务器，让 Copilot 驱动浏览器验证筛选功能，然后再创建拉取请求。
- **使用智能体模式协调多层更改。** 在一个会话中添加了涉及客户端、服务器和测试的筛选功能。
- **使用了自定义智能体。** 从智能体选择器中选择无障碍自定义智能体，观察它根据存储库内容实现高对比度模式。
- **管理并引导了智能体会话。** 在编辑器中审查建议的更改，接受所需内容，并发送后续提示扩展会话以添加浅色模式。
- **通过拉取请求完成了闭环。** 发布本地工作，并按照团队的方式进行端到端审查。

## 回顾和后续步骤

必修的 VS Code 练习系列到此结束。你可以在这里停止，工作坊已完成。

如果想进一步了解 Copilot 的智能体能力，其他练习系列会通过不同工作环境介绍相关场景：

- 💻 **[CLI 练习系列](../../cli/)** — 在终端中交付两项经过审查的更改，体验 Plan 和 Autopilot 模式、技能、自定义 QA 智能体、Playwright MCP、Agent Merge，以及上下文感知的工作交接。
- ☁️ **[云智能体练习系列](../../cloud/)** — 重点介绍将议题分配给云智能体、通过智能体页面监控会话，以及异步迭代拉取请求。

也可以继续扩展已有成果。[awesome-copilot][awesome-copilot] 提供了丰富的指令文件、自定义智能体和技能，可根据自己的项目需要调整。

作为可选扩展，[可选：集成 Foundry][exercise-7] 使用 VS Code 和 Microsoft Foundry Toolkit 准备模型、部署 Backer Concierge，并将其连接到网站。

## 资源

- [GitHub Copilot][github-copilot]
- [VS Code 中的 Copilot Chat][copilot-chat-vscode]
- [使用智能体模式][agent-mode]

---

| [← 上一课：管理智能体][previous-lesson] |
|:--|

[previous-lesson]: ../5-managing-agents/
[exercise-4]: ../4-custom-agents/
[exercise-5]: ../5-managing-agents/
[exercise-7]: ../7-foundry-toolkit/
[github-copilot]: https://github.com/features/copilot
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
[awesome-copilot]: https://github.com/github/awesome-copilot
