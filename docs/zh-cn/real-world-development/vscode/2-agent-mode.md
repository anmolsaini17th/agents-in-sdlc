---
title: "练习 2 - 使用 Copilot 智能体模式添加功能"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

即使是简单的应用更新，通常也需要修改多个文件并执行测试。开发者通常需要找到所有相关文件、进行修改、运行测试、调试，再找出遗漏的文件并继续修改。Copilot 智能体模式正是为此而设计的。

Copilot 智能体模式可以在 IDE 中更自主地工作。它会先探索项目结构，执行必要的更新，运行测试等任务，并自动修复发现的问题。接下来，使用智能体模式为网站添加新功能。

> [!NOTE]
> 智能体模式和云智能体虽然名称相似，但适用于不同场景。智能体模式在 IDE 中执行任务，支持快速反馈和交互；云智能体则像团队中的同伴一样异步工作，通过议题和拉取请求与你协作。

本练习将介绍如何：

- 让 Copilot 智能体模式探索项目、识别相关文件并协调更改。
- 使用 Copilot 智能体模式跨 UI 和数据层实现新功能。
- 在合并到代码库之前审查 Copilot 智能体模式生成的更改和测试。

## 场景

游戏列表不断增加，因此需要让用户按类别和发行商筛选。上一课已添加发行商辅助函数，现在将使用 Copilot 智能体模式完成数据层、UI 和测试的其余工作。

## 运行 Tailspin Toys 网站

修改网站前，先了解 Tailspin Toys 的当前功能。

该网站是一个开发者主题桌游众筹平台，可列出游戏并显示其详情。它是一个单体 Astro 应用，在构建时将页面渲染为静态 HTML。页面通过 frontmatter 使用 Drizzle ORM 直接查询本地 SQLite 数据库，没有单独的后端 API 或客户端 UI 框架。可复用的数据访问辅助函数位于 `src/lib/`，交互功能使用标准 DOM API 编写的小型作用域 Astro `<script>`。

### 启动网站

项目提供了 `npm run dev` 任务，用于启动单体 Astro 开发服务器。按照以下步骤在 GitHub Codespace 中运行：

1. 返回 Codespace；此时应已回到上一课完成的 `main` 分支。
2. 按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux）打开终端。
3. 创建并切换到筛选工作分支：

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 启动网站：

   ```bash
   npm run dev
   ```

   Astro 开发服务器准备就绪后，会显示类似以下 URL 的提示：

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> 如果弹出提示框，询问是否要打开 `http://localhost:4321` 的浏览器窗口，请选择 **x** 将其关闭。

5. 在终端中按住 <kbd>Command</kbd>（Mac）或 <kbd>Ctrl</kbd>（Windows/Linux），然后选择 Astro 服务器地址 `http://localhost:4321`，打开网站。

> [!NOTE]
> 使用 Codespace 时，从终端选择 localhost URL 会自动重定向到 `https://<your-codespace-name>-4321.app.github.dev/`。这是通往 Codespace 的私有隧道，网站服务器正在其中运行。

### 浏览网站

网站运行后，可以查看其主要功能：

- **主页**：显示桌游列表，包括名称、图片和说明。
- **游戏详情页**：选择游戏后会打开详情页，显示游戏名称、说明、发行商和类别等信息。

## 使用 Copilot 查看待办事项

> [!TIP]
> **打开 Copilot Chat**
>
> 开始以下练习前，返回 Codespace，打开 Copilot Chat 面板并选择 **New Chat**，开始干净的会话。模式和模型选择因练习而异；相关步骤会在需要时说明。

网站的初始实现可以正常运行，但我们希望添加新功能。先查看待办事项。使用模板设置项目时，系统已自动创建一组议题；让 GitHub Copilot 显示这些议题。

1. 在 Chat 视图的智能体下拉菜单中选择 **Agent**。该智能体会自主规划并跨文件实现更改、运行终端命令和调用工具。

   ![Chat 视图中的智能体选择器](../../../_images/shared-chat-mode-selector.png)

2. 从可用模型列表中选择 **Claude Sonnet 4.5**。

> [!CAUTION]
> 本研讨会作者并未偏好某个模型。编写本研讨会时使用了 Claude Sonnet 4.5，因此课程中选择该模型，希望获得较为一致的体验。由于 LLM 具有概率性，你可能会看到不同的建议；这是正常现象。

> [!NOTE]
> LLM 具有概率性，因此 Copilot 可能会使用不同的 MCP 命令，但仍应能完成任务。

3. 向 Copilot 发送以下提示，询问议题待办事项：

   ```plaintext
   请显示我的 GitHub 存储库中的待办事项，并根据对用户最有用的程度帮助我确定优先级。
   ```

