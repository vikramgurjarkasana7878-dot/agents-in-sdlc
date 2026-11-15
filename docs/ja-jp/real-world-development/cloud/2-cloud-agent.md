---
title: "演習 2 - GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: Custom instructions][previous-lesson] |
|:--|

技術的負債に悩んでいない組織は、ほとんどないでしょう。未解決のセキュリティ問題、更新が必要なレガシー コード、実装する時間がなくバックログに残った機能要望など、さまざまな課題があります。GitHub Copilot の cloud agent は、コードの更新や機能の追加などのタスクを自律的に実行できます。作業が完了すると、人間の開発者がレビューできる draft PR を作成します。面倒な作業を任せて開発プロセスを加速し、開発者がより大きな課題に集中できるようにします。

Copilot cloud agent を使って、次のことを確認します。

- コード生成環境のカスタマイズ。
- 操作を安全に行う方法。
- スコープが明確な issue の重要性。
- issue を Copilot に割り当てる方法。

## シナリオ

Tailspin Toys には解決したい技術的負債があります。最初のサイトを作成した委託先がドキュメントをほとんど残していません。まず、アプリケーションのすべての export 関数に TSDoc doc comment を追加します。

さらに、デザイン チームはゲームを見つけやすくしたいと考えています。各ゲームの詳細ページに、同じカテゴリのほかのゲームを表示すれば、ユーザーはサイトを引き続き閲覧できます。洗練された実装はまだ必要ありません。UX の受け入れテストに使えるものを求めています。この作業は現在のブロッカーですが、優先度がより高い issue もあります。

どちらも、優先度が下がりやすいタスクの例です。Copilot cloud agent に割り当てると、非同期で処理している間に別の作業に集中できます。その後で Copilot の成果を確認し、期待どおりかレビューできます。

## GitHub Copilot cloud agent とは

[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) は、人間の開発者と同様にバックグラウンドでタスクを実行します。[GitHub issue を Copilot に割り当てる](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions)など、複数の方法で作業を依頼できます。割り当てると、Copilot は進捗を追跡する draft pull request を作成し、環境を準備してタスクに取りかかります。実行中でも完了後でも、Copilot のセッションを確認できます。提案された解決策のレビュー準備ができると、pull request で通知されます。

## スコープが明確な instruction の重要性

GitHub Copilot に魔法はありません。考える作業を省略できるわけではありません。一見簡単な操作でも、詳しく調べると複雑なことがあります。[Copilot cloud agent に依頼するタスクのスコープ][cloud-agent-best-practices]を慎重に決めてください。AI ペア プログラマーとして扱い、段階的に作業し、学び、試し、必要に応じて調整しましょう。生成 AI を導入しても、ソフトウェア開発の基本は変わりません。

## このリポジトリの custom instruction

前の演習では custom instruction と Copilot への影響を確認しました。custom instruction の演習をすでに行っている場合は、`.github/copilot-instructions.md` が機能することを確認できています。まだの場合は、cloud agent に作業を割り当てる前に、リポジトリに用意された instruction file を読み取り専用で確認してください。後でその影響を見つけられます。

リポジトリの GitHub Web UI、または起動済みの Codespace で次のファイルを開きます。

- `.github/copilot-instructions.md` — **Code standards** セクション、特に TypeScript の規約と TSDoc doc comment の要件を確認します。
- `.github/instructions/unit-tests.instructions.md` — `applyTo` frontmatter を確認します。この instruction は `**/*.test.ts` に適用されます。

次のセクションで `Code lacks documentation` issue を cloud agent に割り当てると、作成された pull request に TSDoc doc comment、TypeScript の規約、コメント ヘッダーが反映されます。後の演習で PR をレビューする際にも確認します。

## Copilot cloud agent の開発環境を設定する

誰がコードを書く場合でも、適切な環境とセットアップ スクリプトが必要です。タスクを Copilot に割り当てる場合も同様です。Copilot はソフトウェア エンジニアのように作業します。

cloud agent は作業環境として [GitHub Actions][github-actions] を使います。`.github/workflows/copilot-setup-steps.yml` に設定した[専用セットアップ ワークフロー][setup-workflow]を作成すると、作業開始前に必要な開発 tool や依存関係をインストールできます。このラボでは、学習の流れを支えるために、すでに環境を構成してあります。単一の Astro app 用に、Node.js、プロジェクトの依存関係、エンドツーエンド テスト用のブラウザー バイナリ、マイグレーション済みかつ seed 済みの SQLite データベースを利用できます。

```yaml
name: "Copilot Setup Steps"

# Allows you to test the setup steps from your repository's "Actions" tab
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Set the permissions to the lowest permissions possible needed for *your steps*. Copilot will be given its own token for its operations.
    permissions:
      # If you want to clone the repository as part of your setup steps, for example to install dependencies, you'll need the `contents: read` permission. If you don't clone the repository in your setup steps, Copilot will do this for you automatically after the steps complete.
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Frontend / app setup - Node.js (the whole app is now Astro + Drizzle/libSQL)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # Migrate + seed the local SQLite database so builds and tests have data.
      - name: Set up the database
        run: npm run db:setup
```

