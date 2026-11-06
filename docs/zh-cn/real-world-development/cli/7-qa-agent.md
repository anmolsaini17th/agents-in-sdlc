---
title: "第 7 课 - 创建并使用 QA 智能体"
description: "创建 QA 自定义智能体，将议题需求、quality-checks 技能和 Playwright MCP 结合起来。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

你已使用 `quality-checks` 技能运行自动检查，并使用 Playwright MCP 在浏览器中观察筛选体验。现在将这些能力整合到一个具有明确 QA 流程的自定义智能体中。

本课将学习如何：

- 了解自定义智能体如何结合指令、技能和 MCP 工具。
- 创建并检查可复用的质量保证配置。
- 选择 QA 智能体，根据筛选议题审查其结果。

## 场景

在创建拉取请求（PR）前，Tailspin Toys 希望统一检查需求、代码质量、自动化检查、测试覆盖率和浏览器行为。自定义智能体可以协调 QA 流程并生成可复用报告。

## 什么是自定义智能体？

自定义智能体是在 Markdown 配置文件中定义的专用 Copilot。配置文件说明智能体的用途、指令和可用工具。本研讨会将在 `.github/agents/qa.agent.md` 中定义 QA 角色，并在 Copilot CLI 中选择它。

你使用的不同自定义机制承担不同职责：存储库指令描述团队标准；`quality-checks` 技能封装可重复检查；Playwright MCP 提供浏览器工具；QA 配置则告诉 Copilot 如何利用这些能力评估需求并报告结果。QA 配置不会取代其他机制，也不需要另开会话。

## 创建 QA 配置

创建功能 PR 前，让 Copilot 创建可复用的 QA 配置。该配置将定义 QA 执行的检查和必须遵守的边界。

1. 返回 Codespace，确认筛选对话处于 Interactive 模式。
2. 让默认智能体创建新的自定义智能体：

   ```plaintext
   创建名为 QA 的自定义智能体，保存到 .github/agents/qa.agent.md。它应根据议题和已商定的需求检查功能，遵循存储库指令，运行 quality-checks 技能，使用 Playwright MCP 验证行为，并在覆盖范围不足时添加测试。

   要求它将每项需求报告为通过、失败或受阻，并提供支持证据。修改实现代码前必须征求我的同意，不得提交更改或创建拉取请求。

   现在只创建配置文件，供我审查。
   ```

## 检查配置文件

使用新智能体前，先审查其配置，确认 Copilot 捕获了预期的 QA 流程和边界，避免智能体在你只希望验证功能时进行不完整或范围过大的更改。

1. 输入 `/diff` 并打开 `.github/agents/qa.agent.md`。
2. 阅读 frontmatter。`description` 为必填项；`name` 是可选项，但添加后可以清楚显示智能体名称。
3. 阅读配置指令，确认 QA 会从需求开始、遵循存储库指令、运行 `quality-checks` 技能并使用 Playwright MCP。
4. 确认 QA 会提供支持证据、修改实现代码前会征求同意，并且不会提交更改或创建 PR。
5. 如果生成的配置遗漏任何职责或边界，请在继续前要求默认智能体修改。

## 根据议题运行 QA

Copilot CLI 会在对话启动时加载项目智能体。创建配置文件后，恢复同一筛选对话并选择 QA，使其能够使用已有的议题和规划决策。

1. 使用以下提示启用智能体：

   ```plaintext
   /agent QA
   ```

> [!NOTE]
> 刚创建的智能体可能不会立即出现在可用智能体列表中。它仍然可用，可以使用上述命令激活。

2. 使用以下提示要求 QA 智能体审查功能：

   ```plaintext
   根据议题和计划中的决定审查筛选功能。它是否已准备好创建 PR？
   ```

3. QA 智能体将开始工作。
4. 完成后阅读它提供的报告。

## 总结和后续步骤

你为工作流添加了可复用的专用角色，并审查了它的工作。本课中你：

- 了解自定义智能体如何结合指令、技能和 MCP 工具。
- 创建并检查 QA 配置。
- 选择 QA 智能体，根据筛选议题审查结果。

现在，实现、技能更新、QA 配置、测试和验证报告都已准备好供审查。接下来，[创建功能 PR 并使用 Agent Merge][next-lesson]。

## 资源

- [为 Copilot CLI 创建自定义智能体][create-agents]
- [自定义智能体配置][agent-config]

[previous-lesson]: ../6-mcp-playwright/
[next-lesson]: ../8-create-pull-request/
[create-agents]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli
[agent-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
