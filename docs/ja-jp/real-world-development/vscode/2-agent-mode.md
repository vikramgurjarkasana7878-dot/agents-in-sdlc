---
title: "演習 2 - Copilot Agent Mode で新機能を追加する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

最も簡単なアプリケーションの更新でも、通常は複数のファイル変更やテスト実行などが必要です。開発者は、関連ファイルを探し、変更を加え、テストを実行し、デバッグし、更新し忘れたファイルがないか確認して、さらに修正することになります。必要な作業は次々に増えていきます。

こうした作業に役立つのが Copilot Agent Mode です。

Copilot Agent Mode は、IDE 内でより自律的に作業するように設計されています。開発者と同様に、既存のプロジェクト構造を調べ、必要な更新を行い、テストなどのタスクを実行し、見つかった問題を自動的に修正します。Agent Mode を使って、サイトに新しい機能を追加する方法を確認しましょう。

> [!NOTE]
> 名前は似ていますが、agent mode と cloud agent は異なる用途向けに作られています。agent mode は IDE 内で動作し、すばやいフィードバックとやり取りを可能にします。cloud agent はチーム メンバーのように非同期で作業し、issue と pull request を通じて連携します。

この演習では、次のことを学びます。

- Copilot Agent Mode がプロジェクトを調べ、関連ファイルを特定して、複数の変更を調整する方法。
- GitHub Copilot Agent Mode で UI とデータ レイヤーにわたる新機能を実装する方法。
- Copilot Agent Mode が生成した変更とテストを、コードベースにマージする前にレビューする方法。

## シナリオ

ゲームの一覧が増えてきたため、カテゴリとパブリッシャーで絞り込めるようにします。前の演習で publishers helper を追加したので、Copilot Agent Mode でデータ レイヤー、UI、テストに必要な残りの作業を行います。

## Tailspin Toys の Web サイトを実行する

変更を加える前に、Tailspin Toys の Web サイトを見て、現在の機能を確認しましょう。

この Web サイトは、開発者をテーマにしたボードゲームのクラウドファンディング プラットフォームです。ゲームの一覧と詳細を表示できます。単一の Astro app であり、ビルド時にページを静的 HTML として生成します。ページは frontmatter 内で Drizzle ORM を使ってローカルの SQLite データベースを直接参照します。別の backend API や client-side UI framework はありません。再利用可能なデータアクセス helper は `src/lib/` に置き、操作性は標準 DOM API を使った小規模な Astro `<script>` で追加します。

### Web サイトを起動する

Web サイトを簡単に実行できるように、単一の Astro dev server を起動する `npm run dev` タスクが用意されています。次の手順で GitHub Codespace から実行できます。

1. Codespace に戻ります。前の演習の続きで `main` にいることを確認します。
2. Codespace 内で <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押して、新しいターミナルを開きます。
3. フィルタリング作業用のブランチを作成して切り替えます。

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 次のコマンドで Web サイトを起動します。

   ```bash
   npm run dev
   ```

   Astro dev server の準備ができると、次のような URL が表示されます。

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> `http://localhost:4321` をブラウザーで開くか尋ねるダイアログが表示された場合は、**x** を選択して閉じます。

5. <kbd>Command</kbd> (Mac) または <kbd>Ctrl</kbd> (Windows/Linux) を押しながら、ターミナルに表示された Astro server のアドレス `http://localhost:4321` を選択して Web サイトを開きます。

> [!NOTE]
> Codespace のターミナルに表示される localhost URL を選択すると、`https://<your-codespace-name>-4321.app.github.dev/` に自動的に転送されます。これは、Web server を実行している Codespace へのプライベート トンネルです。

### Web サイトを確認する

Web サイトが起動したら、機能を確認できます。主な機能は次のとおりです。

- **ホーム ページ**: タイトル、画像、説明とともにボードゲームの一覧を表示します。
- **ゲーム詳細ページ**: ゲームを選択すると、タイトル、説明、パブリッシャー、カテゴリなどの情報を表示します。

## Copilot でバックログを確認する

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の演習を始める前に、Codespace に戻って Copilot Chat パネルを開き、**New Chat** を選択して新しい会話を始めます。モードとモデルの選択は演習によって異なります。必要な場合は、各手順で説明します。

Web サイトの初期実装は動作しますが、新しい機能を追加したいところです。まずはバックログを確認しましょう。template からプロジェクトを作成すると、issue のバックログも自動的に作成されています。GitHub Copilot に一覧を表示してもらいます。

1. Chat ビューの agent 選択欄から **Agent** を選択します。**Agent** はファイルにまたがる変更を自律的に計画・実装し、ターミナル コマンドを実行して tool を呼び出します。

   ![Chat ビューの agent 選択画面](../../../_images/shared-chat-mode-selector.png)

2. 利用可能なモデルの一覧から **Claude Sonnet 4.5** を選択します。

> [!CAUTION]
> このワークショップの作成者が特定のモデルを推奨するものではありません。ワークショップでは Claude Sonnet 4.5 を使ったため、比較的安定した体験になるよう手順に含めています。ただし、LLM は確率的に動作するため、説明と異なる提案が表示されることがあります。これは正常な動作です。

