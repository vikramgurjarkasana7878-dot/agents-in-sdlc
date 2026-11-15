---
title: "レッスン 7 - QA agent を作成して使う"
description: "issue の要件、quality-checks skill、Playwright MCP を組み合わせた QA custom agent を作成します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

`quality-checks` skill で自動チェックを実行し、Playwright MCP でブラウザーからフィルタリングの動作を確認しました。次は、それらの機能を明確な QA プロセスを持つ custom agent にまとめます。

このレッスンでは、次のことを行います。

- custom agent が instruction、skill、MCP tool と連携する仕組みを確認します。
- 再利用可能な品質保証プロファイルを作成して確認します。
- QA agent を選択し、フィルタリングの issue に照らして結果をレビューします。

## シナリオ

Tailspin Toys では、pull request (PR) を作成する前に、要件、コード品質、自動チェック、テスト カバレッジ、ブラウザーでの動作を一貫した方法でレビューしたいと考えています。custom agent を使うと、この QA プロセスを調整し、再利用可能なレポートを作成できます。

## custom agent とは

custom agent は、Markdown プロファイルで定義する Copilot の専門的な役割です。プロファイルには、agent の目的、instruction、使用できる tool を記述します。このワークショップでは、`.github/agents/qa.agent.md` に QA の役割を定義し、Copilot CLI で選択します。

これまでに使ったカスタマイズには、それぞれ異なる役割があります。リポジトリの instruction はチームの標準を説明し、`quality-checks` skill は繰り返し実行するチェックをまとめます。Playwright MCP はブラウザー操作の tool を提供します。QA プロファイルは、それらを使って要件を評価し、結果を報告するよう Copilot に指示します。既存のカスタマイズを置き換えるものではなく、別の会話も必要ありません。

## QA プロファイルを作成する

機能の PR を開く前に、再利用可能な QA プロファイルの作成を Copilot に依頼します。実施するチェックと、その範囲も定義します。

1. Codespace に戻り、フィルタリングの会話が Interactive モードであることを確認します。
2. 既定のエージェントに、次の custom agent を作成するよう依頼します。

   ```plaintext
   Create a custom agent named QA in .github/agents/qa.agent.md. It should check features against their issues and agreed requirements, follow the repository instructions, run the quality-checks skill, use Playwright MCP to verify behavior, and add tests when coverage is missing.

   Have it report each requirement as pass, fail, or blocked with supporting evidence. It must ask before changing implementation code, and it must not commit changes or open pull requests.

   Just create the profile for now so I can review it.
   ```

## プロファイルを確認する

新しい agent を使う前に、Copilot が意図した QA の流れと制約をプロファイルに反映したことを確認します。こうすることで、検証だけを依頼した agent が機能を変更するなど、範囲のずれを防げます。

1. `/diff` を入力して `.github/agents/qa.agent.md` を開きます。
2. frontmatter を読みます。`description` は必須です。`name` は省略可能ですが、記述すると agent を識別しやすくなります。
3. プロファイルの instruction を読み、QA が要件を確認し、リポジトリの instruction に従い、`quality-checks` skill と Playwright MCP を利用することを確認します。
4. QA が根拠を示して報告し、実装コードを変更する前に確認を求め、変更を commit したり PR を開いたりしないことを確認します。
5. これらの役割や制約が抜けていた場合は、続ける前に既定の agent に修正を依頼します。

## issue を基に QA を実行する

Copilot CLI は、会話の開始時にプロジェクトの agent を読み込みます。プロファイルを作成した後、同じフィルタリングの会話を再開し、QA を選択すると、issue と計画の内容を引き続き利用できます。

1. 次の prompt で agent を選択します。

   ```plaintext
   /agent QA
   ```

> [!NOTE]
> agent を作成した直後は、利用可能な一覧に表示されない場合があります。その場合でも、上記のコマンドで有効にできます。

2. 次の prompt で、QA agent に機能のレビューを依頼します。

   ```plaintext
   Review the filtering feature against the issue and the decisions in our plan. Is it ready for a PR?
   ```

3. QA agent が作業を進める様子を確認します。
4. 完了したら、QA agent のレポートを読みます。

## まとめと次のステップ

再利用可能な専門的役割をワークフローに追加し、その成果をレビューしました。このレッスンでは、次のことを行いました。

- custom agent が instruction、skill、MCP tool と連携する仕組みを確認しました。
- 再利用可能な品質保証プロファイルを作成して確認しました。
- QA agent を選択し、フィルタリングの issue に照らして結果をレビューしました。

実装、skill の更新、QA プロファイル、テスト、検証レポートが揃いました。次は、それらを[機能の PR にまとめ、Agent Merge を使います][next-lesson]。

## リソース

- [Copilot CLI 用 custom agent の作成][create-agents]
- [custom agent の設定][agent-config]

[previous-lesson]: ../6-mcp-playwright/
[next-lesson]: ../8-create-pull-request/
[create-agents]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli
[agent-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
