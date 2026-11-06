---
title: "练习 3 - 使用 Playwright MCP 服务器测试功能"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：智能体模式][previous-lesson] · [下一课：自定义智能体 →][next-lesson]

你刚才使用智能体模式构建了筛选功能。创建拉取请求前，应确认它在浏览器中正常运行。此处无需手动浏览应用，而是连接 **Playwright MCP 服务器**，让 Copilot 驱动真实浏览器测试功能，然后发布分支并创建 PR。

本练习将学习如何：

- 了解 Model Context Protocol (MCP) 以及 MCP 服务器如何通过新工具扩展 Copilot。
- 将 Playwright MCP 服务器添加到工作区。
- 让 Copilot 使用该服务器在浏览器中手动测试筛选功能。
- 发布分支并为筛选工作创建拉取请求。

## 什么是 Model Context Protocol (MCP)？

智能体模式连接到编辑器以外的工具后，功能会更强大。Copilot 使用 Model Context Protocol (MCP) 与外部工具和服务通信。

![智能体模式内部工作方式示意图，显示它如何与上下文、LLM 和工具交互，其中包括 MCP 服务器和 VS Code 扩展提供的工具](../../../_images/ex1-mcp-diagram.png)

[Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) 让 AI 智能体能够与外部工具和服务通信。使用 MCP，AI 智能体可以实时获取最新信息（通过资源）并代表你执行操作（通过工具）。

这些工具和资源通过 MCP 服务器访问。MCP 服务器充当 AI 智能体与外部工具和服务之间的桥梁，负责管理智能体与外部工具（例如现有 API 或 NPM 本地工具包）之间的通信。每个 MCP 服务器都代表 AI 智能体可访问的一组工具和资源。

常见的 MCP 服务器包括：

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**：提供用于管理 GitHub 存储库、议题和拉取请求的 API。
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)**：使用 Playwright 提供浏览器自动化功能。

