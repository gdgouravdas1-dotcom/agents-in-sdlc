---
title: "演習 2 - Copilot Agent Mode で新機能を追加する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

アプリケーションの簡単な変更でも、複数ファイルの更新やテスト実行が必要になることがあります。開発者は通常、必要なファイルを探し、変更を加え、テストを実行し、デバッグし、変更漏れを見つけて、再び更新します。この繰り返しは続きます。

そこで Copilot Agent Mode が役立ちます。

Copilot Agent Mode は IDE 内でより自律的に作業します。開発者と同様に、プロジェクト構成を調べ、必要な変更を行い、テストなどのタスクを実行し、見つけた問題を自動的に修正します。Agent Mode を使ってサイトに機能を追加する方法を確認しましょう。

> [!NOTE]
> 名前は似ていますが、エージェント モードと cloud agent は異なる用途向けです。エージェント モードは IDE 内で作業し、すばやく対話しながらフィードバックを得られます。cloud agent はチーム メンバーのように非同期で作業し、Issue とプルリクエストを通じてやり取りします。

この演習では、次の方法を学びます。

- Copilot Agent Mode にプロジェクトを調査させ、関連ファイルを特定し、連携した変更を加える。
- UI とデータ レイヤーにまたがる新機能を実装させる。
- コードベースにマージする前に、生成された変更とテストをレビューする。

## シナリオ

ゲームの一覧が増えてきたため、カテゴリと出版社で絞り込めるようにします。前の演習で publishers ヘルパーを追加しました。ここでは Copilot Agent Mode を使って、残りのデータ レイヤー、UI、テストを完成させます。

## Tailspin Toys の Web サイトを実行する

変更を加える前に Tailspin Toys の現在の機能を確認します。

この Web サイトは、開発者をテーマにしたボードゲームのクラウドファンディング プラットフォームです。ゲーム一覧と詳細を表示できます。Web サイトは単一の Astro アプリで、ビルド時にページを静的 HTML としてレンダリングします。ページは frontmatter から Drizzle ORM を使ってローカル SQLite データベースに直接クエリを実行するため、別のバックエンド API やクライアント側 UI フレームワークはありません。再利用可能なデータ アクセス ヘルパーは `src/lib/` にあり、インタラクティブな動作は標準 DOM API を使った小規模な Astro `<script>` で追加します。

### Web サイトを起動する

単一の Astro 開発サーバーを起動する `npm run dev` タスクを利用できます。GitHub Codespace で次の手順を実行します。

1. Codespace に戻ります。前の演習後の `main` ブランチにいるはずです。
2. Codespace 内で <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押してターミナルを開きます。
3. `main` を更新してフィルタリング作業用ブランチを作成します。

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 次のコマンドで Web サイトを起動します。

   ```bash
   npm run dev
   ```

   Astro 開発サーバーの準備ができると、次のような URL が表示されます。

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> `http://localhost:4321` を開くためのダイアログが表示されたら、**x** を選択して閉じます。

5. ターミナルの Astro サーバー アドレス `http://localhost:4321` を、Mac では <kbd>Command</kbd>、Windows/Linux では <kbd>Ctrl</kbd> を押しながら選択してサイトを開きます。

> [!NOTE]
> Codespace のターミナルで localhost の URL を選ぶと、自動的に `https://<your-codespace-name>-4321.app.github.dev/` に転送されます。これは Codespace でホストされた Web サーバーへのプライベート トンネルです。

### Web サイトを確認する

サイトを起動したら、現在の機能を確認できます。

- **ホーム ページ**: ボードゲームのタイトル、画像、説明の一覧を表示します。
- **ゲーム詳細ページ**: ゲームを選択すると、タイトル、説明、出版社、カテゴリなどの詳細が表示されます。

## Copilot でバックログを確認する

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の演習を始める前に Codespace に戻り、Copilot Chat パネルを開いて **New Chat** を選択し、新しい会話を始めます。モードとモデルの選択は演習によって異なります。必要な箇所で説明します。

Web サイトの初期実装は動作しますが、新しい機能を追加して改善したいと考えています。まずバックログを確認しましょう。テンプレートからプロジェクトをセットアップしたとき、Issue のバックログも自動的に作成されています。GitHub Copilot に表示させます。

1. Chat ビューのエージェント ドロップダウンで **Agent** を選択します。Agent は自律的に計画を立て、複数のファイルを変更し、ターミナル コマンドを実行し、ツールを呼び出します。

   ![Chat ビューのエージェント選択画面](../../../_images/shared-chat-mode-selector.png)

2. 利用可能なモデル一覧から **Claude Sonnet 4.5** を選択します。

> [!CAUTION]
> このワークショップの作成者は、特定のモデルを推奨しているわけではありません。作成時に Claude Sonnet 4.5 を使ったため、手順に含めています。提案を比較的そろえて学習しやすくすることを期待していますが、LLM は非決定的なので、手順と異なる提案が表示されることがあります。これは通常の動作です。

