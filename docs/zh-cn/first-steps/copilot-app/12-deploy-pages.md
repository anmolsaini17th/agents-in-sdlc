---
title: "第 12 课 - 使用 GitHub Pages 部署网站"
description: "使用 GitHub Copilot app 配置 Pages 工作流、审查拉取请求，并验证 Space Quiz 已上线。"
authors:
  - jamesmontemagno
lastUpdated: 2026-10-12
---

使用 GitHub Pages 部署 Space Quiz，为它提供公开访问入口。Copilot 可以准备工作流并引导部署，但仍需审查更改并确认网站已上线。

本课将学习如何：

- 让 Copilot 配置 GitHub Pages 部署。
- 在合并前审查工作流和拉取请求。
- 跟进部署并验证已发布的网站。

## 场景

Space Quiz 已经位于 GitHub 存储库中，但用户还需要一种简单的方式来试用它。使用 GitHub Pages 发布这个单文件网站，无需添加服务器或运行时依赖，即可通过公开 URL 访问。

## 让 Copilot 部署网站

在 Copilot app 的 `space-quiz` 会话中，发送以下提示：

```plaintext
为此存储库启用 GitHub Pages。如果需要，请创建 GitHub Actions 工作流来构建网站并部署到 GitHub Pages。如果存储库中尚无 README，请添加一个，介绍应用以及如何在本地运行。使用这些更改创建拉取请求，并在必需检查通过后合并。持续监控部署，直到成功，然后报告在线网站的最终 URL。如果你没有启用 Pages 或合并的权限，请明确告诉我需要批准或执行什么操作；网站上线前不要报告成功。
```

![三阶段示意图：已合并的 space-quiz 拉取请求；成功运行的 GitHub Actions Deploy website 工作流，其中包含 Build website、Upload artifact 和 Deploy to Pages 步骤；以及位于 yourname.github.io/space-quiz/ 的在线 Space Quiz。](../../../_images/first-steps-app-pages-deploy.svg)

> [!WARNING]
> 此提示要求智能体创建并合并拉取请求，以及更改存储库的 Pages 设置。继续操作前，请审查差异、部署权限和 GitHub 的审批提示。

## 审查工作流并验证部署

1. 确认只有在存储库尚无 README 时才添加 README。
2. 审查拉取请求中的更改，包括工作流的构建和部署步骤。
3. 只有在所需的拉取请求检查通过后才合并。
4. 跟进 GitHub Actions 部署运行，直到成功。
5. 打开报告中的 URL，确认 Space Quiz 可以加载。

如果智能体无法启用 Pages 或合并拉取请求，请按其提供的具体说明操作。只有在部署成功且网站可以访问后，才算完成任务。

## 总结和后续步骤

现在，Space Quiz 已作为网站上线；你也亲自验证了部署，而不是只依赖智能体的报告。继续学习[第 13 课：审查和后续步骤][next-lesson]。

[next-lesson]: ../13-review/
