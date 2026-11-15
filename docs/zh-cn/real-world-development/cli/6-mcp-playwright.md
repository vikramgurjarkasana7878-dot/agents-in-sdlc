---
title: "第 6 课 - 使用 Playwright MCP 验证功能"
description: "在 Copilot CLI 中检查或配置 Playwright MCP，并在浏览器中探索筛选功能。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

如前所述，编写代码不只是写代码。我们还要处理数据和外部服务，并让 Copilot 使用更多自动化功能。这正是 MCP 服务器的用武之地。MCP 服务器可以为 Copilot CLI 提供更多内置工具和服务。

本课将介绍如何：

- 了解模型上下文协议 (MCP) 及 Copilot CLI 如何使用它。
- 在 Playwright MCP 服务器尚未可用时添加它。
- 要求智能体操控浏览器并探索筛选功能。

## 场景

单元测试和端到端测试很重要，但验证 UI 更新还需要实际与 UI 交互。你希望 Copilot 能像用户一样使用正在开发的网站，进一步自动化更改，并提高对更新符合预期的信心。

## 什么是模型上下文协议 (MCP)？

[模型上下文协议 (MCP)][mcp-blog-post] 为 AI 智能体提供了与外部工具和服务通信的方式。借助 MCP，AI 智能体可以实时连接外部工具和服务，从而获取最新信息并代表你执行操作。

AI 智能体通过 MCP 服务器访问这些工具和资源。MCP 服务器是智能体与外部工具及服务之间的桥梁，每个服务器代表智能体可以访问的一组工具和资源。

常见的 MCP 服务器包括：

- **[GitHub MCP Server][github-mcp]**：提供用于管理 GitHub 存储库、issue 和拉取请求的 API。
- **[Playwright MCP Server][playwright-mcp-server]**：借助 Playwright 提供浏览器自动化能力。

还有许多其他 MCP 服务器可供使用。GitHub 托管了 [MCP 注册表][mcp-registry]，以提升生态系统的可发现性并促进贡献。

> [!CAUTION]
> 应像对待项目中的其他依赖项一样对待 MCP 服务器。使用前请仔细审查其源代码、验证发布者并考虑安全影响。只使用可信的 MCP 服务器，并谨慎授予其访问敏感资源或执行敏感操作的权限。

## 添加 Playwright MCP 服务器

如果 Playwright MCP 服务器尚不可用，请添加它，让 Copilot 能像用户一样操作网站。

> [!NOTE]
> 如果 Copilot CLI 已配置 Playwright MCP，请跳过这些设置步骤，直接继续下一节。

1. 返回 Codespace。
2. 在 Copilot CLI 中输入以下命令，打开添加 MCP 服务器对话框：

   ```plaintext
   /mcp add
   ```

3. 名称输入 `playwright`，然后按 <kbd>Tab</kbd>。
4. 按 <kbd>Enter</kbd> 确认服务器类型为 **STDIO**，然后按 <kbd>Tab</kbd>。
5. 在 **Command** 对话框中粘贴以下命令：

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. 按 <kbd>Control</kbd>+<kbd>S</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>S</kbd>（Windows/Linux）保存新的 MCP 服务器。
7. 按 <kbd>Esc</kbd> 退出 MCP 对话框。

## 要求 Copilot 使用 Playwright 探索功能

之前你已手动确认功能行为符合预期。现在使用 Copilot 中的 Playwright MCP 服务器完成同样的检查。

1. 使用以下提示词，要求 Copilot 通过 Playwright MCP 验证功能：

   ```plaintext
   Start the app and use Playwright MCP to check filtering against the issue and our plan. Tell me what works and what doesn't, without making changes. Stop the server you started when you're done.
   ```

> [!NOTE]
> 通常不必明确指定要使用的 MCP 服务器，Copilot 会自行判断。不过，明确指出你认为重要的信息有助于获得更一致的结果，也能减少一些 token 消耗。

2. 观察 Copilot 在浏览器中执行的步骤。
3. 阅读报告，确认各项行为符合预期。

Copilot 会启动服务器、通过 Playwright 操作网站、停止服务器并提供报告。

## 总结和后续步骤

恭喜，你已通过 Copilot CLI 使用 Playwright MCP 服务器在真实浏览器中探索功能。本课中，你：

- 了解了模型上下文协议 (MCP) 及 Copilot CLI 如何使用它。
- 在 Playwright MCP 服务器尚不可用时添加了它。
- 要求智能体操控浏览器并探索筛选功能。

接下来，在[创建 QA 自定义智能体][next-lesson]时，将技能和浏览器工具结合起来。

## 资源

- [MCP 到底是什么，为什么大家都在讨论它？][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [向 Copilot CLI 添加 MCP 服务器][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers
