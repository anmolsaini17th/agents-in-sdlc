---
title: "第 4 课 - 使用自定义指令引导 Copilot"
description: "探索存储库指令，添加文档标准，并将其应用于筛选代码。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

使用生成式 AI 时，上下文至关重要。如果任务需要按照特定方式完成，就应让 Copilot 能获取相应指导。[指令文件][instruction-files]不仅描述想要什么代码，也说明代码应如何组织。构建完筛选功能后，现在来了解 Copilot 使用的指令、添加文档标准并将其应用于代码。

本课将学习如何：

- 了解存储库指令和按路径限定的指令文件如何传递给智能体。
- 更新指令文件，确保遵循编码标准。
- 观察指令文件对代码的影响。

## 场景

Tailspin Toys 有一套开发指南和要求，包括：

- 注释应解释意图和不明显的决策，而不是复述代码。
- `db/` 和 `src/lib/` 中导出的函数应使用 TSDoc/JSDoc 说明用途、参数和返回值，并为适用的情形记录可注入的 `db` 参数。
- 可复用 Astro 组件应说明其 `Props` 接口；代码更改时应同步更新注释。
- 保留现有格式和 lint 指南。

使用指令文件可以确保 Copilot 按照这些实践完成任务。

## 指令文件

自定义指令可以提供上下文和偏好，帮助 Copilot 理解编码风格和要求。可以指定编码规范、库以及注释风格等偏好，为整个存储库或特定文件类型创建指令。

指令文件有两种：

- `.github/copilot-instructions.md`：每次针对该存储库的请求都会发送给 Copilot 的单个文件，内容应是适用于大多数请求的项目级信息。
- `.github/instructions/*.instructions.md`：针对特定语言、文件类型或任务提供指南。

> [!NOTE]
> 其他指令格式及其支持情况因环境而异。依赖特定格式前，请查阅[自定义指令支持参考][custom-instructions-support]。

## 探索本项目的自定义指令文件

项目中已包含一组指令文件。先探索它们，再进行更改，以便观察效果。

1. 返回 Codespace。
2. 在终端以外的 Codespaces 编辑器中打开 `.github/copilot-instructions.md`。
3. 查看项目简介和编码指南。该文件适用于本存储库中的每次 Copilot 交互。
4. 打开 `.github/instructions` 文件夹并查看其中的文件，包括 Astro、Drizzle 数据层、测试等方面的指令。
5. 打开 `.github/instructions/unit-tests.instructions.md`。注意顶部的 `applyTo` 字段，它使用 glob 选择适用文件。
6. 打开 `.github/instructions/drizzle.instructions.md`，查看它指向其他指令文件和现有项目文件的链接。通过这种方式，可以拆分较大的指令集并复用内容，也可以为 Copilot 提供参考示例。

## 更新指令以符合团队指南

现有文件已经提供良好起点，但仍缺少一项标准：需要修改核心 `copilot-instructions.md`，要求为新生成的 TypeScript 添加 TSDoc 注释。

1. 在 `.github/copilot-instructions.md` 的 **Code formatting guidance** 部分（大约第 35 行）找到最后一条规则。
2. 添加以下项目：

   ```markdown
   - 所有新编写的 TypeScript 都应添加 TSDoc 注释作为代码文档。
   ```

3. 保存文件。

## 使用更新后的指引

Copilot CLI 会在对话启动时加载存储库指令。保存后，退出并恢复筛选会话，使 Copilot 加载新指令，同时保留功能上下文。

1. 如果筛选对话仍在运行，输入 `/exit` 关闭。Copilot CLI 会自动保存会话。
2. 在存储库根目录的 `game-filters-cli` 分支上恢复已保存的会话：

   ```bash
   copilot --resume
   ```

   如果系统提示，选择筛选会话并确认检出的是 `game-filters-cli` 分支。
3. 让 Copilot 应用更新后的指引：

   ```plaintext
   我们刚刚更新了指令和代码指南。请将已生成的代码更新为符合这些指南。
   ```

4. 输入 `/diff` 并查看已更改的 TypeScript 文件。注意新生成的 TSDoc 注释，并确认其准确说明了代码。

## 总结和后续步骤

你探索了 Copilot CLI 如何读取指令文件，并将新标准应用于功能。具体来说，你：

- 了解存储库指令和按路径限定的指令文件如何传递给智能体。
- 更新指令文件，确保遵循编码标准。
- 观察指令文件对代码的影响。

接下来，[自定义并运行可复用的 quality-checks 技能][next-lesson]，确保一致地运行 lint 和测试。

## 资源

- [为 Copilot CLI 添加自定义指令][instruction-files]
- [自定义指令支持][custom-instructions-support]
- [创建自定义指令的最佳实践][instructions-best-practices]
- [Awesome Copilot：指令文件及其他资源集合][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
