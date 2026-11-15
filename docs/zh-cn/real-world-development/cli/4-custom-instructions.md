---
title: "第 4 课 - 使用自定义说明引导 Copilot"
description: "探索存储库说明，添加文档标准，并将其应用于筛选代码。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

上下文对生成式 AI 至关重要。如果任务需要以特定方式完成，应确保 Copilot 能获取相应指导。[说明文件][instruction-files]不仅描述需要什么代码，还描述代码应如何组织。现在筛选功能已经构建完成，你将探索 Copilot 使用的说明、添加文档标准，并将其应用于代码。

本课将介绍如何：

- 了解存储库说明和按路径限定范围的说明文件如何传递给智能体。
- 更新说明文件，确保遵循编码标准。
- 查看说明文件对代码的影响。

## 场景

和优秀的开发团队一样，Tailspin Toys 也有一套开发实践准则和要求，包括：

- 注释应解释意图和不明显的决策，而不是复述代码。
- `db/` 和 `src/lib/` 中导出的函数应使用 TSDoc/JSDoc 说明用途、参数和返回值；如果有可注入的 `db` 参数，也应予以说明。
- 可复用的 Astro 组件应记录 `Props` 契约，并在相关代码发生变化时同步更新注释。
- 保留现有格式和 lint 指南。

使用说明文件，可以确保 Copilot 获得正确的信息，并按照这些实践完成任务。

## 说明文件

自定义说明可为 Copilot 提供上下文和偏好，帮助它理解你的编码风格和需求。你可以指定编码约定、库，甚至希望代码包含的注释类型。可以为整个存储库创建说明，也可以针对特定文件类型提供任务级上下文。

说明文件分为两类：

- `.github/copilot-instructions.md`：每次针对该存储库的请求都会发送给 Copilot 的单个说明文件。它应包含项目级信息，即与大多数 Copilot 聊天或 CLI 请求相关的上下文，例如技术栈、构建内容概述、最佳实践和其他全局指导。
- `.github/instructions/*.instructions.md`：可针对特定任务或文件类型创建，为特定语言（如 TypeScript 或 Astro）或创建 UI 组件、新增单元测试等任务提供指导。

> [!NOTE]
> 不同环境支持的其他说明格式可能有所不同。依赖某种格式前，请查阅[自定义说明支持参考][custom-instructions-support]。

## 探索此项目中的自定义说明文件

初始项目已包含一组说明文件。进行更改前，先探索现有文件并了解其影响。

1. 返回 Codespace。
2. 在 Codespaces 编辑器（不是终端）中打开 `.github/copilot-instructions.md`。
3. 浏览文件，了解项目简介及编码指导。这些说明适用于此存储库中与 Copilot 的每次交互。
4. 打开 `.github/instructions` 文件夹并查看其中的文件。注意文件包括 Astro、Drizzle 数据层、测试等方面的说明。
5. 打开 `.github/instructions/unit-tests.instructions.md`。注意顶部的 `applyTo` 字段，它设置了决定说明适用文件的 glob。此处会匹配任何 TypeScript 测试文件，例如符合 `**/*.test.ts` 的文件。
6. 打开 `.github/instructions/drizzle.instructions.md`，查看其中指向其他说明文件和项目现有文件的链接。这可以将较大的说明集拆分成更小的可复用文件。

## 更新说明文件以符合团队指南

现有文件是不错的起点，但仍缺少一项标准。下面修改核心 `copilot-instructions.md`，确保新生成的 TypeScript 添加 [TSDoc 注释][tsdoc]。

1. 在 `.github/copilot-instructions.md` 中找到 **Code formatting guidance** 部分，约在第 35 行。
2. 在该部分末尾添加以下项目：

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

文件会自动保存。

## 使用更新后的指导

Copilot CLI 会在对话启动时加载存储库说明。保存更新后，退出并恢复筛选对话，让 Copilot 在保留功能上下文的同时加载新指导。

1. 如果筛选对话仍打开，请输入 `/exit` 关闭。Copilot CLI 会自动保存对话。
2. 在 `game-filters-cli` 分支的存储库根目录运行以下命令，恢复已保存的对话：

   ```bash
   copilot --resume
   ```

   如果系统提示，请选择筛选对话，并确认当前检出的是 `game-filters-cli` 分支。
3. 要求 Copilot 应用更新后的指导：

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. 输入 `/diff`，查看更改的 TypeScript 文件。注意新生成的 TSDoc 注释，并确认其准确说明了代码。

## 总结和后续步骤

你已探索 Copilot 如何从说明文件获取上下文，并将新标准应用于功能。具体来说，你：

- 探索了存储库说明和按路径限定范围的 `*.instructions.md` 文件如何传递给智能体。
- 更新了说明文件，确保遵循编码标准。
- 查看了说明文件对代码的影响。

接下来，你将[自定义并运行可复用的 quality-checks 技能][next-lesson]，确保一致地运行 lint 和测试。

## 资源

- [为 GitHub Copilot 添加自定义说明][instruction-files]
- [自定义说明支持][custom-instructions-support]
- [创建自定义说明的最佳实践][instructions-best-practices]
- [Awesome Copilot：说明文件及其他资源集合][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
[tsdoc]: https://tsdoc.org/
