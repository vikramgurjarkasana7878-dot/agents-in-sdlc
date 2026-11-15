---
title: "演習 1 - Custom instructions (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: Agent mode →][next-lesson]

生成 AI を使うときは、コンテキストが重要です。特定の方法でタスクを進めたい場合や、Copilot に知っておいてほしい背景情報がある場合は、そのコンテキストを参照できるようにします。Copilot に特定のコンテキストを与える方法はいくつかあります。特に重要なのが、コード生成の指針を伝える [instruction file][instruction-files] です。

この演習では、次のことを行います。

- リポジトリの custom instruction と、パス単位で適用される instruction file を通じて、プロジェクト固有のコンテキスト、コーディング ガイドライン、ドキュメント標準が Copilot に届く仕組みを確認します。
- 現在の instruction を使って、フィルタリング用の最初のデータ スライス（publishers helper）を生成します。
- `.github/copilot-instructions.md` にリポジトリ全体の新しい標準を追加します。
- 同じ prompt を再実行し、生成コードに新しい標準が適用される様子を確認します。
- instruction の更新とフィルタリングの最初のスライスを `main` に commit し、次の演習で Copilot が最新のガイダンスを使えるようにします。

> [!CAUTION]
> 生成コードが、設定した標準から外れる場合もあります。Copilot は非決定的に動作します。この演習の目的は、instruction を更新した後の動作の変化を確認することであり、出力を一字一句一致させることではありません。

## Instruction file

### シナリオ

Tailspin Toys には、開発プラクティスに関するガイドラインと要件があります。たとえば、次のとおりです。

- データ レイヤーには必ず unit test を用意します。
- UI はダーク モードで、モダンな印象にします。
- TSDoc doc comment を使ってコードにドキュメントを追加します。
- 各ファイルの先頭に、ファイルの役割を説明するコメント ブロックを追加します。

instruction file を使って、これらのプラクティスに沿うタスクを Copilot が実行できるように情報を渡します。

### Custom instructions

custom instruction を使うと、コンテキストや設定を Copilot に渡せるため、コーディング スタイルや要件をより正確に理解させられます。コーディング規約、ライブラリ、含めたいコメントの種類を指定し、関連性の高い提案やコード スニペットを得られます。instruction はリポジトリ全体に対しても、特定のファイル種別に対しても作成できます。

instruction file には 2 種類あります。

- `.github/copilot-instructions.md` は、リポジトリへの**すべて**のリクエストで Copilot に送られる単一の instruction file です。使用する技術スタック、開発中の内容、ベスト プラクティスなど、Copilot へのほとんどの chat や CLI リクエストに関係するプロジェクト レベルの情報を記載します。
- `.github/instructions/*.instructions.md` は、特定のタスクやファイル種別に合わせて作成します。TypeScript や Astro などの言語、UI component や unit test の作成といったタスクのガイドラインを記載できます。

> [!NOTE]
> IDE で作業する場合、instruction file は Copilot Chat でのコード生成にのみ使われ、コード補完や next-edit suggestion には使われません。
>
> Copilot Chat、Copilot CLI、Copilot cloud agent は、コード生成時にリポジトリ レベルの instruction と、`applyTo` frontmatter がある `*.instructions.md` ファイルの両方を使います。
>
> Copilot は AGENTS.md や CLAUDE.md など、[ほかの標準に基づく instruction file][custom-instructions-support] もサポートしています。

### Instruction file を管理するためのベスト プラクティス

instruction file の作成方法を詳しく説明することは、このワークショップの範囲外です。ただし、サンプル プロジェクトには代表的な方法が含まれています。基本的には、次の点を意識してください。

- `copilot-instructions.md` は、プロジェクトの説明、構造、全体に適用するコーディング標準など、プロジェクト レベルのガイダンスに集中させます。
- `*.instructions.md` は、unit test、Astro component、データ レイヤーなどのファイル種別や、特定のタスクに特化した instruction に使います。
- 自然言語で明確に記述し、コードのよい例と避けるべき例を示します。

instruction file の作り方にも AI の使い方にも、唯一の正解はありません。プロジェクトに合う方法を試しながら見つけてください。