此外还有许多其他 MCP 服务器。GitHub 提供 [MCP 注册表](https://github.com/mcp)，方便用户发现并参与生态系统。

> [!CAUTION]
> 安全方面应将 MCP 服务器视作项目中的其他依赖项。使用前请仔细审查其源代码、验证发布者并考虑安全影响。只使用可信的 MCP 服务器，并谨慎授予敏感资源或操作的访问权限。

## 查看 MCP 配置

`.vscode/mcp.json` 文件用于配置 VS Code 工作区中可用的 MCP 服务器。

1. 在 Codespace 中打开 `.vscode/mcp.json`。
2. 你会看到已配置的 `github` 服务器：

   ```json
   {
     "servers": {
       "github": {
         "type": "http",
         "url": "https://api.githubcopilot.com/mcp/"
       }
     }
   }
   ```

项目模板自带此 `github` 配置，因此 Copilot 能在上一课读取待办事项。它使用[远程 GitHub MCP 服务器][remote-github-mcp-server]，无需本地安装；VS Code 通过 HTTP 连接，并在 Copilot 首次使用其工具时使用 GitHub 身份验证。

## 添加 Playwright MCP 服务器

接下来添加第二个服务器。[Playwright MCP 服务器][playwright-mcp-server]为 Copilot 提供可控的浏览器，用于测试筛选功能。

1. 在 `.vscode/mcp.json` 中添加 `playwright` 配置，与 `github` 并列：

   ```json
   {
     "servers": {
       "github": {
         "type": "http",
         "url": "https://api.githubcopilot.com/mcp/"
       },
       "playwright": {
         "command": "npx",
         "args": ["@playwright/mcp@latest", "--headless"]
       }
     }
   }
   ```

2. 保存文件。

与远程 `github` 服务器不同，`playwright` 是**本地**服务器：VS Code 会在本机运行 `npx @playwright/mcp@latest`。`--headless` 参数使 Playwright 在没有可见窗口的情况下运行浏览器，这对于没有桌面环境的 Codespace 是必要的。

> [!NOTE]
> Tailspin Toys 项目已使用 Playwright 运行端到端测试，因此通常已安装 Playwright 所需的浏览器。如果 Copilot 报告缺少浏览器，请让它运行 `npx playwright install chromium`，然后重试。

## 启动并信任服务器

VS Code 会在需要时启动 MCP 服务器。Copilot 首次需要服务器工具时，VS Code 会启动服务器并询问你是否信任它。

1. 在 `.vscode/mcp.json` 中，`playwright` 配置上方会显示 **Start** 操作。选择它立即启动服务器，确认配置正确。
2. 如果 VS Code 询问是否信任服务器，请审查配置并选择信任，以便启动服务器。

也可以不手动启动；如果跳过此步骤，VS Code 会在下一步 Copilot 首次需要该工具时启动服务器，并询问是否信任。

## 测试筛选功能

[Playwright MCP 服务器][playwright-mcp-server]提供真实浏览器。无需亲自点击浏览应用来检查工作，智能体可以打开页面、导航、应用筛选条件、读取结果并总结观察到的情况。这样可以快速确认功能是否符合预期，无需离开当前会话。

Playwright MCP 服务器会读取页面的[无障碍树][playwright-mcp-server]，而不是截图。因此，智能体会根据结构化的带标签元素（按钮、链接和列表项）进行判断，就像辅助技术一样。快速功能检查也可以顺便验证基本无障碍情况。

服务器已连接且应用正在运行后，提示 Copilot 测试刚完成的筛选功能：

```text
使用 Playwright MCP 服务器打开浏览器，访问正在运行的应用 http://localhost:4321，并验证新的游戏筛选功能：

1. 打开游戏页面并记录列出的游戏数量。
2. 应用类别筛选，确认列表仅显示该类别的游戏。
3. 清除筛选，再应用发行商筛选，确认列表仅显示该发行商的游戏。
4. 同时应用类别和发行商筛选，确认结果同时符合两个条件。

报告每一步的观察结果，并指出任何行为异常之处。
```

Copilot 会通过 Playwright MCP 服务器启动浏览器、执行上述步骤并报告结果。根据议题中的验收标准审查总结；如有问题，可以继续提问，或要求它在创建 PR 前修复代码。

> [!NOTE]
> 测试要求应用正在 `http://localhost:4321` 运行。如果已停止开发服务器，请先重启。Copilot 首次使用 Playwright MCP 服务器时，可能需要下载浏览器；如果报告缺少浏览器，请让它运行 `npx playwright install chromium`，然后重试。

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## 发布分支并创建拉取请求

确认功能正常后，就可以创建拉取请求（PR）供团队审查。首先发布 `filtering-vscode` 分支。

1. 在 Codespace 中打开 **Source Control** 面板，审查 Copilot 所做的更改。
2. 选择 **+** 图标暂存更改。
3. 使用 **Sparkle** 按钮生成提交消息。

   ![Source Control 面板中的更改列表](../../../_images/ex3-source-control-changes.png)

4. 选择 **Publish**，将分支推送到存储库。

## 创建拉取请求

创建 PR 的方式有很多，包括使用 github.com 和 GitHub 命令行界面（CLI）。现在可以让 GitHub Copilot 查找相关议题并创建关联 PR。

1. 打开 Copilot Chat 面板并选择 **New Chat**，开始新会话。
2. 确认选择了 **Agent**，以便 Copilot 使用 GitHub 工具。
3. 提示 Copilot 创建 PR：

   ```plaintext
   查找存储库中与按类别和发行商筛选相关的议题。从当前分支创建新的拉取请求，并将其关联到正确的议题。
   ```

4. 根据需要选择 **Continue**，允许 Copilot 执行收集信息和操作所需的任务。首次使用 GitHub 工具时，可能会提示你进行身份验证，请按照提示操作。
5. 留意 Copilot 如何搜索议题、找到正确议题并创建 PR。
6. 选择 Copilot 提供的链接审查拉取请求，但**暂时不要合并**。

## 总结和后续步骤

恭喜你！本练习中你：

- 了解了 Model Context Protocol (MCP) 以及 MCP 服务器如何通过新工具扩展 Copilot。
- 将 Playwright MCP 服务器添加到工作区。
- 使用该服务器在浏览器中手动测试筛选功能。
- 发布分支并为筛选工作创建拉取请求。

你使用 MCP 服务器测试了功能，但 MCP 只是扩展 Copilot 的一种方式。接下来，[创建自定义智能体][next-lesson]，简化无障碍审查等专门任务。

## 资源

- [MCP 是什么，为什么大家都在讨论它？][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [GitHub MCP Server][github-mcp-server]
- [GitHub MCP 注册表][mcp-registry]
- [VS Code 中的 MCP 服务器][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
