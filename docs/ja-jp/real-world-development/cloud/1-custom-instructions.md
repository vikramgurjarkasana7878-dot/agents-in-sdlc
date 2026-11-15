---
title: "演習 1 - Custom instructions (Cloud agent)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: Copilot cloud agent →][next-lesson]

生成 AI を使うときは、コンテキストが重要です。特定の方法でタスクを進めたい場合や、Copilot に知っておいてほしい背景情報がある場合は、そのコンテキストを参照できるようにします。[instruction file][instruction-files] を使うと、何をしてほしいかだけでなく、どのように進めるかも Copilot に伝えられます。

この演習では、次のことを行います。

- リポジトリの custom instruction と、パス単位で適用される instruction file を通じて、プロジェクト固有のコンテキスト、コーディング ガイドライン、ドキュメント標準が Copilot に届く仕組みを確認します。
- `.github/copilot-instructions.md` にリポジトリ全体の新しい標準を追加します。

> [!NOTE]
> VS Code や CLI の harness とは異なり、この演習では更新前と更新後の prompt を比較しません。Copilot cloud agent は GitHub issue を基に非同期で作業するため、リアルタイムで並べて効果を示すのは困難です。instruction file の影響は、この harness の後半で cloud agent が作成した pull request をレビューする際に確認します。

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

- `.github/copilot-instructions.md` は、リポジトリへの**すべて**のリクエストで Copilot に送られる単一の instruction file です。使用する技術スタック、開発中の内容、ベスト プラクティスなど、ほとんどのチャットや CLI リクエストに関係するプロジェクト レベルの情報を記載します。
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

instruction file の書き方にも AI の使い方にも、唯一の正解はありません。プロジェクトに合う方法を試しながら見つけてください。

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

## リポジトリ標準を追加する

この演習で編集するのは、コード内に TSDoc doc comment とファイル コメント ヘッダーを追加する、プロジェクト全体のルールです。後続の演習で issue を割り当てると、cloud agent はこのルールを読み取ります。

編集の前にブランチを作成します。cloud agent は issue の対象ブランチから instruction を読み取りますが、ここでは変更をきれいに commit します。

1. Codespace のターミナルで、新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions-cloud
   ```

前に説明したように、`.github/copilot-instructions.md` で Copilot にプロジェクト レベルの情報を伝えます。コード提案の質を高めるため、リポジトリのコーディング標準を記述しましょう。

1. `.github/copilot-instructions.md` をもう一度開きます。
2. 27 行目付近にある **Code formatting requirements** セクションを探します。ここにはプロジェクトのコーディング標準が記載されていますが、コード内ドキュメントのルールはまだありません。そのため、生成された helper には doc comment がありませんでした。
3. 既存の標準のすぐ下に次の Markdown を追加し、ファイル コメント ヘッダーと TSDoc doc comment を追加するよう Copilot に指示します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> 前のレッスンで見たように、instruction file は全体に適用するガイダンス用のリポジトリ レベル（`.github/copilot-instructions.md`）にも、特定の言語、ファイル種別、タスク向けの `*.instructions.md` にも作成できます。今回の doc comment ルールのようにプロジェクト全体に適用する標準は、リポジトリ レベルのファイルに記載します。

## instruction の更新を commit、push、マージする

cloud agent は issue の対象ブランチにある instruction file を読み取ります。次の演習で Copilot に issue を割り当てると、`main` からブランチが作成されます。cloud agent が更新した instruction を読み取れるよう、変更を `main` にマージする必要があります。

1. 変更を stage して commit します。

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. ブランチを push します。

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. github.com で `custom-instructions-cloud` から `main` に向けた pull request を作成してマージします。次の演習で issue を割り当てると、cloud agent はこの instruction を読み取ります。

> [!TIP]
> このワークショップでは、`main` に直接作業して commit してもかまいません。ブランチを使う手順は、実際のプロジェクトでの作業方法を体験できるようにしています。

## まとめと次のステップ

このプロジェクトの instruction file から Copilot がコンテキストを得る方法を確認し、`.github/copilot-instructions.md` にリポジトリ全体の標準を追加しました。この harness の後半で、cloud agent が作成する pull request にその標準が反映されていることを確認します。

次は、最初の issue を [Copilot cloud agent に割り当てます][next-lesson]。

## リソース

- [GitHub Copilot のカスタマイズ用 instruction file][instruction-files]
- [custom instruction 作成のベスト プラクティス][instructions-best-practices]
- [Copilot 向け custom instruction をより良く書くための 5 つのヒント][copilot-instructions-five-tips]
- [Awesome Copilot — instruction file などのリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
