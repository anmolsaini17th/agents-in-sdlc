---
title: "第 2 课 - 添加星级评分：快速改进"
description: "在 GitHub Copilot app 中启动第一个智能体会话，修改游戏卡片并通过第一个拉取请求合并更改。"
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

熟悉工作区并尝试快速聊天后，现在开始**智能体会话**，进行项目中的第一次更改。我们会从小处着手：游戏数据中已经有星级评分，但主页的游戏卡片尚未显示它。让智能体显示评分、审查更改，并通过第一个拉取请求合并。

本课将学习如何：

- 启动智能体会话并了解会话结构。
- 请求智能体对项目进行小幅、有针对性的更改。
- 在工作区差异视图中审查更改。
- 在本地运行应用，并在浏览器中确认更改。
- 创建并合并第一个拉取请求。

## 场景

Tailspin Toys 中的每款游戏都有星级评分，并且详情页已经显示评分。但主页的游戏卡片只显示标题、类别、发行商和说明。作为热身，让智能体在每张卡片上显示现有评分。这项小型独立更改非常适合第一个会话。

## 会话的组成

**会话**是与智能体进行的对话，它在独立工作区中运行。每个会话都有专用的 **Git 工作树和分支**，因此可以同时运行多个会话，例如一个添加功能，另一个修复 bug，而更改不会相互冲突。会话按存储库分组显示在侧边栏中，选择任意会话即可切换。

会话中可以看到三项内容：与智能体的**对话**、智能体探索和编辑文件时的**工具活动**，以及显示差异的**已更改文件**列表。

## 开始会话并请求更改

开始新会话，探索项目并实现功能。在[设置 app][prior-lesson]时，已经从 GitHub 存储库添加项目。现在为该存储库创建新会话并请求更改。

1. 返回或打开 GitHub Copilot app。
2. 选择侧边栏顶部的 **+**，启动会话。
3. 选择存储库 `tailspin-toys`。
4. 在提示框下方选择**新工作树**和 **Interactive** 模式。使用以下提示请求更改：

   ```plaintext
   在列表页面的游戏卡片上显示每款游戏的 starRating（满分 5 星）。如果评分为 null，则显示“No rating yet”。保持卡片布局不变，添加测试并运行相关检查。
   ```

5. 按 <kbd>Enter</kbd> 将提示发送给 Copilot。

Copilot app 会先创建新的工作树，也就是项目的隔离副本。随后它会探索项目、查找需要更新的文件并生成代码。现在，你已使用 Copilot app 添加了新功能。

## 审查差异

所有 AI 生成的更改在合并前都应审查，即使更改很小也不例外。现在在 Copilot app 中检查更改。

1. 在 app 右上角选择 **Toggle review panel**。在打开的面板中选择 **Changes**，查看已更改文件及其差异。

   ![GitHub Copilot app 顶部工具栏，箭头指向 Create PR 右侧的 Toggle review panel 按钮](../../../_images/app-2-review-panel.png)

2. 查看 `GameCard.astro` 中添加的代码，该文件用于显示游戏详情。代码应类似以下内容：在有评分时显示评分，在 `starRating` 为 `null` 时显示 `No rating yet`：

   ```astro
   {game.starRating !== null ? (
       <span class="text-xs font-medium px-2.5 py-0.5 rounded bg-amber-900/60 text-amber-300" data-testid="game-rating">
           ★ {game.starRating} / 5
       </span>
   ) : (
       <span class="text-xs font-medium text-slate-500" data-testid="game-rating-empty">
           No rating yet
       </span>
   )}
   ```

> [!NOTE]
> 与所有生成式 AI 工具一样，Copilot 的输出具有概率性，因此实际代码可能与上述示例不同，但整体应较为相似。

## 检查更改

打开浏览器前，先审查智能体自动检查的结果。确认测试覆盖数字类型的 `starRating` 和 `null` 回退。缺少先决条件或跳过的检查不算通过；批准任何安装请求前都应先审查。

不要只阅读代码就假定它能正常工作。让 Copilot 打开网站，以便在浏览器画布中查看更新后的 UI。

> [!TIP]
> 画布是 Copilot app 中的交互式组件。稍后会探索自定义画布并创建自己的画布；现在先使用内置浏览器画布。

1. 使用以下提示，请求 Copilot 启动应用并在浏览器画布中打开页面：

   ```plaintext
   启动应用并在浏览器画布中打开。
   ```

2. 稍等片刻，应用会启动，浏览器窗口会在 Copilot app 中打开。
3. 确认有评分的游戏卡片显示满分五分的评分。
4. 完成后，要求 Copilot 停止此会话启动的开发服务器：

   ```plaintext
   停止开发服务器并关闭浏览器画布。
   ```

## 创建并合并第一个拉取请求

现在功能已经完成，可以创建拉取请求（PR）将代码合并到现有代码库。

1. 选择右上角的 **Create PR**。
2. 如果系统提示，选择 **Sign in with your browser** 并按照提示进行身份验证。
3. 如果 Copilot 说明存储库策略要求创建 PR 前必须关联议题，请确认它应遵循此流程。
4. Copilot 将开始创建 PR。
5. 选择聊天上方的 **PR** 气泡，在审查面板中打开 PR。你可以在此审查 PR。
6. 准备好后，选择 **Ready to merge**。
7. 在新对话框中选择 **Merge pull request**，合并 PR。

## 总结和后续步骤

恭喜！你使用 GitHub Copilot app 交付了第一次更改，具体包括：

- 启动智能体会话并了解会话结构。
- 指示智能体对游戏卡片进行小幅、有针对性的更改。
- 在工作区差异视图中审查更改。
- 在本地运行应用，并在浏览器中确认星级评分。
- 创建 PR 1、审查检查结果并明确合并。

接下来，从筛选议题开始，使用 [Plan 和 Autopilot 模式][next-lesson]构建更大的功能。

## 资源

- [在 GitHub Copilot app 中使用智能体会话][agent-sessions]
- [关于 GitHub Copilot app][about-copilot-app]
- [在 GitHub Copilot app 中管理议题和拉取请求][managing-issues-prs]

[prior-lesson]: ../1-install-copilot-app/
[next-lesson]: ../3-agent-modes/
[agent-sessions]: https://docs.github.com/copilot/how-tos/github-copilot-app/agent-sessions
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
[managing-issues-prs]: https://docs.github.com/copilot/how-tos/github-copilot-app/managing-issues-and-pull-requests
