---
title: "演習 5: GitHub Copilot の作業を反復して改善する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

## 作業をレビューする

このラボでは、ユーザー体験の改善と機能追加を目的とする複数のタスクを GitHub Copilot に依頼しました。コードへのドキュメント追加、デザイン チームが改善できる関連ゲーム機能の構築、ハイ コントラストとライト モードの切り替えを含むアクセシビリティ機能の実装を依頼しています。変更を確認し、必要に応じて Copilot にフィードバックを送って、作業を改善しましょう。

### シナリオ

これまでに何度も説明したように、生成 AI を導入しても、ソフトウェア設計と DevOps の基本は変わりません。生成されたコードをレビューし、通常の DevOps プロセスに沿って作業します。ここでは、生成されたドキュメント、関連ゲーム機能、アクセシビリティ機能について GitHub Copilot の提案をレビューし、チームのレビューを有効にします。

## セキュリティと GitHub Copilot cloud agent

Copilot cloud agent は非同期かつ人の監視なしでタスクを実行するため、安全性を保つための制約があります。

- Copilot はリポジトリへの読み取りアクセスと、コード用ブランチへの書き込みアクセスのみを持ちます。
- cloud agent は GitHub Actions 内で、独立した一時的な環境を作成して作業します。
- GitHub Actions workflow の実行には、人間による承認が必要です。
- 既定では、[外部リソースへのアクセスは制限されています](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall)。MCP server も対象です。

## 生成されたドキュメントをレビューする

まず、Copilot cloud agent が作成した最初の pull request (PR)、つまりコードへのドキュメント追加を確認します。GitHub.com の標準的な PR 画面を使います。

> [!NOTE]
> PR に GitHub Copilot が firewall によってブロックされているという警告が表示されることがあります。Copilot は既定で外部リソース（外部 MCP server への呼び出しを含む）へのアクセスが制限されているため、これは**想定どおり**です。必要であれば、[Copilot cloud agent の firewall を設定または無効化][agent-firewall]できます。

1. github.com のリポジトリに戻ります。
2. **Pull Requests** を選択して PR の一覧を開きます。
3. **Add missing documentation** などのタイトルが付いた pull request を開きます。

> [!NOTE]
> Copilot がまだ作業中の場合、pull request のタイトルに **[WIP]** が含まれます。その場合は作業の完了を待ちます。数分かかることがあるため、休憩したり、これまでに学んだ内容を振り返ったりしてください。

4. PR の準備ができたら **Files changed** タブを選択し、変更をレビューします。

    ![Files changed タブ](../../../_images/shared-pr-files-changed.png)

5. 新しく追加された TSDoc doc comment や、そのほかのドキュメントを確認します。変更内容は状況によって異なります。

    変更を確認する際は、演習 2 の最初に確認した custom instruction file による TSDoc doc comment、TypeScript の規約、コメント ヘッダーを探してください。

6. レビューが終わり問題がなければ、**Conversation** タブに戻り、下にスクロールします。
7. workflow の実行承認を待っている表示があることを確認します。
8. 承認待ちの workflow があれば **Approve and run workflows** を選択します。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. pull request の checks セクションに workflow がキューに入ったことが表示されます。問題がなければ、単一 Astro app のプロジェクト チェックが成功します。完了まで数分かかることがあります。

## GitHub Copilot に変更を依頼する

Copilot との pull request 作業は一方通行ではありません。pull request やコードのインライン コメントで、チーム メンバーに依頼するのと同じように Copilot にメンションできます。Copilot はコメントを確認し、対応するセッションを新たに開始します。結果は一意ではないため、必ずしも適切な依頼文を示せるわけではありません。たとえば、次の変更を依頼できます。

- 各コード ファイルの先頭に、内容を簡潔に説明するコメント ヘッダーを追加する。
- TypeScript と Astro のファイルに TSDoc doc comment を追加する。
- Astro app の構造を説明する README を作成する。

1. 上記の例のいずれか、または追加したいドキュメント変更を選び、**@copilot** をメンションしてコメントを追加します。
2. **View Session** を選択し、Copilot が作業を進める様子を確認します。Copilot が新しいセッションを開始することにも注目してください。
3. **Back to pull request** を選択すると PR に戻れます。

    ![Back to pull request](../../../_images/ex7-back-to-pr.png)

4. Copilot が変更を完了すると、pull request に新しい commit が追加されます。
5. **Files changed** タブを選択し、変更をレビューします。

納得できるまで反復してもかまいません。満足したら、draft PR をレビュー可能な状態に変更して `main` ブランチにマージできます。

![PR をレビュー可能な状態に変更する画面](../../../_images/ex7-ready-for-review.png)

## 関連ゲーム機能をレビューする

ゲーム詳細ページへの関連ゲーム表示を依頼した issue に対し、Copilot が作成した PR を確認しましょう。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Show related games on the game details page** などのタイトルが付いた PR を選択します。
4. **Files changed** タブを選択し、生成されたコードをレビューします。
5. レビューが終わり問題がなければ、**Conversation** タブに戻り、下にスクロールします。
6. workflow の実行承認を待っている表示があることを確認します。
7. 承認待ちの workflow があれば **Approve and run workflows** を選択します。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

