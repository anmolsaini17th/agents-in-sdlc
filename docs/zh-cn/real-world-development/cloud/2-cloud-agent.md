---
title: "练习 2 - GitHub Copilot 云智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：自定义指令][previous-lesson] |
|:--|

几乎所有组织都会遇到技术债务，例如尚未解决的安全问题、需要更新的旧代码，或因缺少时间而长期搁置的功能请求。GitHub Copilot 云智能体可以自主执行更新代码和添加功能等任务。完成后，它会创建草稿 PR，供开发者审查。这样可以将繁琐任务交给智能体，加快开发进度，让开发者专注于更重要的工作。

你将了解 Copilot 云智能体的以下用法：

- 自定义代码生成环境。
- 确保操作安全。
- 了解明确限定议题范围的重要性。
- 将议题分配给 Copilot。

## 场景

Tailspin Toys 有一些技术债务需要处理。最初受雇开发网站的承包商没有留下文档，因此代码缺乏说明。团队希望首先为应用程序中所有导出的函数添加 TSDoc 文档注释。

此外，设计团队希望改善游戏发现体验。他们希望在每款游戏的详情页中显示相关游戏（同一类别中的其他游戏），方便用户继续浏览。当前不需要完善的实现，只需要可用于验收测试用户体验的版本。此项工作目前受阻，但有其他优先级更高的议题。

这些任务可能很容易被搁置，适合分配给 Copilot 云智能体。它可以异步处理任务，让开发者先处理其他工作，之后再回来审查其成果。

## 认识 GitHub Copilot 云智能体

[GitHub Copilot 云智能体](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent)可以像开发者一样在后台执行任务。你可以通过多种方式与它协作，例如[将 GitHub 议题分配给 Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions)。分配任务后，Copilot 会创建草稿拉取请求以跟踪进度、设置环境并开始处理任务。你可以在会话进行中或完成后查看详情。解决方案准备好供审查时，它会在拉取请求中提及你。

## 明确限定指令范围的重要性

GitHub Copilot 没有魔法，使用它并不能省去思考。即使看似简单的操作，也可能包含复杂细节，因此要注意[为 Copilot 云智能体限定任务范围][cloud-agent-best-practices]。将它视为 AI 配对程序员：分阶段工作、学习、试验并不断调整。引入生成式 AI 并不会改变软件开发的基本原则。

## 本存储库中的自定义指令

前面的练习介绍了自定义指令及其作用。如果你已完成自定义指令练习，就已看到 `.github/copilot-instructions.md` 的效果；如果还没有，可以先快速查看。分配任务前，先只读查看存储库中的指令文件，稍后便能观察它们的影响。

在 GitHub Web 界面或正在运行的 Codespace 中打开以下文件：

- `.github/copilot-instructions.md`：查看 **Code standards** 部分，尤其是 TypeScript 规范和 TSDoc 文档注释要求。
- `.github/instructions/unit-tests.instructions.md`：注意 `applyTo` frontmatter，它将指令限定在 `**/*.test.ts` 文件。

在下一节中，将 `Code lacks documentation` 议题分配给云智能体时，留意它生成的拉取请求是否包含 TSDoc 文档注释、TypeScript 规范和文件注释头。稍后的练习还会再次审查这些内容。

## 为 Copilot 云智能体设置开发环境

编写代码通常需要特定环境和设置脚本，以确保环境正常；分配任务给 Copilot 也是如此。云智能体使用 [GitHub Actions][github-actions]执行任务。你可以通过 `.github/workflows/copilot-setup-steps.yml` 中的[专用设置工作流][setup-workflow]自定义其环境，为它提供所需工具和依赖项。本实验已提前配置此工作流，确保 Copilot 可以使用 Node.js、项目依赖项、端到端测试所需的浏览器，以及为单体 Astro 应用迁移并填充数据的 SQLite 数据库：

```yaml
name: "Copilot Setup Steps"

# 可从存储库的“Actions”选项卡测试这些设置步骤
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # 权限应限制为*这些步骤*所需的最低级别。Copilot 会使用自己的令牌执行操作。
    permissions:
      # 如果设置步骤需要克隆存储库（例如安装依赖项），则需要 `contents: read` 权限。如果设置步骤不克隆存储库，步骤完成后 Copilot 会自动为你克隆。
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # 前端/应用设置 - Node.js（整个应用现在基于 Astro + Drizzle/libSQL）
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # 迁移并填充本地 SQLite 数据库，以便构建和测试时有可用数据。
      - name: Set up the database
        run: npm run db:setup
```