> [!NOTE]
> LLM の確率的な性質により、Copilot が異なる MCP command を使う場合がありますが、タスク自体は完了できます。

3. 次の prompt を Copilot に送信して、バックログについて質問します。

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. **Continue** を選択して、すべての issue を一覧表示する command を実行します。
5. 生成された issue の一覧を確認します。

Copilot がユーザーに最も役立つと考えた順に、issue を優先順位付けしたことを確認してください。

## instruction file をレビューする

agent にコードを生成してもらう前に、作業のコンテキストを与える instruction file を確認します。Web サイトの機能追加に取り組む方法を説明する [user interface (UI)][ui-instructions] file を使います。

1. Codespace で `.github/instructions/ui.instructions.md` を開きます。
2. 機能追加に関する全体的なガイダンスを確認します。次の内容が含まれています。
   - アーキテクチャの概要。
   - component の設計、テスト可能性、アクセシビリティの原則。
   - Astro、Tailwind CSS、Drizzle などのファイル用 instruction へのリンク。

> [!TIP]
> instruction file から、ほかの instruction file やプロジェクト内のファイルを参照できます。パスは instruction file からの相対パスです。これにより、再利用しやすく小さな単位に分け、Copilot に例や template を示せます。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md

## フィルタリング機能を実装する

フィルタリングを完成させるには、少なくとも次の 3 つの別々の変更が必要です。

- データ レイヤー (`src/lib/`) にフィルタリング ロジックを追加または調整する。
- フィルタリングの動作をテストする test を追加または更新する。
- ゲーム一覧ページにフィルタリング UI を追加する。

さらに、コードベースにマージする前にテストが成功する必要があります。Copilot Agent Mode にこれらの作業を依頼しましょう。

1. 現在の Copilot 会話を続けるか、**New Chat** を選択して新しい会話を始めます。
2. **Add Context**、**Instructions**、**ui** を選択して instruction file を追加します。

   ![UI instruction file を選択する画面](../../../_images/ex3-select-instructions-file.png)

3. Chat ビューの agent 選択欄で **Agent** が選択されていることを確認します。

   ![Chat ビューの agent 選択画面](../../../_images/shared-chat-mode-selector.png)

4. モデルが **Claude Sonnet 4.5** のままであることを確認します。
5. バックログの関連 issue に基づいて機能を実装するよう、次の prompt を送信します。

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Copilot がプロジェクトを調べ、必要なファイルを特定する様子を確認します。データ レイヤー、UI、テストを見つけ、ファイルを更新してテストを実行します。

   ![Copilot がプロジェクト ファイルを確認する画面](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot はプロジェクトの調査、ファイルの変更、テストの実行など、複数の作業を行います。タスクやコードベースの複雑さによっては数分かかります。作業中、エディターに **Keep** と **Undo** のボタンが表示される場合があります。Copilot が終わると、すべての変更に対して **Keep** または **Undo** を選択できるため、作業中に選択する必要はありません。

7. Copilot が求めたら **Continue** を選択してテストを実行します。

   ![Copilot Chat でテスト実行の確認を求めるダイアログ](../../../_images/ex3-agent-mode-run-tests.png)

8. 作業中に一時停止したり、テストが失敗したりすることがあります。問題ありません。Copilot はコード生成とテストを繰り返し、エラーがないと判断するまで作業を続けます。

   ![Copilot Agent Mode の完了した Chat セッション](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 生成されたコードに問題がないか確認します。

> [!CAUTION]
> Copilot やほかの AI tool が生成したコードをレビューすることは常に重要です。

10. Web サイトを実行しているブラウザーに戻り、新しい機能を試します。
11. 動作を確認してコードのレビューを終えたら、Copilot Chat ウィンドウで **Keep** を選択します。

## まとめと次のステップ

この演習では、GitHub Copilot Agent Mode を使って Tailspin Toys に機能を追加しました。次の方法を学びました。

- GitHub Copilot Agent Mode で UI とデータ レイヤーにまたがる新機能を実装する。
- Copilot Agent Mode でプロジェクトを調べ、関連ファイルを特定し、複数の変更を調整する。
- Copilot Agent Mode が生成した変更とテストを、コードベースにマージする前にレビューする。

次は [Playwright MCP server で機能をテスト][next-lesson]し、pull request を作成します。

### 追加の演習 — ページネーションを実装する

ゲーム一覧が増えると、ページネーションが必要になります。この演習で学んだことを使い、Copilot にページネーションの実装を依頼します。次の点を検討してください。

- 既存の instruction file を使うなど、既存のベスト プラクティスに従う。
- ページサイズをユーザーが選択できるようにするか、固定値にするかを検討する。
- 希望する実装に必要なガイダンスを prompt で Copilot に伝える。
- GitHub Copilot に変更やコンテキストを伝え、反復が必要になる場合がある。これは通常の進め方です。

## リソース

- [Copilot の ask、edit、agent mode: それぞれの役割と使い分け][choose-mode]
- [VS Code の agent mode][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
