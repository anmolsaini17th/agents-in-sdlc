---
title: "第 8 课 - 创建并合并功能 PR"
description: "一起审查筛选功能、指令、技能更新、QA 配置和测试，然后创建 PR 并使用 Agent Merge。"
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

筛选实现、指令更新、技能更新、质量保证 (QA) 配置和测试都已保存在同一个分支中。现在一起审查这些更改并创建拉取请求。你已经手动合并了星级评分 PR，这次将使用 **Agent Merge** 管理流程。

> [!NOTE]
> 通常会将功能、指令更新、技能更新和 QA 智能体拆分成多个 PR。为了简化研讨会，我们在同一会话和分支中完成了筛选与质量工作，并将其放入同一个 PR。

本课将学习如何：

- 了解 Agent Merge 及其如何自动化合并流程。
- 检查完整的功能 PR 和验证证据。
- 完成审查后授权 Agent Merge，并确认 PR 已合并。

## 场景

在整个筛选工作流中，你使用 Copilot 规划、实现并验证了功能。现在 Tailspin Toys 希望自动处理剩余 PR 工作，同时让开发者保留合并授权权。

## 认识 Agent Merge

**Agent Merge** 会在 GitHub Copilot app 中自动处理合并拉取请求所需的剩余工作。启用后，app 会话会读取 PR 并解决阻止合并的问题，例如修复失败的持续集成 (CI) 检查、响应审查意见，以及在需要时变基；GitHub 允许后便会合并。该流程在后台运行，即使 app 重启也会继续，并会在 PR 合并后自动关闭。

之前你亲自选择 **Merge pull request**。Agent Merge 可以代为处理，但它编辑代码和合并仍然需要你明确授权。授予合并权限前，请审查其允许的操作和已完成的工作。

## 使用 Agent Merge 管理 PR

代码已完成并审查后，让 Agent Merge 管理 PR 流程。

1. 使用智能体选择器选择 **Default agent**。
2. 选择 **Create PR** 旁的下拉菜单。
3. 选择 **Agent Merge**。按钮会变为 **Agent Merge**。
4. 选择 **Agent Merge** 按钮启动流程。仅在下拉菜单中选择该选项并不会启动流程。

Agent Merge 流程启动后会：

- 使用标题和说明创建拉取请求。
- 如果会话从议题开始，则在 PR 正文中引用相关议题。
- 针对目标分支变基或处理潜在合并冲突。
- 监控 CI 流程，确保所有检查通过。
- 监控 PR 中其他开发者或 Copilot 代码审查的反馈，并进行更新以解决评论。
- 可选择在所有条件满足后自动合并 PR。

接下来让 Agent Merge 在所有检查通过后合并 PR。

5. 授予合并权限前，审查 PR 差异和验证证据。
6. 选择 **Agent Merge** 旁的下拉菜单。
7. 确认 **Merge pull request** 旁有勾选标记。
8. 明确要求智能体合并：

   ```plaintext
   所有必需检查和审批通过后合并 PR。不要绕过存储库保护规则。
   ```

9. 跟进进度并处理报告的阻塞项。继续前确认 PR 状态为 **Merged**；仅启用了合并权限或智能体仍在运行，并不代表已完成合并。

> [!IMPORTANT]
> Agent Merge 不会绕过存储库保护规则或权限不足问题。继续前先解决这些阻塞项。

## 总结和后续步骤

你已自动处理开发流程中的多个部分，包括生成代码、测试和验证代码，以及管理拉取请求。你：

- 了解了 Agent Merge 及其如何自动化合并流程。
- 检查了完整的功能 PR 和验证证据。
- 完成审查后授权 Agent Merge，并确认 PR 已合并。

接下来，[使用现有画布并创建分诊画布][next-lesson]，探索使用智能体检查、规划和可视化工作的更多方式。

## 资源

- [在 GitHub Copilot app 中管理议题和拉取请求][managing-issues-prs]
- [关于 GitHub Copilot app][about-copilot-app]

[next-lesson]: ../9-canvases/
[managing-issues-prs]: https://docs.github.com/copilot/how-tos/github-copilot-app/managing-issues-and-pull-requests
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
