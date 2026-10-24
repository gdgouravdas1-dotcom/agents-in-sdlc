---
title: "演習 1 - カスタム指示 (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: エージェント モード →][next-lesson]

生成 AI を使う際はコンテキストが重要です。作業方法や Copilot に知っておいてほしい背景情報がある場合は、参照できるようにします。特定のコンテキストを共有する方法はいくつかあります。中でも[指示ファイル][instruction-files]は、コード生成の指針を伝えるために使います。

この演習では、次のことを行います。

- リポジトリのカスタム指示とパス別の指示ファイルを通じて、プロジェクト固有のコンテキスト、コーディング ガイドライン、ドキュメント標準が Copilot に伝わる仕組みを確認します。
- 現在の指示を使ってフィルタリング用の最初のデータ スライス (publishers ヘルパー) を生成します。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する標準を追加します。
- 同じプロンプトを再実行し、生成コードが新しい標準に従うことを確認します。
- 指示の更新とフィルタリングのスライスを `main` にコミットし、次の演習で Copilot が使えるようにします。

> [!CAUTION]
> 生成コードが設定した標準から外れることがあります。Copilot の出力は非決定的です。この演習では、指示を更新した後の動作の傾向を確認します。出力を一字一句同じにすることが目的ではありません。

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

指示ファイルの作成方法はひとつではありません。プロジェクトやシナリオに応じて試しながら、最適な方法を見つけてください。

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
5. このプロジェクトでユニット テストを作成するための指示を確認します。
6. `.github/instructions/drizzle.instructions.md` の末尾を開き、`unit-tests.instructions.md` などの別の指示ファイルやプロジェクト内ファイルへのリンクを確認します。こうして指示を小さく再利用しやすいファイルに分け、生成例を Copilot に示せます (パスは指示ファイルからの相対パスです)。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションにはプロジェクト標準が記載されていますが、コード内ドキュメントの要件はまだありません。次の手順で TSDoc コメントとファイル コメント ヘッダーのルールを追加します。

## 変更用のブランチを作成する

ここではベスト プラクティスに従い、変更用のブランチを作成します。