> [!TIP]
> GitHub Copilot を使うプロジェクトには、充実した instruction file のセットを用意することをお勧めします。このプロジェクトには、[UI 更新][ui-instructions]や [Astro][astro-instructions] など、さまざまなタスク向けの instruction があります。
>
> Copilot に instruction file を作成してもらうこともできます。surface ごとに方法は異なります。たとえば VS Code では **Configure Chat → Generate Agent Instructions**、Copilot CLI では `/init` を使います。各 surface のレッスンで、関連する場面を説明します。
>
> template や出発点を探す場合は、instruction file、custom agent などのリソースを集めた [awesome-copilot][awesome-copilot] を確認してください。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## このプロジェクトの custom instruction file を確認する

このリポジトリに用意された instruction file を読んでみましょう。中核となる `copilot-instructions.md` と、タスク別の `*.instructions.md` ファイルがあります。エディターか GitHub Web UI で開いてください。

1. `.github/copilot-instructions.md` を開きます。
2. プロジェクトの簡潔な説明と、**Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。**Code standards** 内の **GitHub Actions Workflows** のガイダンスにも注目してください。これらは Copilot とのあらゆるやり取りに適用されます。
3. `.github/instructions` フォルダーを開いて確認します。Astro、Drizzle データ レイヤー、テストなどに関する instruction があります。
4. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` フィールドが、適用対象を決める glob（リポジトリ ルートからの相対パス）を指定しています。たとえば、`**/*.test.ts` に一致する TypeScript のテスト ファイルが対象です。
5. このプロジェクトの unit test 作成に特化した instruction を確認します。
6. `.github/instructions/drizzle.instructions.md` を開いて、末尾まで読みます。`unit-tests.instructions.md` などの別の instruction file や、プロジェクト内の既存ファイルへのリンクに注目してください。大きな instruction を小さく再利用しやすい単位に分け、Copilot に例を示せます。（記載されているパスはリポジトリ ルートではなく、instruction file からの相対パスです。）

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションにはプロジェクトのコーディング標準が記載されていますが、コード内ドキュメントのルールはまだありません。次の手順で TSDoc doc comment とファイル コメント ヘッダーのルールを追加します。

## 変更用ブランチを作成する

ここではベスト プラクティスに沿って、変更用ブランチを作成します。

1. 前の演習で使った Codespace に戻ります。
2. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して、新しいターミナルを開きます。
3. 次のコマンドでブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions
   ```

## instruction を更新する*前に* Copilot Chat を使う

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の演習を始める前に、Codespace に戻り、Copilot Chat パネルを開いて **New Chat** を選択し、新しい会話を始めます。モードとモデルの選択は演習によって異なります。必要な場合は、各手順で説明します。

custom instruction の効果を見るため、まず現在の instruction を使って prompt を送信します。その後、instruction を更新して同じ prompt をもう一度送信し、違いを確認します。

1. 前の演習で使ったエディターのタブを閉じ、Copilot に必要なコンテキストだけが渡るようにします。
2. Copilot が helper の配置先を把握できるよう `src/lib/publishers.ts` を開きます。
3. Chat ビューの agent 選択欄から **Agent** を選択し、Copilot がファイルに変更を適用できるようにします。

   ![Chat ビューの agent 選択画面](../../../_images/shared-chat-mode-selector.png)

4. 次の prompt を送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot はプロジェクトを確認して変更を加えます。helper とテストなど、複数のファイルが対象になる場合があります。
6. 提案された helper が型付き関数であり、最初の引数として `db` client を受け取り、型付きの publisher 配列を返すことを確認します。これは `.github/instructions/drizzle.instructions.md` のデータ レイヤー規約（`src/lib/*.ts` に適用）によるものです。
7. 生成コードには TSDoc doc comment とファイル レベルのコメント ヘッダーが**ない**ことを確認します。

> [!CAUTION]
> Copilot は確率的に動作するため、明示されていなくても doc comment を追加することがあります。その場合でも問題ありません。instruction 更新後に一貫性が改善されることがこの演習のポイントです。

## リポジトリ標準を追加する

前に説明したように、`.github/copilot-instructions.md` は Copilot にプロジェクト レベルの情報を提供するファイルです。リポジトリのコーディング標準を記述し、コードの提案を改善しましょう。

1. `.github/copilot-instructions.md` をもう一度開きます。
2. 27 行目付近にある **Code formatting requirements** セクションを探します。プロジェクトのコーディング標準が記載されていますが、コード内ドキュメントのルールはまだありません。そのため、生成された helper には doc comment がありませんでした。
3. 既存の標準のすぐ下に次の Markdown を追加し、ファイル コメント ヘッダーと TSDoc doc comment を追加するよう Copilot に指示します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> 前のレッスンで見たように、instruction file は全体に適用するガイダンス用のリポジトリ レベル（`.github/copilot-instructions.md`）にも、特定の言語、ファイル種別、タスク向けの `*.instructions.md` にも作成できます。今回の doc comment ルールのようにプロジェクト全体に適用する標準は、リポジトリ レベルのファイルに記載します。

## prompt を再実行して変化を確認する

1. Copilot Chat に戻り、**New Chat** を選択してコンテキストをクリアします。
2. `src/lib/publishers.ts` を選択し、Copilot が対象ファイルに注目するようにします。
3. 前と**同じ prompt** を送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 提案されたファイルの先頭に、次のようなコメント ブロックが追加されていることを確認します。

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. 提案された関数に、次のような TSDoc comment が含まれていることを確認します。

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

Copilot を導いて新しいプロジェクト標準を適用できました。次の演習では、このコードを土台として作業を続けます。

## instruction を commit してブランチを push する

instruction file もリポジトリのほかのファイルと同じように、ソース管理で管理します。変更を commit してブランチをリポジトリに push しましょう。

1. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して新しいターミナルを開きます。
2. ターミナルで、instruction と helper の変更が含まれていることを確認します。

   ```bash
   git status
   ```

3. instruction と、helper の基盤として Copilot が変更したすべてのファイルを stage して commit します。

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # If git status shows additional supporting updates (for example tests), add those files too.
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. ブランチをリポジトリに push します。

   ```bash
   git push -u origin custom-instructions
   ```

## pull request を作成してマージする

ブランチを push したら、バックログにあるドキュメント標準の issue に関連付けて pull request を作成します。手動で行うこともできますが、GitHub tool に接続済みの Copilot に依頼できます。issue を見つけて、issue をクローズする PR を作成してからマージするよう prompt で伝えます。

> [!NOTE]
> Copilot に GitHub 上の操作を依頼するのはこれが初めてです。プロジェクト template は GitHub の tool を workspace に接続済みなので、セットアップは不要です。Copilot が初めて tool を使うとき、VS Code で GitHub へのサインインを求められる場合があります。案内に従ってください。後の演習で、この接続の仕組みである Model Context Protocol (MCP) を学びます。

1. Codespace で Copilot Chat を開きます。
2. <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) を押して Copilot Chat ビューを開き、agent の一覧から **Agent** が選択されていることを確認します。
3. instruction file の更新に関する issue を見つけ、現在のブランチから PR を作成するよう Copilot に依頼します。instruction の更新と、今後のフィルタリング作業に向けた publishers helper の土台の両方を説明に含めるよう伝えます。

   ```
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot が issue を確認して PR を作成します。
5. Copilot が GitHub 操作を許可するよう求めたら、コマンドを確認して、適切な場合に **Allow** を選択します。
6. PR が作成されたら、次の prompt で PR をマージしてローカル ブランチを `main` に戻すよう依頼します。

   ```
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. Copilot が GitHub 操作や shell command の実行許可を求めたら、コマンドを確認して **Allow** を選択します。

GitHub Copilot の助けを借りて pull request を作成し、マージできました。

## まとめと次のステップ

このプロジェクトの instruction file から Copilot がどのようにコンテキストを得るかを確認し、VS Code の Copilot Chat で次のことを行いました。

- *現在の* instruction を使ってコード生成 prompt を送り、Copilot の出力を確認しました。
- `.github/copilot-instructions.md` にリポジトリ全体の新しい標準を追加しました。
- 同じ prompt を再実行し、提案されたコードに新しい標準が反映されることを確認しました。
- 次の演習で使えるよう、instruction の更新と helper の土台を commit して `main` に反映しました。

次は、Copilot がコードベース全体に新機能を追加する際に、この instruction を[agent mode で適用します][next-lesson]。

## リソース

- [GitHub Copilot のカスタマイズ用 instruction file][instruction-files]
- [custom instruction 作成のベスト プラクティス][instructions-best-practices]
- [Copilot 向け custom instruction をより良く書くための 5 つのヒント][copilot-instructions-five-tips]
- [GitHub Copilot の個人用 custom instruction][personal-instructions]
- [Awesome Copilot — instruction file などのリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
