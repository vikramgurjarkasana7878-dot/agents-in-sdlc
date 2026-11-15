---
title: "演習 3 - Playwright MCP server で機能をテストする"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: Agent mode][previous-lesson] · [次のレッスン: Custom agents →][next-lesson]

agent mode でフィルタリング機能を作成しました。pull request を開く前に、ブラウザーで正しく動作することを確認します。自分でアプリを操作する代わりに、**Playwright MCP server** を接続し、Copilot に実際のブラウザーで機能をテストしてもらいます。その後、ブランチを公開して PR を作成します。

この演習では、次のことを行います。

- Model Context Protocol (MCP) とは何か、MCP server が Copilot に tool を追加する仕組みを学びます。
- Playwright MCP server を workspace に追加します。
- ブラウザーでフィルタリング機能を手動テストするよう Copilot に依頼します。
- ブランチを公開し、フィルタリング作業の pull request を作成します。

## Model Context Protocol (MCP) とは

agent mode は、エディターの外部にあるサービスにもアクセスできると、さらに便利になります。Copilot がそれを実現する方法が Model Context Protocol (MCP) です。エージェントが外部の tool やサービスと通信するための標準です。

![agent mode の内部構造と、コンテキスト、LLM、tool（MCP server や VS Code 拡張機能が提供する tool を含む）とのやり取りを示す図](../../../_images/ex1-mcp-diagram.png)

[Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) は、AI エージェントが外部の tool やサービスと通信するための仕組みです。MCP を使うと、AI エージェントはリソースを使って最新情報にアクセスしたり、tool を使ってユーザーに代わって操作したりできます。

これらの tool やリソースは、AI エージェントと外部の tool やサービスの橋渡しをする MCP server を介して利用します。MCP server は、既存の API や NPM package のようなローカル tool と AI エージェントとの通信を管理します。各 MCP server は、エージェントがアクセスできる別の tool やリソースのセットを提供します。

よく使われている MCP server の例を紹介します。

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**: GitHub リポジトリの管理 API を提供します。新しいリポジトリの作成、既存リポジトリの更新、issue や pull request の管理などをエージェントが行えます。
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)**: Playwright を使ったブラウザー自動化機能を提供します。Web ページへの移動、フォームの入力、ボタンの選択などをエージェントが行えます。

