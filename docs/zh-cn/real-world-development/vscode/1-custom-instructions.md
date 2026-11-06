---
title: "练习 1 - 自定义指令（VS Code）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：先决条件][previous-lesson] · [下一课：智能体模式 →][next-lesson]

使用生成式 AI 时，上下文至关重要。如果任务有特定做法，或者 Copilot 需要了解一些背景信息，就应确保它能获取这些上下文。[指令文件][instruction-files]是提供代码生成指导的主要方式。

在本练习中，你将：

- 了解如何通过存储库自定义指令和按路径限定的指令文件，将项目上下文、编码指南和文档标准传递给 Copilot。
- 使用当前指令生成筛选功能的第一个数据切片（发行商辅助函数）。
- 在 `.github/copilot-instructions.md` 中添加新的存储库级标准。
- 再次运行相同提示，观察生成的代码如何应用新标准。
- 将指令更新和筛选功能切片提交到 `main`，供下一课中的 Copilot 使用。

> [!CAUTION]
> 生成的代码可能不符合你设定的某些标准。Copilot 的结果并非确定性输出。本练习旨在观察更新指令前后的行为趋势，而不是逐字匹配某种输出。

## 指令文件

### 场景

Tailspin Toys 有一套开发实践指南和要求，包括：

- 数据层始终需要单元测试。
- UI 应使用暗色模式并具有现代风格。
- 应通过 TSDoc 文档注释为代码添加说明。
- 每个文件开头都应有一段注释，描述文件的用途。

通过指令文件，可以让 Copilot 获得正确的信息，并按照这些实践开展工作。

### 自定义指令

自定义指令可向 Copilot 提供上下文和偏好，帮助它理解你的编码风格和要求。你可以指定编码规范、库以及注释风格等偏好，为整个存储库或特定文件类型创建指令。

指令文件有两种：

- `.github/copilot-instructions.md`：每次针对该存储库的请求都会发送给 Copilot 的单个文件。内容应是适用于大多数请求的项目级信息，例如技术栈、项目概述、最佳实践和全局指南。
- `.github/instructions/*.instructions.md`：可针对具体任务或文件类型提供指南，例如 TypeScript、Astro、UI 组件或单元测试。

> [!NOTE]
> 在 IDE 中，指令文件只用于 Copilot Chat 的代码生成，不用于代码补全或下一步编辑建议。
>
> Copilot Chat、Copilot CLI 和 Copilot 云智能体在生成代码时都会使用存储库级指令文件，以及带有 `applyTo` frontmatter 的 `*.instructions.md` 文件。
>
> Copilot 还支持其他标准的指令文件，例如 AGENTS.md 和 CLAUDE.md。

### 管理指令文件的最佳实践

- 将 `copilot-instructions.md` 限定为项目级指南，例如项目说明、项目结构和全局编码标准。
- 使用 `*.instructions.md` 为文件类型（例如单元测试、Astro 组件和数据层）或特定任务提供专门指令。
- 使用自然语言，保持说明清楚，并提供正反示例。

指令文件没有唯一正确的写法，AI 的使用方式也不止一种。可以通过试验找到适合项目和场景的做法。

> [!TIP]
> 每个使用 GitHub Copilot 的项目都应有一套完善的指令文件。浏览本项目中的文件时，你可能会看到针对 [UI 更新][ui-instructions]和 [Astro][astro-instructions]等任务的指令。
>
> Copilot 也可以帮助生成指令文件。不同产品界面的操作各不相同，例如 VS Code 中的 **Configure Chat → Generate Agent Instructions**，或 Copilot CLI 中的 `/init`。对应产品的课程会在相关步骤中介绍。
>
> 需要模板或起点？可以查看 [awesome-copilot][awesome-copilot]，其中包含大量指令文件、自定义智能体和其他资源。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## 浏览本项目的自定义指令文件

先阅读存储库自带的指令文件：一个核心 `copilot-instructions.md` 文件，以及一组用于不同任务的 `*.instructions.md` 文件。可以在编辑器或 GitHub Web 界面中打开这些文件。