4. 选择 **Continue** 运行列出所有议题的命令。
5. 查看生成的议题列表。

Copilot 还根据对用户的帮助程度为这些事项排定了优先级。

## 查看指令文件

让智能体生成代码前，先查看将用于提供上下文的指令文件。本练习将使用 [user interface (UI)][ui-instructions] 文件，其中说明了如何为网站添加功能。

1. 在 Codespace 中打开 `.github/instructions/ui.instructions.md`。
2. 查看整体指南，包括：
   - 项目架构概述。
   - 组件设计、可测试性和无障碍原则。
   - 指向各文件类型专用指令的链接，包括 Astro、Tailwind CSS 和 Drizzle。

> [!TIP]
> 指令文件可以引用其他指令文件和项目文件。路径相对于当前指令文件，因此可以重复使用内容、拆分复杂说明，并为 Copilot 提供模板和示例。

## 实现筛选功能

完成筛选至少需要对应用程序进行三项更改：

- 在数据层（`src/lib/`）中添加或完善筛选逻辑。
- 添加或更新筛选行为测试。
- 更新游戏列表页面，添加筛选 UI。

此外，合并前需要运行并通过测试。Copilot 智能体模式可以完成这些工作。

1. 继续当前 Copilot 会话，或选择 **New Chat** 开始新会话。
2. 选择 **Add Context**、**Instructions**，然后选择 **ui** 指令文件。

   ![选择 UI 指令文件的示例](../../../_images/ex3-select-instructions-file.png)

3. 确认 Chat 视图的智能体下拉菜单中仍选中 **Agent**。
4. 确认仍选择 **Claude Sonnet 4.5**。
5. 使用待办事项中的相关议题提示 Copilot 实现功能：

   ```plaintext
   请根据待办事项中相关 GitHub 议题的要求，更新网站以支持按发行商和类别筛选。上一课已经添加了发行商辅助函数，请保留并在需要时完善它，同时完成数据层、UI 和测试的其余工作。完成前确保所有测试通过。服务器已经在运行，无需重新启动。
   ```

6. 观察 Copilot 如何探索项目并查找相关文件，包括数据层辅助函数、UI 和测试。然后它会修改文件并运行测试。

   ![Copilot 正在探索项目文件](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot 会执行多个任务，例如探索项目、修改文件和运行测试。根据任务和代码库的复杂度，这可能需要几分钟。过程中编辑器可能会出现 **Keep** 和 **Undo** 按钮。Copilot 完成后，每项更改都会提供 **Keep** 或 **Undo** 选项；工作尚未完成时无需选择。

7. Copilot 提示运行测试时，选择 **Continue**。

   ![Copilot Chat 提示用户确认运行测试](../../../_images/ex3-agent-mode-run-tests.png)

8. 过程中可能会暂停，甚至出现测试失败。这没有关系！Copilot 会在生成代码和运行测试之间迭代，直到完成任务且未检测到错误。

   ![Copilot 智能体模式完整会话](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 检查生成的代码是否存在潜在问题。

> [!CAUTION]
> 务必审查 Copilot 或其他 AI 工具生成的代码。

10. 返回正在运行的网站浏览器，试用新功能。
11. 确认功能正常并审查代码后，在 Copilot Chat 窗口中选择 **Keep**。

## 总结和后续步骤

恭喜你！本练习介绍了如何使用 GitHub Copilot 智能体模式为 Tailspin Toys 网站添加功能。你了解到：

- 如何使用 GitHub Copilot 智能体模式跨 UI 和数据层实现新功能。
- 如何让 Copilot 智能体模式探索项目、识别相关文件并协调更改。
- 如何在合并到代码库之前审查 Copilot 智能体模式生成的更改和测试。

接下来，[使用 Playwright MCP 服务器测试功能][next-lesson]，然后为其创建拉取请求。

### 拓展练习 — 实现分页

随着游戏列表增加，网站需要分页。使用本练习学到的技能，提示 Copilot 更新网站以实现分页。考虑以下事项：

- 遵循现有最佳实践，包括使用已有指令文件。
- 决定分页方式：允许用户选择每页大小，还是使用固定大小。
- 编写提示时，提供必要信息，让 Copilot 按照你的设想实现功能。
- 你可能需要与 GitHub Copilot 多次交互，要求它更改并提供上下文。这是正常的协作方式。

## 资源

- [Copilot ask、edit 和 agent 模式：功能及适用时机][choose-mode]
- [VS Code 中的智能体模式][vs-code-agent-mode]

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