它和其他 GitHub 工作流类似，但有几点需要注意：

- 其中只有一个名为 `copilot-setup-steps` 的作业。Copilot 开始处理拉取请求前，GitHub Actions 会运行此作业。
- `workflow_dispatch` 触发器可让你从存储库的 **Actions** 选项卡手动运行工作流。这样可以先测试工作流是否成功，而不必等待 Copilot 启动它。

## 添加文档

大家都知道文档很重要，但多数项目的文档要么已经过时，要么完全缺失。这类技术债务常被忽视，降低工作效率，也让维护代码库或引入新开发者变得更困难。Copilot 擅长生成文档，因此很适合把这项工作分配给云智能体；稍后你会回来审查结果。

1. 在新的浏览器选项卡中打开 github.com 上的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新议题对话框。
4. 选择 **Blank issue**。
5. 将 **Title** 设置为 `代码缺少文档注释`。
6. 将 **Description** 设置为：

   ```plaintext
   Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
   ```

7. 选择 **Create** 创建议题。
8. 在右侧选择 **Assign to Copilot**，打开分配对话框。

   ![将议题分配给 Copilot](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。
10. 选择 **Pull Requests** 选项卡。
11. 打开新生成的拉取请求（PR），其标题类似 `[WIP]: Code lacks documentation`。如果列表中没有新 PR，请稍等片刻并刷新浏览器。
12. 几分钟后，Copilot 应会创建待办事项列表。

> [!NOTE]
> Copilot 的待办事项可能需要几分钟才会显示在 PR 中。Copilot 正在创建环境（运行前面介绍的工作流）、分析项目并确定处理任务的方案。

13. 查看待办事项及即将执行的任务。
14. 向下滚动 PR 时间线，查看 Copilot 已开始处理议题的更新。
15. 选择 **View session** 按钮。

   ![Copilot 会话视图](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 可能需要刷新窗口才能看到状态指示器更新。

16. 浏览实时会话，了解 Copilot 如何解决问题，包括探索代码、了解现状、暂停思考并确定方案，以及编写代码。

这可能需要几分钟。Copilot 云智能体的主要优势之一是能够异步执行任务，让你专注于其他工作。接下来，我们会利用这一点：再分配一项任务给云智能体，同时开始为应用添加功能。

## 在游戏详情页添加相关游戏部分

如前所述，GitHub Copilot 云智能体的一大优势是可以拆分工作：你处理一项任务时，它可以处理另一项。为设计团队添加相关游戏部分可能不会花很长时间，但这段时间仍可用于其他工作。现在就把任务分配给 Copilot 云智能体。

1. 返回 github.com 上的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新议题对话框。
4. 选择空白模板 **Blank issue**。
5. 将 **Title** 设置为 `在游戏详情页显示相关游戏`。
6. 将 **Description** 设置为：

   ```markdown
   We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
   ```

7. 选择 **Create** 创建议题。
8. 在右侧选择 **Assign to Copilot**，打开分配对话框。

   ![将议题分配给 Copilot](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。

不久后，你会在议题的第一条评论旁看到 👀，表示 Copilot 已开始处理。

![Copilot 使用眼睛表情符号表示正在处理议题](../../../_images/ex4-issue-eyes-emoji.png)

Copilot 正在处理第二项请求！云智能体的工作方式类似软件工程师，因此你不需要一直监控它，完成后再审查即可。接下来，我们来创建和使用自定义智能体。

## 总结和后续步骤

本课介绍了 [GitHub Copilot 云智能体][copilot-agents]。可以将议题分配给云智能体，让它异步处理技术债务、开发新功能，或协助将代码迁移到其他框架。

你了解了以下内容：

- 自定义代码生成环境。
- 确保操作安全。
- 明确限定议题范围的重要性。
- 将议题分配给 Copilot。

云智能体正在后台工作，我们可以开始创建和使用自定义智能体。[云智能体还可以使用 MCP 服务器][cloud-agent-mcp]，并可使用之前练习中介绍的自定义指令。

## 资源

- [关于 Copilot 云智能体][copilot-agents]
- [将 GitHub 议题分配给 Copilot][assign-issue]
- [Copilot 云智能体设置工作流最佳实践][cloud-agent-best-practices]

| [下一课：自定义智能体 →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