一般的な GitHub workflow と同様ですが、いくつかポイントがあります。

- `copilot-setup-steps` という単一の job が含まれています。この job は、Copilot が pull request の作業を始める前に GitHub Actions で実行されます。
- `workflow_dispatch` トリガーを使うと、リポジトリの **Actions** タブから手動で workflow を実行できます。Copilot が実行するのを待たずに workflow の成功を確認できるため、設定のテストに便利です。

## ドキュメントを追加する

ドキュメントが重要なのは誰もが理解していますが、情報が古かったり、まったくなかったりするプロジェクトは少なくありません。こうした技術的負債は放置されやすく、生産性を下げ、コードベースの保守や新しいメンバーの参加を難しくします。Copilot はドキュメントの作成を得意としているため、cloud agent に割り当てるのに適した issue です。バックグラウンドでドキュメントを生成してもらい、後の演習で結果をレビューします。

1. 新しいブラウザー タブで github.com のリポジトリを開きます。
2. **Issues** タブを選択します。
3. **New issue** を選択して issue 作成画面を開きます。
4. **Blank issue** を選択します。
5. **Title** に `Code lacks documentation` と入力します。
6. **Description** に次のテキストを入力します。

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. **Create** を選択して issue を作成します。
8. 右側の **Assign to Copilot** を選択し、割り当てダイアログを開きます。

  ![issue への Copilot の割り当て](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。

  ![Copilot の割り当ての詳細](../../../_images/ex4-assign-copilot-details.png)

10. **Pull Requests** タブを選択します。
11. 新しく作成された pull request (PR) を開きます。タイトルは **[WIP]: Code lacks documentation** のようになります。PR が一覧に表示されない場合は、少し待ってからブラウザーを更新してください。
12. 数分後、Copilot が todo list を作成します。

> [!NOTE]
> Copilot の todo list が PR に表示されるまで数分かかることがあります。Copilot は環境を作成し（前述の workflow を実行）、プロジェクトを分析して、最適な取り組み方を判断しています。

13. todo list を確認し、完了予定の作業を把握します。
14. pull request のタイムラインを下にスクロールし、Copilot が issue の作業を開始したことを示す更新を確認します。
15. **View session** ボタンを選択します。

  ![Copilot のセッション表示](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 更新された状態が表示されない場合は、ウィンドウを再読み込みしてください。

16. 実行中のセッションをスクロールして Copilot の作業を確認します。コードを調査して状態を把握し、計画を立て、コードを作成する様子を確認できます。

この作業には数分かかることがあります。Copilot cloud agent の主な目的は、非同期でタスクを実行し、その間に別の作業へ集中できるようにすることです。この利点を活かすため、もう 1 つのタスクを cloud agent に割り当ててから、アプリケーションに機能を追加する作業を進めます。

## ゲーム詳細ページに関連ゲームを追加する

GitHub Copilot cloud agent の利点の 1 つは、作業を分けて、ユーザーが別のタスクに集中している間に agent に別の作業を進めてもらえることです。デザイン チーム向けに関連ゲームを追加する作業が短時間で終わるとしても、その時間をほかのタスクに使えます。cloud agent に割り当てましょう。

1. github.com のリポジトリに戻ります。
2. **Issues** タブを選択します。
3. **New issue** を選択して issue 作成画面を開きます。
4. **Blank issue** を選択します。
5. **Title** に `Show related games on the game details page` と入力します。
6. **Description** に次の内容を入力します。

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
    ```

7. **Create** を選択して issue を作成します。
8. 右側の **Assign to Copilot** を選択し、割り当てダイアログを開きます。

  ![issue への Copilot の割り当て](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。

ほどなくして、issue の最初のコメントに 👀 が表示され、Copilot が作業を開始したことがわかります。

![issue で Copilot が作業中であることを目の絵文字で示す画面](../../../_images/ex4-issue-eyes-emoji.png)

Copilot は 2 つ目の依頼にも取り組んでいます。Copilot cloud agent はソフトウェア エンジニアのように作業するため、常に監視する必要はありません。完了後に結果をレビューします。次は custom agent の作成と使用を確認しましょう。

## まとめと次のステップ

このレッスンでは [GitHub Copilot cloud agent][copilot-agents] を確認しました。cloud agent に issue を割り当てると、非同期でタスクを実行できます。技術的負債の解消、新しい機能の作成、別のフレームワークからのコード移行などを依頼できます。

このレッスンでは、次の内容を確認しました。

- コード生成環境のカスタマイズ。
- 操作を安全に行う方法。
- スコープが明確な issue の重要性。
- issue を Copilot に割り当てる方法。

cloud agent がバックグラウンドで作業している間に、custom agent の作成と利用に取り組みます。[Copilot cloud agent は MCP server も使用できます][cloud-agent-mcp]。また、前のモジュールで確認した custom instruction も利用できます。

## リソース

- [Copilot cloud agent について][copilot-agents]
- [GitHub issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent のセットアップ workflow に関するベスト プラクティス][cloud-agent-best-practices]

| [次のレッスン: Custom agents →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