ほかにも、さまざまな tool やリソースを利用できる MCP server があります。GitHub は、エコシステム内の発見や貢献を促進するために [MCP registry](https://github.com/mcp) を公開しています。

> [!CAUTION]
> セキュリティの観点では、MCP server はプロジェクト内のほかの依存関係と同様に扱ってください。使用前にソース コードを慎重に確認し、公開元を検証して、セキュリティへの影響を検討します。信頼できる MCP server のみを使い、機密リソースや操作へのアクセスを許可する際は慎重に判断してください。

## MCP の設定を確認する

`.vscode/mcp.json` ファイルで、この VS Code workspace で利用できる MCP server を設定します。

1. Codespace で `.vscode/mcp.json` を開きます。
2. すでに `github` server が設定されています。

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

この `github` 設定はプロジェクト template に含まれています。そのため、前の演習で Copilot がバックログの issue を読み取れました。これは[リモート GitHub MCP server][remote-github-mcp-server] を使用します。ローカルにインストールする必要はなく、VS Code が HTTP 経由で接続し、Copilot が初めて tool を使うときに GitHub 認証を行います。

## Playwright MCP server を追加する

次に 2 つ目の server を追加します。[Playwright MCP server][playwright-mcp-server] は、Copilot が操作できるブラウザーを提供します。機能をテストするために利用します。

1. `.vscode/mcp.json` に `github` と並べて `playwright` の設定を追加し、ファイルを次のようにします。

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

2. ファイルを保存します。

リモートの `github` server とは異なり、`playwright` は**ローカル** server です。VS Code は `npx @playwright/mcp@latest` を実行して起動します。`--headless` フラグにより、Playwright は表示ウィンドウなしでブラウザーを実行します。デスクトップ環境がない Codespace では、この設定が必要です。

> [!NOTE]
> Tailspin Toys ではエンドツーエンド テストにすでに Playwright を使用しているため、必要なブラウザーは通常インストール済みです。Copilot からブラウザーがないと報告された場合は、`npx playwright install chromium` を実行して再試行してください。

## server を起動して信頼する

VS Code は必要に応じて MCP server を起動します。Copilot が初めて server の tool を使うとき、VS Code は server を起動し、信頼するか確認します。

1. `.vscode/mcp.json` で `playwright` 設定の上に **Start** 操作が表示されます。選択して server を起動し、設定が正しいことを確認します。
2. VS Code で server を信頼するか確認されたら、設定を確認して信頼することを選択し、起動を許可します。

手動での起動は省略できます。その場合は、次の手順で Copilot が初めて tool を使用する際に、VS Code が server を起動して信頼を確認します。

## フィルタリング機能をテストする

[Playwright MCP server][playwright-mcp-server] を使うと、Copilot が実際のブラウザーを操作できます。自分でアプリを操作する代わりに、agent がページを開き、移動し、filter を適用して、結果を読み取って報告できます。会話を離れずに機能を確認できます。

Playwright MCP server は、スクリーンショットではなくページの [accessibility tree][playwright-mcp-server] を使います。agent は、支援技術と同様に、button、link、list item など、構造化されラベル付けされた要素を扱います。簡単な機能確認が、アクセシビリティを大まかに確認する機会にもなります。

server を接続し、アプリを起動したら、次の prompt で Copilot にフィルタリング機能を試してもらいます。

```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

Copilot は Playwright MCP server でブラウザーを起動し、各手順を実行して結果を報告します。issue の受け入れ条件に照らして報告を確認してください。問題があれば、追加で質問するか、PR を開く前にコードの修正を依頼します。

> [!NOTE]
> このテストには、`http://localhost:4321` で実行中のアプリが必要です。dev server を停止していた場合は、prompt を送信する前に再起動してください。Copilot が初めて Playwright MCP server を使う際、ブラウザーをダウンロードする場合があります。見つからないと報告されたら、`npx playwright install chromium` を実行して再試行してください。

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## ブランチを公開して pull request を作成する

機能の動作を確認したので、チームがレビューできるよう pull request (PR) を作成します。まず `filtering-vscode` ブランチを公開します。

1. Codespace の **Source Control** パネルに移動し、Copilot が加えた変更を確認します。
2. **+** アイコンを選択して変更を stage します。
3. **Sparkle** ボタンで commit message を生成します。

   ![変更が表示された Source Control パネル](../../../_images/ex3-source-control-changes.png)

4. **Publish** を選択し、ブランチをリポジトリに push します。

## pull request を作成する

github.com や GitHub command-line interface (CLI) など、pull request を作成する方法はいくつかあります。GitHub Copilot を使っているので、PR の作成も依頼しましょう。Copilot は該当する issue を見つけ、PR と関連付けられます。

1. Copilot Chat パネルに移動し、**New Chat** を選択して新しいセッションを開始します。
2. agent の一覧から **Agent** を選択し、Copilot が GitHub tool を使えるようにします。
3. 次の prompt で PR 作成を依頼します。

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. Copilot が情報を集めて操作を行う際は、必要に応じて **Continue** を選択します。GitHub tool を初めて使うときに GitHub 認証を求められた場合は、案内に従ってください。
5. Copilot が issue を検索し、該当するものを見つけて PR を作成します。
6. Copilot が表示したリンクを選択して PR をレビューします。ただし、まだマージしないでください。

## まとめと次のステップ

この演習では、次のことを行いました。

- Model Context Protocol (MCP) とは何か、MCP server が Copilot に tool を追加する仕組みを学びました。
- Playwright MCP server を workspace に追加しました。
- 出荷前にブラウザーで機能をテストしました。
- ブランチを公開し、フィルタリング作業の pull request を作成しました。

MCP server を使って機能をテストしましたが、MCP は Copilot を拡張する方法の 1 つです。次は、アクセシビリティの確認などの作業を効率化する[custom agent を作成します][next-lesson]。

## リソース

- [MCP とは何か、なぜいま注目されているのか][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [GitHub MCP Server][github-mcp-server]
- [GitHub MCP Registry][mcp-registry]
- [VS Code の MCP server][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