> [!NOTE]
> LLM の非決定的な性質により、Copilot が別の MCP コマンドを使う場合がありますが、タスクは完了できるはずです。

3. 次のプロンプトでバックログの Issue を表示します。

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. **Continue** を選択してすべての Issue を一覧表示するコマンドを実行します。
5. 生成された Issue の一覧を確認します。

Copilot がユーザーにとって役立つ順に Issue を優先付けしたことを確認できます。

## 指示ファイルを確認する

Copilot にコードを生成させる前に、作業のコンテキストとして使う指示ファイルを確認します。Web サイトへの機能追加方法を記載した [UI 指示ファイル](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md)を使用します。

1. Codespace で `.github/instructions/ui.instructions.md` を開きます。
2. 機能追加に関する全体的な指針を確認します。次の内容が含まれます。
   - アーキテクチャの概要
   - コンポーネント設計、テスト容易性、アクセシビリティの原則
   - Astro、Tailwind CSS、Drizzle など各ファイル形式向けの指示へのリンク

> [!TIP]
> 指示ファイルから、ほかの指示ファイルやプロジェクト内のファイルを参照できます。パスは指示ファイルの位置からの相対パスです。再利用や分割が容易になり、Copilot に例やテンプレートを示せます。

## フィルタリング機能を実装する

フィルタリングを完成させるには、アプリケーションに少なくとも次の 3 つの変更が必要です。

- データ レイヤー (`src/lib/`) に残りのフィルタリング ロジックを追加または調整する。
- フィルタリング動作のテストを追加または更新する。
- ゲーム一覧ページにフィルタリング UI を追加する。

また、コードベースにマージする前にテストが成功する必要があります。Copilot Agent Mode にこれらの作業を任せてみましょう。

1. 現在の Copilot 会話を続けるか、**New Chat** を選択して新しい会話を始めます。
2. **Add Context**、**Instructions**、**ui** の順に選択します。

   ![UI 指示ファイルを選択する例](../../../_images/ex3-select-instructions-file.png)

3. Chat ビューのエージェント ドロップダウンで **Agent** が選択されていることを確認します。

   ![Chat ビューのエージェント選択画面](../../../_images/shared-chat-mode-selector.png)

4. モデルとして **Claude Sonnet 4.5** が選択されていることを確認します。
5. バックログの関連 Issue に基づいて機能を実装するよう、次のプロンプトを送信します。

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Copilot がプロジェクトを調査し、機能に関係するデータ レイヤー、UI、テストのファイルを探してから、変更とテスト実行を始める様子を確認します。

   ![Copilot がプロジェクト ファイルを調査する画面](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot はプロジェクトの調査、ファイルの変更、テスト実行など複数の作業を行います。タスクやコードベースの複雑さによって数分かかる場合があります。作業中にエディターで **Keep** と **Undo** が表示されることがあります。Copilot が終わるとすべての変更にどちらかの選択肢が表示されるため、作業途中で選ぶ必要はありません。

7. Copilot に求められたら **Continue** を選択してテストを実行します。

   ![Copilot Chat がテスト実行の確認を求めるダイアログ](../../../_images/ex3-agent-mode-run-tests.png)

8. 作業中に一時停止したり、テストが失敗したりすることがあります。Copilot はコード生成とテストを繰り返し、エラーが検出されなくなるまで作業します。

   ![Copilot Agent Mode の完了した Chat セッション](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 生成されたコードに問題がないか確認します。

> [!CAUTION]
> Copilot などの AI ツールが生成したコードは、必ずレビューしてください。

10. Web サイトを表示しているブラウザーに戻り、新しい機能を試します。
11. すべて動作し、コードのレビューも済んだら、Copilot Chat ウィンドウで **Keep** を選択します。

## まとめと次のステップ

GitHub Copilot Agent Mode を使って Tailspin Toys の Web サイトに機能を追加する方法を学びました。

- Copilot Agent Mode に UI とデータ レイヤーをまたぐ機能を実装させる。
- プロジェクトを調査し、関連ファイルを見つけて、連携した変更を行わせる。
- コードベースにマージする前に、生成された変更とテストをレビューする。

次に、[Playwright MCP サーバーで機能をテスト][next-lesson]し、プルリクエストを作成します。

### おまけの演習 — ページネーションを実装する

ゲーム一覧が増えるとページネーションが必要になります。この演習で学んだことを活用し、Copilot にサイトのページネーションを実装させてみましょう。次の点を検討します。

- 既存の指示ファイルを使うなど、ベスト プラクティスに従う。
- ページごとの件数をユーザーが選べるようにするか、固定値にするか決める。
- 希望する実装に必要な情報を含むプロンプトを作成する。
- Copilot に変更を依頼してコンテキストを与えながら、繰り返し改善する。これは Copilot を使う通常の流れです。

## リソース

- [Copilot の Ask、Edit、Agent モード: 違いと使い分け][choose-mode]
- [VS Code のエージェント モード][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
