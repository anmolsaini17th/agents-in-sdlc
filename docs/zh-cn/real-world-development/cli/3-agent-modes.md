---
title: "第 3 课 - 智能体模式：Plan 和 Autopilot"
description: "使用 Plan 确定方案、使用 Autopilot 根据议题构建筛选功能，并在 Interactive 模式中审查结果。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

我们刚才为项目添加了一个小功能，但更大的更改需要更完善的流程。GitHub Copilot CLI 可以配合组织现有的工作流，确保以正确方式构建正确功能。本课将开始一系列课程，按照典型的智能体驱动开发流程，从议题开始生成新功能，确认代码有效、功能符合预期，最后成功合并到项目中。

> [!NOTE]
> 后续功能工作将沿用同一对话和分支。通常会为不同类型的文件创建不同分支或 PR，但本研讨会会采用简化流程，帮助你专注于核心概念。

本课将学习如何：

- 从 GitHub 议题开始新的 Copilot 对话。
- 在 Plan 模式中明确需求。
- 使用 Autopilot 模式实现新功能。
- 审查代码。
- 在转发的浏览器中手动验证功能。

继续开发此功能时，你还会更新存储库指令、自定义现有 `quality-checks` 技能、添加 MCP 验证、创建 QA 智能体并创建功能 PR。

## 场景

Tailspin Toys 的目录不断扩大，访问者需要按类别和发行商筛选游戏。待办事项中的议题描述了该功能，但组合类别等细节需要在编码前确定。你将使用 Plan 模式明确这些决定，然后授权 Autopilot 在限定范围内实现功能。

## 背景

将 AI 编码智能体引入开发流程不会改变基本原则，反而使这些原则更加重要。多数开发者遵循的流程大致如下：

1. 从已创建、说明工作要求的议题开始。
2. 制定构建方案。
3. 编写并审查代码。
4. 运行测试验证代码。
5. 手动验证新功能。
6. 创建拉取请求（PR）。
7. 代码通过审查且持续集成流程成功后，合并代码。

> [!NOTE]
> 具体流程因团队和组织而异，但大多数流程都与上述步骤类似。

坚持采用标准流程，可以确保 AI 生成的代码符合要求，并经过与人工编写代码相同的审查过程。

## 对话模式

**对话模式**控制智能体的自主程度。按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 可在模式之间切换：

- **Interactive**：你与智能体协作。智能体提出更改建议，并等待你的输入后再继续。
- **Plan**：智能体先制定计划，在此模式下不能编辑项目文件。
- **Autopilot**：智能体自主工作，编写代码、运行测试并迭代，直至任务完成。

从 Plan 模式开始，审查计划，然后使用 Autopilot 实现。

## 从议题开始

开始筛选工作前，返回 Codespace，确认存储库和终端已准备就绪。

1. 返回 Codespace。如果它已停止，请重新启动。
2. 确认星级评分 PR 已合并。
3. 如果终端尚未打开，按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux）。
4. 更新 `main`，然后创建筛选功能分支：

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. 启动 Copilot CLI：

   ```bash
   copilot --yolo
   ```

6. 按两次 <kbd>Tab</kbd> 打开 **Issues** 选项卡。
7. 按 <kbd>A</kbd> 显示所有议题。
8. 使用方向键选中标题为 **Allow users to filter games by category and publisher** 的议题。
9. 按 <kbd>C</kbd> 将议题加入提示，并返回 **Session** 选项卡。

此时提示会以 `#7`（或类似编号）开头。`#` 可将 GitHub 议题或拉取请求（PR）加入上下文。

## 规划筛选功能

规划可以在将实现任务交给 Copilot 前明确功能方案。处理复杂任务时，花时间规划总是值得的。现在切换到 Plan 模式，让 Copilot 制定计划。

1. 按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 切换到 Plan 模式，确认提示下方显示 **Plan**。
2. 在上一步添加的议题引用后输入以下提示：

   ```plaintext
   制定实现此功能的计划。
   ```

Copilot 会先探索项目，再确定最佳方案。

3. Copilot 可能会询问筛选功能的工作方式。根据你的偏好回答，没有唯一正确答案。
4. 计划完成后，按 <kbd>Control</kbd>+<kbd>E</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>E</kbd>（Windows/Linux）展开计划。
5. 向上和向下滚动以审查计划。
6. 要求 Copilot 修改任何不符合你决定的部分。

## 批准 Autopilot

计划撰写并审查完成后，就可以开始实现。让 Copilot 使用 Autopilot 执行工作。

Autopilot 会让 Copilot 持续迭代，直到它认为任务已经完成。

1. 选择 **Accept plan and build on autopilot (recommended)**，或当前版本中含义相同的选项。
2. 确认提示下方显示 **Autopilot**。
3. 观察 Copilot 按计划迭代、生成代码并运行测试。

> [!NOTE]
> 批准后可能会立即开始实现，因此请先审查计划。如果 Copilot 报告缺少依赖项或端口冲突，请先解决设置问题，再将检查视为已完成。

## 审查并验证实现

代码生成后，和其他代码一样，合并前需要审查。现在检查代码并运行网站，确认一切正常。

1. 按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 切换到 Interactive 模式。确认模式指示器不再显示 **Plan** 或 **Autopilot**。
2. 输入 `/diff`，检查筛选实现和测试。
3. 根据议题和规划阶段做出的决定审查结果。
4. 审查完成后，按 <kbd>Esc</kbd> 退出差异视图。
5. 查看项目检查结果，并要求 Copilot 解决失败项。

## 试用新功能

代码看起来不错，但运行是否正常？像之前一样启动应用，并通过 Codespaces 转发端口打开网站。

1. 请求 Copilot 启动应用：

   ```plaintext
   启动应用，以便我在浏览器中试用筛选功能。告诉我 URL，并让服务器继续运行。
   ```

2. Codespaces 显示端口 `4321` 可用时，选择 **Open in Browser**。
3. 尝试按类别筛选、按发行商筛选，以及计划中确定的组合方式。
4. 确认重置和无结果时的行为符合议题和你的决定。
5. 返回 Codespace，要求 Copilot 停止启动的开发服务器。

## 总结和后续步骤

你使用不同的对话模式构建并审查了功能。本课中你：

- 从 GitHub 议题开始新的 Copilot 对话。
- 在 Plan 模式中明确需求。
- 使用 Autopilot 模式实现新功能。
- 审查代码。
- 在转发的浏览器中手动验证功能。

接下来，[使用自定义指令][next-lesson]深入了解代码生成，确保生成代码遵循已记录的实践。

## 资源

- [GitHub Copilot CLI 中的 Autopilot][autopilot]
- [Copilot CLI 命令参考][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