1. 打开 `.github/copilot-instructions.md`。
2. 查看项目简介，以及 **Agent notes**、**Code standards**、**Scripts** 和 **Repository Structure** 等部分。注意 **Code standards** 下的 **GitHub Actions Workflows** 指南。
3. 打开 `.github/instructions` 文件夹，查看其中针对 Astro、Drizzle 数据层、测试等内容的指令。
4. 打开 `.github/instructions/unit-tests.instructions.md`。注意 `applyTo` 字段，它设置了相对于存储库根目录的 glob；例如，匹配 `**/*.test.ts` 的 TypeScript 测试文件会应用这些指令。
5. 查看该文件中针对本项目单元测试的说明。
6. 打开 `.github/instructions/drizzle.instructions.md` 并滚动到底部。注意其中指向其他指令文件（如 `unit-tests.instructions.md`）和项目现有文件的链接。通过这种方式，可以拆分较大的指令集并复用内容，也可以为 Copilot 提供参考示例。（这些路径是相对于指令文件，而不是存储库根目录。）

> [!NOTE]
> `copilot-instructions.md` 中的 **Code formatting requirements** 部分介绍了项目编码标准，但目前还没有要求添加代码文档。接下来会添加 TSDoc 文档注释和文件注释头的规则。

## 为更改创建分支

遵循最佳实践，为本次更改创建分支。