8. pull request の checks セクションに workflow がキューに入ったことが表示されます。単一 Astro app のプロジェクト チェックが成功するまで、数分かかることがあります。
9. **省略可能:** Codespace でこのブランチに切り替えて、関連ゲーム機能を手動でテストできます。Codespace に移動してターミナルを開き、次のコマンドを実行します（`<branch-name>` は Copilot が作成したブランチ名、たとえば **copilot/fix-8** に置き換えます）。

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

Copilot が関連ゲーム機能を実装しました。ほかの PR と同様に、Copilot cloud agent に更新を依頼して反復できます。たとえば、関連ゲームの表示件数を変えたり、コメント ヘッダーや TSDoc doc comment を追加したりできます（この issue を割り当てた時点では instruction を更新する前でした）。**Conversation** タブにコメントを追加すると Copilot が確認し、新しいセッションを開始します。

## アクセシビリティ機能をレビューする

最後に、アクセシビリティ custom agent が実装したアクセシビリティ機能を確認します。この PR には、演習 3 で依頼したハイ コントラスト モードと、演習 4 の mission control で追加依頼したライト モードの両方が含まれます。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Add high contrast mode to website** などのタイトルが付いた PR を選択します。

> [!NOTE]
> Copilot がまだ作業中の場合、pull request のタイトルに **[WIP]** が含まれます。その場合は作業の完了を待ちます。数分かかることがあります。

4. **Files changed** タブを選択し、生成されたコードをレビューします。
5. 実装を確認し、特に次の点を確認します。
   - モード切り替え用の UI component
   - ユーザー設定を保持する local storage の利用
   - ハイ コントラスト モードとライト モード用の CSS またはスタイルの変更
   - アクセシビリティ属性（ARIA label、キーボード操作など）
   - モード切り替えを管理する JavaScript/TypeScript コード

6. レビューが終わり問題がなければ、**Conversation** タブに戻り、下にスクロールします。
7. workflow の実行承認を待っている表示があることを確認します。
8. 承認待ちの workflow があれば **Approve and run workflows** を選択します。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. pull request の checks セクションで workflow がキューに入り、単一 Astro app のプロジェクト チェックが成功することを確認します。完了まで数分かかることがあります。
10. **省略可能:** Codespace でこのブランチに切り替えてアクセシビリティ機能を手動テストできます。Codespace に移動してターミナルを開き、次のコマンドを実行します（`<branch-name>` は Copilot が作成したブランチ名に置き換えます）。

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    続いてアプリケーションを起動し、ブラウザーでハイ コントラストとライト モードの切り替えをテストします。ページを再読み込みしても設定が保持されることを確認してください。

アクセシビリティ custom agent が、アクセシビリティのベスト プラクティスに沿って機能を実装できるよう Copilot を導いたことに注目してください。改善点や懸念があれば、前の PR と同様にコメントで **@copilot** に更新を依頼できます。

## 省略可能な演習 — さらに作業を委任する

cloud agent は、実際のバックログ作業を任せ、その間に別の作業へ集中するのに適しています。リポジトリにいくつか issue を作成して Copilot に割り当ててみましょう。次のような案があります。

- ゲーム詳細ページに支援者向けの関心フォームを作成する。
- ゲーム一覧ページにページネーションを実装する。
- データアクセス helper に入力検証とエラー処理を追加する。

## まとめ

Cloud agent harness を完了しました。これらのレッスンを通じて、次のことを行いました。

- **このリポジトリにある custom instruction file を確認し**、cloud agent の出力に影響することを確認しました。
- **issue を Copilot cloud agent に割り当て**、環境のセットアップ、計画、非同期の作業を確認しました。
- **アクセシビリティ用の custom agent を作成して使い**、ハイ コントラストとライト モードの切り替え機能を追加しました。
- **Copilot agents ページを mission control として使い**、アクセシビリティのセッションを監視し、作業中に指示を追加しました。
- **cloud agent の pull request をレビューして反復し**、`@copilot` に変更を依頼し、workflow を承認しました。

## レビューと次のステップ

Cloud agent harness は完了です。ほかの harness では、今回の内容を別の環境で試せます。

- 🖥️ **[VS Code harness](../../vscode/)** — IDE から Copilot Chat の agent mode と MCP 統合を確認します。
- 💻 **[CLI harness](../../cli/)** — Plan、Autopilot、agent skill、QA custom agent、Playwright MCP、Agent Merge、cloud agent への省略可能な `/delegate` を使い、ターミナルからレビュー済みの変更を 2 つリリースします。

自分のリポジトリで、次のアイデアを試してください。

- リファクタリングやテスト カバレッジの issue を cloud agent に割り当てる。
- 別の分野向けの custom agent を作成する。
- 異なる技術スタック用に `copilot-setup-steps.yml` を設定する。

## リソース

- [GitHub Copilot][github-copilot]
- [Copilot agents について][copilot-agents]
- [GitHub issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent のセットアップ workflow に関するベスト プラクティス][cloud-agent-best-practices]
- [Copilot cloud agent の firewall を設定する][agent-firewall]

---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
