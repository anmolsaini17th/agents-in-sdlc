---
title: "第 1 课 - 安装 GitHub Copilot app"
description: "安装 GitHub Copilot app，从 GitHub 存储库连接模板项目，熟悉工作区并尝试快速聊天。"
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

**[GitHub Copilot app][about-copilot-app]** 是用于智能体驱动开发的桌面应用。它基于 GitHub Copilot CLI 构建，并与 GitHub 原生集成，因此存储库、分支和 CI 管道开箱即用。它适用于由你指挥多个智能体并行工作的流程，每个智能体都有独立工作区；同时也能自动处理重复任务。Node.js 已安装且项目副本已准备好，接下来安装 app 并连接存储库。

本课将学习如何：

- 安装 GitHub Copilot app 并登录。
- 从 GitHub 存储库将项目添加到 app。
- 熟悉工作区，包括模板自动创建的待办事项。
- 通过快速聊天了解 app。

## 场景

团队正使用 AI 智能体处理不断增长的待办事项。Copilot app 将这些工作集中在一个位置：领取议题、运行智能体、审查更改并合并拉取请求。本课会完成安装和连接，并介绍如何开始关于项目的对话。

> [!NOTE]
> 需要符合条件的 Copilot 计划：Copilot Free、Copilot Student 或任一付费计划（Pro、Pro+、Business 或 Enterprise）。如果使用 Copilot Business 或 Copilot Enterprise，管理员必须保持 **GitHub Copilot app** 策略启用。

## 安装并配置 GitHub Copilot app

首先需要安装 GitHub Copilot app。Windows、macOS 和 Linux 均提供相应版本。现在安装 app、完成身份验证并添加 Tailspin Toys 存储库。

1. 在浏览器中打开 [GitHub Copilot app 登录页面][download-app]。
2. 下载适用于当前平台的 app，并按照页面提供的说明安装。
3. 安装完成后打开 app。
4. 选择 **Sign in to GitHub** 并按照提示进行身份验证。如果使用启用了数据驻留的 GitHub Enterprise Cloud，请在提示时选择 **Use GitHub Enterprise** 并输入 `*.ghe.com` 主机名。GitHub Enterprise Server 不受支持。
5. 如果新手引导过程中提供了存储库选择，请选择从模板创建的 Tailspin Toys 存储库。也可以跳过存储库选择，在完成引导后再添加项目。
6. 按提示选择主题，然后完成新手引导。
7. 在侧边栏的 **Sessions** 下查找存储库。如果已经列出，无需再次添加。
8. 如果尚未连接存储库，选择 **Sessions** 旁的 **+** 创建新会话，然后选择 **Add GitHub repository**。输入先决条件课程中记录的所有者和存储库名称。
9. 确认存储库显示在侧边栏中。此布局会在 **Sessions** 下按存储库分组显示文件夹，例如 `tailspin-toys`；每个存储库下方会列出单独的会话。

## 熟悉工作区

连接项目后，先熟悉 app 界面。侧边栏包含几个区域：

- **New**：开始与 Copilot 的新聊天会话。
- **Pull requests**：通过 app 的原生 GitHub 集成查看拉取请求。你可以浏览和筛选 PR、检查 CI 状态并审查 PR，无需离开 app。
- **Issues**：以相同方式查看议题。你可以浏览和筛选议题，并从议题启动会话。
- **Automations**：保存按计划或按需运行的智能体任务。适合管理待办列表、定期维护和其他重复工作。总结课程会将其作为后续学习内容，而不是本研讨会的另一项练习。
- **Customize**：通过 MCP 服务器、插件、技能和其他组件扩展 Copilot app。后续会使用它配置 Playwright MCP。
- **Chats**：用于提问和头脑风暴的轻量对话，无需分支或单独工作区。开始首次聊天后会显示此区域。如果尚未显示，选择 **Sessions** 旁的 **+** 开始第一次聊天。
- **Sessions**：按存储库文件夹分组显示智能体会话。选择存储库名称（例如 `tailspin-toys`）打开存储库视图，或选择其下的会话返回对话。每个会话都在独立工作区运行，因此可以同时运行多个会话，而不会让更改相互冲突。添加星级评分时会创建第一个会话。

接下来的研讨会会逐步介绍工作区的使用方式。

> [!TIP]
> 有疑问时可以询问 Copilot！如果不确定如何操作或某项功能是否可用，可以向 Copilot 提问，它会提供帮助。

### 查找预先创建的待办事项

大多数项目都有待办事项，Tailspin Toys 也不例外。现在查看从模板创建项目时自动生成的待办事项。

1. 在侧边栏的 **Sessions** 下选择 Tailspin Toys 存储库文件夹，而不是其下方的某个会话。然后在存储库顶部导航中选择 **Issues**。
2. 按标题查找以下议题，不要假定其议题编号：
   - Allow users to filter games by category and publisher
   - Update our repository coding standards
3. 选择议题阅读详情。每个议题也都可以作为启动智能体会话的入口。完成第一个小型更改后，将从筛选议题开始工作。

> [!NOTE]
> **Issues** 和 **Pull requests** 中的列表会自动筛选，只显示添加到 Copilot app 的存储库中的项目。若要查看其他存储库中的工作项，请将对应存储库添加到 app。

## 尝试快速聊天

快速熟悉 app 的方法之一是询问 app 自身的问题。**快速聊天**可以让你提问或头脑风暴，而无需创建分支或工作树，适合快速、临时的问题，不需要会话。

1. 在侧边栏中选择 **Chats** 旁的 **+**，开始新聊天。如果尚未显示 **Chats**，选择 **Sessions** 旁的 **+** 开始首次聊天；之后会显示 **Chats**。
2. 询问 app 如何使用自身会话：

   ```plaintext
   GitHub Copilot app 如何使用工作树？
   ```

3. 阅读对话视图中的回复。你会看到每个会话都在自己的隔离 Git 工作树中运行，因此可以并行运行多个智能体而不让更改相互冲突。可以继续对话，也可以随时开始新聊天。

## 总结和后续步骤

恭喜！你已安装 GitHub Copilot app、连接项目并熟悉工作区。你学会了如何：

- 安装 app 并登录 GitHub。
- 从 GitHub 存储库添加项目。
- 熟悉工作区并在 **Issues** 中查找预先创建的待办事项。
- 通过快速聊天提出临时问题。

接下来，[启动第一个智能体会话][next-lesson]，让它在游戏卡片上显示星级评分。

## 资源

- [关于 GitHub Copilot app][about-copilot-app]
- [开始使用 GitHub Copilot app][getting-started]
- [在 GitHub Copilot app 中使用智能体会话][agent-sessions]

[next-lesson]: ../2-add-star-rating/
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
[getting-started]: https://docs.github.com/copilot/how-tos/github-copilot-app/getting-started
[agent-sessions]: https://docs.github.com/copilot/how-tos/github-copilot-app/agent-sessions
[download-app]: https://gh.io/app