1. 返回上一课使用的 Codespace。
2. 按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux）打开终端。
3. 创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions
   ```

## 更新指令前先使用 Copilot Chat

> [!TIP]
> **打开 Copilot Chat**
>
> 开始以下练习前，返回 Codespace，打开 Copilot Chat 面板并选择 **New Chat**，开始一个干净的会话。模式和模型选择因练习而异；相关步骤会在需要时说明。

先在当前指令文件下发送提示，观察自定义指令的效果；之后更新指令并再次发送相同提示。

1. 关闭之前练习中打开的编辑器选项卡，避免 Copilot 获取不必要的上下文。
2. 打开 `src/lib/publishers.ts`，让 Copilot 知道辅助函数应放在哪里。
3. 在 Chat 视图的智能体下拉菜单中选择 **Agent**，以便 Copilot 修改文件。

   ![Chat 视图中的智能体选择器](../../../_images/shared-chat-mode-selector.png)

4. 发送以下提示：

   ```plaintext
   创建或更新 src/lib/publishers.ts，添加一个数据访问辅助函数，返回所有发行商的列表，每项包含名称和 ID。应用文件更改。
   ```

5. Copilot 会探索项目并应用代码更新，通常会同时修改辅助函数文件和测试。
6. 注意生成的辅助函数是一个带类型的函数，它将 `db` 客户端作为第一个参数，并返回带类型的发行商数组。这来自 `.github/instructions/drizzle.instructions.md` 中的数据层规范，该文件适用于 `src/lib/*.ts`。
7. 注意生成的代码**缺少** TSDoc 文档注释和文件级注释头。

> [!CAUTION]
> Copilot 的输出具有概率性，有时即使未指定也会添加文档注释。如果发生这种情况也没关系，本练习的重点是观察更新指令后的一致性改进。

## 添加新的存储库标准

`.github/copilot-instructions.md` 用于向 Copilot 提供项目级信息。接下来为存储库补充编码标准，以改进代码建议。

1. 重新打开 `.github/copilot-instructions.md`。
2. 找到 **Code formatting requirements** 部分（大约在第 27 行）。这里记录了项目编码标准，但尚未包含代码文档规则。
3. 在现有标准下方添加以下 Markdown 内容，要求 Copilot 添加文件注释头和 TSDoc 文档注释：

   ```markdown
   - 每个导出的函数都应包含 TSDoc 注释，说明其用途、参数和返回值。
   - 在导入语句或任何代码之前，添加注释块说明文件用途。
   ```

4. 保存 `copilot-instructions.md`。

> [!TIP]
> 指令文件既可放在存储库级别（`.github/copilot-instructions.md`）以提供全局指南，也可使用 `*.instructions.md` 为特定语言、文件类型或任务提供指令。项目级标准应放在存储库级文件中。

## 再次运行提示并观察变化

1. 返回 Copilot Chat 并选择 **New Chat**，清除会话上下文。
2. 再次打开 `src/lib/publishers.ts`，让 Copilot 关注正确文件。
3. 发送与之前相同的提示：

   ```plaintext
   创建或更新 src/lib/publishers.ts，添加一个数据访问辅助函数，返回所有发行商的列表，每项包含名称和 ID。应用文件更改。
   ```

4. 现在生成的文件开头应包含类似以下内容的注释块：

   ```typescript
   /**
    * Tailspin Toys 众筹平台的发行商数据访问辅助函数。
    * 提供从数据库检索发行商信息的函数。
    */
   ```

5. 生成的函数也应包含类似以下内容的 TSDoc 注释：

   ```typescript
   /**
    * 返回包含所有发行商 ID 和名称的列表。
    *
    * @param db - Drizzle 数据库客户端。
    * @returns 解析为发行商对象数组的 Promise。
    */
   ```

你刚才引导 Copilot 遵循新的项目标准，并将其应用到真实代码中；下一课将基于此继续开发。

## 提交指令并推送分支

指令文件与其他项目文件一样，通过源代码管理进行维护。接下来提交并推送分支。

1. 按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux）打开终端。
2. 运行以下命令，确认指令和辅助函数更改已存在：

   ```bash
   git status
   ```

3. 暂存并提交指令更新，以及 Copilot 为辅助函数基础所做的所有更改：

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # 如果 git status 显示还有其他支持性更改（例如测试），也一并添加。
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. 将分支推送到存储库：

   ```bash
   git push -u origin custom-instructions
   ```

## 创建并合并拉取请求

分支推送后，创建拉取请求，并关联待办事项中的文档标准议题。也可以让 Copilot 查找议题、创建关联 PR 并合并。

> [!NOTE]
> 这是第一次让 Copilot 代表你在 GitHub 上操作。项目模板已配置 GitHub 工具，无需额外设置。Copilot 首次使用工具时，VS Code 可能会提示你登录 GitHub。按照提示授权即可。后续课程会介绍此连接的工作方式（Model Context Protocol）。

1. 在 Codespace 中打开 Copilot Chat。
2. 按 <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd>（Windows/Linux）打开 Copilot Chat 视图，并确保智能体选择器中选中 **Agent**。
3. 让 Copilot 查找与指令文件有关的议题，并从当前分支创建 PR，说明指令更新和发行商辅助函数基础：

   ```plaintext
   查找与更新指令文件相关的议题。从当前分支创建新的 PR，说明该 PR 会关闭此议题，并注明我们还添加了供后续筛选工作使用的发行商辅助函数基础。
   ```

4. Copilot 将查找议题并创建 PR。
5. 当 Copilot 提示允许其代表你执行 GitHub 操作时，先审查命令，再酌情选择 **Allow**。
6. PR 创建后，提示 Copilot 将其合并到 `main`，然后在本地切回 `main` 并拉取最新代码：

   ```plaintext
   将 PR 合并到 main。然后在本地切回 main 并拉取最新代码，确保代码已更新。
   ```

7. 当 Copilot 提示允许其代表你执行 GitHub 操作和运行 shell 命令时，先审查命令，再酌情选择 **Allow**。

现在，你已借助 GitHub Copilot 创建并合并拉取请求。

## 总结和后续步骤

你了解了 Copilot 如何读取本项目的指令文件，并在 VS Code 中使用 Copilot Chat 完成以下任务：

- 发送代码生成提示，观察 Copilot 如何应用现有指令。
- 在 `.github/copilot-instructions.md` 中添加新的存储库级标准。
- 再次发送相同提示，观察生成代码如何应用新标准。
- 将指令更新和辅助函数基础提交到 `main`，供下一课继续开发。

接下来，在[智能体模式][next-lesson]中运用这些指令，让 Copilot 跨多个文件实现新功能。

## 资源

- [GitHub Copilot 自定义指令文件][instruction-files]
- [创建自定义指令的最佳实践][instructions-best-practices]
- [编写更好的 Copilot 自定义指令的 5 个技巧][copilot-instructions-five-tips]
- [GitHub Copilot 个人自定义指令][personal-instructions]
- [Awesome Copilot：指令文件及其他资源集合][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
