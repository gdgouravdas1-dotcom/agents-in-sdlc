---
title: "演習 1 - カスタム指示 (Cloud agent)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: Copilot cloud agent →][next-lesson]

生成 AI を使う際はコンテキストが重要です。特定の方法で作業したい場合や、Copilot に背景情報を伝えたい場合は、その情報を参照できるようにします。[指示ファイル][instruction-files]を使うと、Copilot に「何を」するかだけでなく、「どのように」行うかも伝えられます。

この演習では、次のことを行います。

- リポジトリのカスタム指示とパス別の指示ファイルを通じて、プロジェクト固有のコンテキスト、コーディング ガイドライン、ドキュメント標準が Copilot に伝わる仕組みを確認します。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する標準を追加します。

> [!NOTE]
> VS Code や CLI のハーネスとは異なり、ここでは変更前後のプロンプトを使いません。Copilot cloud agent は GitHub Issue に対して非同期で動作するため、リアルタイムで並べて比較しにくいためです。このハーネスの後半で cloud agent が作成したプルリクエストをレビューするときに、指示ファイルの影響を確認できます。

## 指示ファイル

### シナリオ

Tailspin Toys には、開発プラクティスに関するガイドラインと要件があります。

- データ レイヤーには必ずユニット テストを追加します。
- UI はダーク モードにし、現代的な見た目にします。
- TSDoc のドキュメント コメントでコードを文書化します。
- 各ファイルの先頭に、そのファイルの役割を説明するコメント ブロックを追加します。

指示ファイルを使い、Copilot がこれらのプラクティスに沿って作業するために必要な情報を与えます。

### カスタム指示

カスタム指示を使うと、Copilot がコーディング スタイルや要件を理解できるよう、コンテキストや好みを伝えられます。推奨する規約、ライブラリ、コメントの書き方などを指定し、リポジトリ全体または特定のファイル形式に適用できます。

指示ファイルには次の 2 種類があります。

- `.github/copilot-instructions.md` は、リポジトリ内のすべての依頼で Copilot に渡される単一の指示ファイルです。技術スタック、開発対象、ベスト プラクティスなど、ほとんどの依頼に関係するプロジェクト全体の情報を記述します。
- `.github/instructions/*.instructions.md` は、特定の作業やファイル形式向けの指示ファイルです。TypeScript や Astro などの言語、UI コンポーネントやユニット テストの作成方法を指定できます。

> [!NOTE]
> IDE では、指示ファイルは Copilot Chat でのコード生成にのみ使用され、コード補完や次の編集候補には使用されません。
>
> Copilot Chat、Copilot CLI、Copilot cloud agent は、リポジトリ レベルの指示と `applyTo` フロントマターを持つ `*.instructions.md` ファイルを使用します。
>
> Copilot は AGENTS.md や CLAUDE.md など、ほかの標準の指示ファイルも[サポートしています][custom-instructions-support]。

### 指示ファイルを管理するためのベスト プラクティス

サンプル プロジェクトの指示ファイルには、代表的な構成が用意されています。基本的な考え方は次のとおりです。

- `copilot-instructions.md` には、開発対象、プロジェクト構成、全体的なコーディング標準など、プロジェクト レベルの指針を記載します。
- `*.instructions.md` は、ユニット テスト、Astro コンポーネント、データ レイヤーなど、ファイル形式や特定の作業に関する指針に使います。
- 自然な言葉で明確に記述し、コードの良い例と避ける例を示します。

指示ファイルの作成方法はひとつではありません。プロジェクトやシナリオに応じて、試しながら最適な方法を見つけてください。

> [!TIP]
> GitHub Copilot を使うプロジェクトには、充実した指示ファイルのセットを用意しましょう。このプロジェクトには [UI の更新][ui-instructions]や [Astro][astro-instructions]など、さまざまな作業向けのファイルがあります。
>
> Copilot に指示ファイルを生成させることもできます。各環境での方法は異なります (VS Code の **Configure Chat → Generate Agent Instructions**、Copilot CLI の `/init` など)。必要に応じて各レッスンで説明します。
>
> テンプレートや出発点を探す場合は、指示ファイルやカスタム エージェントなどを集めた [awesome-copilot][awesome-copilot] を確認してください。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## このプロジェクトのカスタム指示を確認する

