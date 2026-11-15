---
title: "演習 3 - Custom agents"
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

このレッスンでは、custom agent を使って次のことを確認します。

- custom agent の定義方法。
- custom agent へのタスクの割り当て方。

## シナリオ

Tailspin Toys は、視覚能力や好みにかかわらず、すべてのユーザーがクラウドファンディング プラットフォームを利用できるようにすることに取り組んでいます。最近、現在のダーク テーマではテキストと背景色のコントラストが不十分で、読みにくいというフィードバックがありました。このアクセシビリティ上の課題に対応するため、デザイン チームはユーザーが切り替えられるハイ コントラスト モードを求めています。

アクセシビリティは重要なため、できるだけ早く実装します。custom agent に機能を生成してもらいましょう。

## アクセシビリティ custom agent を確認する

アクセシビリティ用の custom agent はすでに用意されています。Copilot をどのように導くのかを理解するため、内容を確認しましょう。

Codespace に戻り、アクセシビリティ custom agent ファイルを確認します。

1. `.github/agents/accessibility.md` を開きます。
2. `name` と `description` フィールドがある YAML frontmatter に注目します。

> [!CAUTION]
> `name` と `description` を含む frontmatter は、custom agent に必須です。

3. 続くセクションを確認します。アクセシブルな Web サイトのコード生成に関する中核的な責務、アクセシビリティのベスト プラクティス、HTML・CSS・JavaScript のコード例、よくある落とし穴や間違いが説明されています。

## issue を作成して割り当てる

mission control は、環境内のすべての agent を管理する場所です。Copilot cloud agent にタスクを割り当てたり、監視したり、進行中のセッションに追加の指示を送ったりできます。まず、ハイ コントラスト モードの作成を Copilot に依頼します。

1. リポジトリを開きます。
2. **Issues** タブを選択します。
3. **New issue** を選択して issue 作成画面を開きます。
4. **Blank issue** を選択します。
5. **Title** に `Add high contrast mode to website` と入力します。
6. **Description** に次の内容を入力します。

    ```plaintext
    We need a high contrast mode for the site. There should be a toggle for high contrast which the user can set. It should store the setting in local storage on the browser.
    ```

7. **Create** を選択して issue を作成します。
8. 右側の **Assign to Copilot** を選択して割り当てダイアログを開きます。
9. custom agent の一覧から **Accessibility agent** を選択します。

    ![custom agent と Accessibility が選択されている cloud agent 割り当て画面](../../../_images/ex5-select-custom-agent.png)

10. **Assign** を選択します。
11. Copilot がバックグラウンドでタスクを開始します。

## まとめと次のステップ

このレッスンでは GitHub Copilot の [custom agent][custom-agents] を確認しました。custom agent は、特定のタスクや領域に合わせた専門的な AI アシスタントです。チームの専門知識や標準を再利用可能な agent に落とし込み、Copilot が作業を効果的に進められるようにできます。

このレッスンでは、次のことを確認しました。

- custom agent の定義方法。
- custom agent へのタスクの割り当て方。

Copilot がハイ コントラスト モードを実装している間に、mission control から[エージェントのセッションを監視して案内します][next-lesson]。

## リソース

- [custom agent について][custom-agents]
- [組織で custom agent を使うための準備][org-custom-agents]
- [Enterprise で custom agent を使うための準備][enterprise-custom-agents]

---

| [← 前のレッスン: GitHub Copilot cloud agent][previous-lesson] | [次のレッスン: エージェントの監視と管理 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