1. 前の演習で使用した Codespace に戻ります。
2. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して新しいターミナルを開きます。
3. ブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions
   ```

## 指示を更新する前に Copilot Chat を使う

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の演習を始める前に Codespace に戻り、Copilot Chat パネルを開いて **New Chat** を選択し、新しい会話を始めます。モードとモデルの選択は演習によって異なります。必要な箇所で説明します。

カスタム指示の効果を確認するため、まず現在の指示ファイルのままでプロンプトを送信します。その後、指示を更新して同じプロンプトを再送し、違いを確認します。

1. Copilot が必要なコンテキストだけを使うよう、前の演習で開いたエディター タブを閉じます。
2. `src/lib/publishers.ts` を開き、ヘルパーの配置先を Copilot に示します。
3. Chat ビューのエージェント ドロップダウンから **Agent** を選び、Copilot がファイルを変更できるようにします。

   ![Chat ビューのエージェント選択画面](../../../_images/shared-chat-mode-selector.png)

4. 次のプロンプトを送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot がプロジェクトを調査し、ヘルパー ファイルとテストなどに変更を適用します。
6. 提案されたヘルパーが、`db` クライアントを最初の引数に取る型付き関数で、型付きの出版社配列を返すことを確認します。これは `src/lib/*.ts` に適用される `.github/instructions/drizzle.instructions.md` のデータ レイヤー規約によるものです。
7. 提案されたコードには、TSDoc ドキュメント コメントとファイル レベルのコメント ヘッダーがないことを確認します。

> [!CAUTION]
> Copilot は非決定的なため、指示がなくてもドキュメント コメントを追加することがあります。その場合も問題ありません。指示更新後の一貫性の改善を確認することが目的です。

## リポジトリ標準を追加する

前述のとおり、`.github/copilot-instructions.md` は Copilot にプロジェクト レベルの情報を伝えます。コーディング標準を明記して、提案の質を高めましょう。

1. `.github/copilot-instructions.md` を開き直します。
2. 27 行目付近にある **Code formatting requirements** セクションを探します。プロジェクト標準が記載されていますが、コード内ドキュメントのルールはまだありません。
3. 既存の標準の直後に次の Markdown を追加し、ファイル コメント ヘッダーと TSDoc コメントを Copilot に指示します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> 前のレッスンで確認したように、指示ファイルはリポジトリ全体向けの `.github/copilot-instructions.md` と、言語、ファイル形式、作業向けの `*.instructions.md` として作成できます。今回のドキュメント コメント規則のような全体標準は、リポジトリ レベルのファイルに記載します。

## プロンプトを再実行して変化を確認する

1. Copilot Chat に戻り、**New Chat** を選んで新しい会話を始めます。
2. `src/lib/publishers.ts` を選択して Copilot の対象ファイルを示します。
3. 先ほどと**同じプロンプト**を送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 提案されたファイルの先頭に、次のようなコメント ブロックがあることを確認します。

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. 提案された関数に、次のような TSDoc コメントがあることを確認します。

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

新しいプロジェクト標準を Copilot に伝え、次の演習につながる実際のコードに適用できました。

## 指示とブランチをコミットしてプッシュする

指示ファイルもリポジトリの資産です。ほかのファイルと同じようにソース管理します。

1. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押してターミナルを開きます。
2. 指示とヘルパーの変更があることを確認します。

   ```bash
   git status
   ```

3. 指示の更新と、ヘルパーの基盤として Copilot が変更したすべてのファイルをステージしてコミットします。

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # If git status shows additional supporting updates (for example tests), add those files too.
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. ブランチをリポジトリにプッシュします。

   ```bash
   git push -u origin custom-instructions
   ```

## プルリクエストを作成してマージする

ブランチをプッシュしたら、バックログのドキュメント標準の Issue に関連付けるプルリクエストを作成します。GitHub のツールがプロジェクトに接続されているため、Issue を探し、Issue を解決する PR を作成してマージするよう Copilot に依頼できます。

> [!NOTE]
> この演習では初めて Copilot に GitHub の操作を依頼します。プロジェクト テンプレートで GitHub のツールをワークスペースに接続済みなので、セットアップは不要です。初回の利用時に VS Code から GitHub へのサインインを求められる場合があります。画面の指示に従ってください。後の演習で、この接続に使う Model Context Protocol を学びます。

1. Codespace の Copilot Chat を開きます。
2. <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) を押して Copilot Chat ビューを開き、エージェント選択で **Agent** が選択されていることを確認します。
3. 指示ファイルの更新に関係する Issue を探し、現在のブランチから PR を作成して、新しい publishers ヘルパーの基盤も説明するよう依頼します。

   ```
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot が GitHub の操作を許可するよう求めたら、コマンドを確認して適切な場合に **Allow** を選択します。
5. PR の作成後、次のプロンプトで PR をマージし、ローカルのブランチを `main` に戻します。

   ```
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

6. GitHub の操作やシェル コマンドを許可するよう求められたら、内容を確認して適切な場合に **Allow** を選択します。

GitHub Copilot の支援でプルリクエストを作成してマージできました。

## まとめと次のステップ

このプロジェクトで Copilot が指示ファイルからコンテキストを得る方法を確認し、VS Code の Copilot Chat を使って次のことを行いました。

- 既存の指示でコード生成プロンプトを送信し、提案内容を確認しました。
- `.github/copilot-instructions.md` にリポジトリ全体の標準を追加しました。
- 同じプロンプトを再実行し、提案コードが新しい標準に従うことを確認しました。
- 次の演習で作業を引き継げるよう、指示の更新とヘルパーの基盤をコミットして `main` に反映しました。

次は [エージェント モード][next-lesson]を使って、Copilot にコードベース全体の機能を追加させます。

## リソース

- [GitHub Copilot をカスタマイズする指示ファイル][instruction-files]
- [効果的なカスタム指示を作成するためのベスト プラクティス][instructions-best-practices]
- [Copilot 向けの優れたカスタム指示を書く 5 つのヒント][copilot-instructions-five-tips]
- [GitHub Copilot の個人用カスタム指示][personal-instructions]
- [awesome-copilot — 指示ファイルなどのリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
[awesome-copilot]: https://github.com/github/awesome-copilot