このリポジトリに含まれる指示ファイルを確認します。中核となる `copilot-instructions.md` と、各作業向けの `*.instructions.md` ファイルがあります。エディターまたは GitHub の Web UI で開いてください。

1. `.github/copilot-instructions.md` を開きます。
2. プロジェクトの簡単な説明や **Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。**Code standards** 内の **GitHub Actions Workflows** の指針も含め、Copilot とのやり取り全般に適用されます。
3. `.github/instructions` フォルダーを開き、Astro、Drizzle データ レイヤー、テストなどの指示ファイルを確認します。
4. `.github/instructions/unit-tests.instructions.md` を開き、先頭の `applyTo` を確認します。これはリポジトリ ルートからの glob で、たとえば `**/*.test.ts` に一致する TypeScript テスト ファイルに適用されます。
5. このプロジェクトのユニット テスト作成に関する指示を確認します。
6. `.github/instructions/drizzle.instructions.md` の末尾を開き、`unit-tests.instructions.md` などの別の指示ファイルやプロジェクト内のファイルへのリンクを確認します。こうして指示を再利用できる小さなファイルに分け、生成例を Copilot に示せます (パスはリポジトリ ルートではなく、指示ファイルからの相対パスです)。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションにはプロジェクト標準が記載されていますが、コード内ドキュメントの要件はまだありません。次の手順で TSDoc コメントとファイル コメント ヘッダーのルールを追加します。

## リポジトリ標準を追加する

ここで行う編集は、TSDoc コメントとファイル レベルのコメント ヘッダーをコードに含めるプロジェクト全体のルールを追加することです。後の演習で Issue を割り当てると、cloud agent がこのルールを使います。

編集前にブランチを作成します。cloud agent は Issue の対象ブランチから指示を読みますが、ここでは変更を整理してコミットします。

1. Codespace のターミナルで新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions-cloud
   ```

前述のとおり、`.github/copilot-instructions.md` は Copilot にプロジェクト レベルの情報を提供します。リポジトリのコーディング標準を明記して、提案の質を高めましょう。

1. `.github/copilot-instructions.md` を開き直します。
2. 27 行目付近の **Code formatting requirements** セクションを探します。コーディング標準が記載されていますが、コード内ドキュメントのルールはまだありません。
3. 既存の標準の直後に、次の Markdown を追加して、ファイル コメント ヘッダーと TSDoc コメントを指示します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> 前のレッスンで学んだように、指示ファイルはリポジトリ全体向けの `.github/copilot-instructions.md` と、言語、ファイル形式、作業向けの `*.instructions.md` として作成できます。今回追加したようなプロジェクト全体の標準は、リポジトリ レベルのファイルに記述します。

## 指示の更新をコミット、プッシュしてマージする

cloud agent は、対象となる Issue のブランチから指示ファイルを読み込みます。次の演習で Issue を割り当てると、Copilot は `main` からブランチを作成します。そのため、指示の変更を `main` に反映しておく必要があります。

1. ステージしてコミットします。

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. ブランチをプッシュします。

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. github.com で `custom-instructions-cloud` から `main` へのプルリクエストを作成し、マージします。次の演習で Issue を割り当てると、cloud agent はこれらの指示を読み込みます。

> [!TIP]
> このワークショップでは `main` に直接作業してもかまいません。その場合はブランチを作成せず、`main` に直接コミットできます。ここでは実際のプロジェクトに近い流れにするため、ブランチを使っています。

## まとめと次のステップ

このプロジェクトで Copilot が指示ファイルからコンテキストを得る方法を確認し、リポジトリ全体に適用する標準を `.github/copilot-instructions.md` に追加しました。以降のハーネスで cloud agent が作成するプルリクエストをレビューするときに、この標準の影響を確認できます。

次に、[最初の Issue を Copilot cloud agent に割り当てます][next-lesson]。

## リソース

- [GitHub Copilot のカスタマイズに使う指示ファイル][instruction-files]
- [効果的なカスタム指示を作成するためのベスト プラクティス][instructions-best-practices]
- [Copilot 向けの優れたカスタム指示を書く 5 つのヒント][copilot-instructions-five-tips]
- [awesome-copilot — 指示ファイルなどのリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
