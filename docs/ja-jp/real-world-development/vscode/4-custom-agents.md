---
title: "演習 4 - Custom agents"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## custom agent とは

GitHub Copilot の [custom agent][custom-agents-concept] を使うと、開発ワークフロー内の特定のタスクや領域に合わせた専門的な AI アシスタントを作成できます。リポジトリの `.github/agents` フォルダーにある Markdown ファイルで agent を定義すると、特定の種類の作業をより効果的に進めるための instruction、ベスト プラクティス、コーディング パターン、領域固有の知識を Copilot に与えられます。チームの専門知識を再利用可能な agent として定義できます。たとえば、[WCAG][wcag] への準拠を徹底するアクセシビリティ agent、安全なコーディングに従うセキュリティ agent、一貫したテスト パターンを守るテスト agent などがあります。

custom agent は、プロジェクト内の `.github/agents` フォルダー、またはグローバルな `~/.copilot/agents` にある Markdown ファイルで定義します。各ファイルには、少なくとも `name` と `description` を含む YAML frontmatter を記述し、その後に agent の動作、専門知識、instruction を定める Markdown prompt を続けます。

### custom agent と agent skill の比較

custom agent と [agent skill][agent-skills-concept] には概念的に重なる部分があります。どちらも主に Markdown ファイルで定義し、AI に作業の進め方を伝えます。わかりやすく区別すると、**custom agent** は作業者、**skill** は tool です。

custom agent には独自のコンテキスト ウィンドウがあり、作業の一部として skill（ほかの agent も含む）を調整するように設計されています。このラボでは、アクセシビリティ custom agent がアクセシビリティ ガイドラインに沿ってサイトをレビューし、更新します。その作業の中で、pull request ワークフロー用の skill や、テストを実行・管理する skill を呼び出すこともできます。

> [!NOTE]
> custom agent の作成に唯一の正解はありません。AI のほかの使い方と同様に、自分の環境やシナリオに合わせて試し、改善してください。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## シナリオ

Tailspin Toys は、視覚能力や好みにかかわらず、すべてのユーザーがクラウドファンディング プラットフォームを利用できるようにすることに取り組んでいます。現在のダーク テーマではテキストと背景色のコントラストが不十分で、読みにくいというフィードバックがありました。このアクセシビリティ上の課題に対応するため、デザイン チームはユーザーが切り替えられるハイ コントラスト モードを求めています。

アクセシビリティは重要なため、できるだけ早く実装します。custom agent に機能を生成してもらいましょう。

この演習では、次のことを行います。

- 既存のアクセシビリティ custom agent を確認します。
- Copilot Chat でアクセシビリティ agent を使い、ハイ コントラスト モードを実装します。

## アクセシビリティ custom agent を確認する

アクセシビリティ用の custom agent はすでに用意されています。Copilot をどのように導くのかを理解するため、内容を確認しましょう。

Codespace に戻り、ターミナルを開いて、アクセシビリティ作業用の新しいブランチを `main` から作成します（演習 3 のフィルタリング PR とは分けておきます）。

```bash
git checkout main
git pull
git checkout -b accessibility-vscode
```

1. `.github/agents/accessibility.md` を開きます。
2. `name` と `description` フィールドがある YAML frontmatter に注目します。

> [!CAUTION]
> `name` と `description` を含む frontmatter は、custom agent に必須です。

3. 続くセクションを確認します。アクセシブルな Web サイトのコード生成に関する中核的な責務、アクセシビリティのベスト プラクティス、HTML・CSS・JavaScript のコード例、よくある落とし穴や間違いが説明されています。

## Copilot Chat で custom agent を使う

VS Code の Copilot Chat ビュー下部にある agent の一覧には、`.github/agents` で定義したすべての custom agent が表示されます。custom agent を選択すると、その instruction と tool を使う chat session にできます。

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の演習を始める前に、Codespace に戻って Copilot Chat パネルを開き、**New Chat** を選択して新しい会話を始めます。モードとモデルの選択は演習によって異なります。必要な場合は、手順で説明します。

1. agent の一覧から **Agent** を選択します（すでに選択されている場合はそのまま進みます）。

   ![Chat ビューの agent 選択画面](../../../_images/shared-chat-mode-selector.png)

2. Chat ビューの下部にある agent の一覧を選択します。既定では、選択中の agent として **default** が表示されています。
3. 利用可能な agent の一覧から **Accessibility agent** を選択します。
4. 次の prompt をアクセシビリティ agent に送信します。

    ```plaintext
    Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
    ```

5. Copilot Chat がコードベースを調べ、変更を提案してプロジェクト ファイルを編集します。変更はファイル パスと差分とともに chat に表示されます。
6. 編集が進むと、agent が変更したファイル数を示す **Files changed** の表示が更新されます。

> [!NOTE]
> 処理には数分かかることがあります。Copilot は Astro component、CSS、関連テストなどの既存ファイルを編集し、実際にリポジトリを変更します。

次の演習で、実行中の作業を確認し、方向を調整します。

## まとめと次のステップ

このレッスンでは GitHub Copilot の [custom agent][custom-agents] を確認しました。custom agent は、特定のタスクや領域に合わせた専門的な AI アシスタントです。チームの専門知識や標準を再利用可能な agent に落とし込み、Copilot が作業を効果的に進められるようにできます。

このレッスンでは、次のことを確認しました。

- custom agent の定義方法。
- Copilot Chat の agent mode で custom agent を使う方法。

次は、エージェントの作業を[監視して案内し][next-lesson]、変更を確認しながら、同じセッションにライト モードの切り替え機能を追加します。

## リソース

- [custom agent について][custom-agents]
- [IDE で custom agent を作成する][creating-custom-agents-ide]
- [VS Code の custom agent][custom-agents-vscode]
- [custom agent の設定][custom-agents-config]
- [awesome-copilot の custom agent][awesome-copilot-agents]

---

| [← 前のレッスン: Playwright MCP server で機能をテストする][previous-lesson] | [次のレッスン: エージェントの監視と管理 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
