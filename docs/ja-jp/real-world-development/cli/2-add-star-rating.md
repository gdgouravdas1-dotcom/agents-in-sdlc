---
title: "レッスン 2 - 星評価を追加する: すぐできる改善"
description: "Copilot CLI でゲーム カードに小さな変更を加え、転送されたブラウザーで確認し、最初のプルリクエストとしてマージします。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Copilot CLI をインストールして会話を試したので、プロジェクトに最初の変更を加えます。小さな変更です。ゲーム データには星評価がすでにありますが、ホーム ページのゲーム カードにはまだ表示されていません。エージェントに評価を表示させ、変更をレビューして、最初のプルリクエストとしてマージします。

このレッスンでは、次のことを行います。

- 機能ブランチで焦点を絞った Copilot の会話を開始する。
- エージェントに小さな変更を依頼する。
- `/diff` で変更をレビューする。
- アプリを実行し、Codespaces から転送されたブラウザーで確認する。
- 最初のプルリクエストを作成してマージする。

## シナリオ

Tailspin Toys の各ゲームには星評価があり、ゲーム詳細ページにはすでに表示されています。一方、ホーム ページのゲーム カードにはタイトル、カテゴリ、出版社、説明しかありません。ウォームアップとして、各カードに既存の評価を表示させます。最初のセッションに適した、小さく独立した変更です。

## 会話の構成

**会話**は、Copilot CLI でタスクに取り組む場所です。Copilot app とは異なり、通常の CLI 会話では専用 worktree を作成せず、ターミナルで現在チェックアウトされているリポジトリと Git ブランチを使います。保存した会話は後で再開できますが、ファイルとブランチはディスク上の通常の Git 状態として保持されます。

会話中には、プロンプトとエージェントの応答、ファイルを調査・編集するエージェントのツール動作、`/diff` で確認できる変更の 3 つが表示されます。

## 会話を開始して変更を依頼する

新しい会話を開始して機能の実装に取り掛かります。

1. Codespace に戻ります。
2. ターミナルが開いていない場合は、<kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押します。
3. Copilot を起動していない場合は、次のコマンドを実行します。

   ```bash
   copilot --yolo
   ```

4. スラッシュ コマンド `/new` を実行して <kbd>Enter</kbd> を押し、新しいセッションを開始します。
5. 次のプロンプトで変更を依頼します。

   ```plaintext
   Show each game's starRating out of 5 in the game cards on the list page. If the rating is null, show "No rating yet". Keep the card layout as it is, add tests, and run the relevant checks.
   ```

Copilot がプロジェクトを調査し、ゲーム詳細を表示するファイルを見つけ、必要なコードを作成します。Copilot CLI で新機能を追加できました。

## 差分をレビューする

小さな変更でも、AI が生成した変更はすべてマージ前にレビューしてください。Copilot CLI 内で変更を確認します。

1. `/diff` を入力し、変更されたすべてのファイルを確認します。
2. ゲーム カードには評価がある場合に数値が表示され、`starRating` が `null` の場合は `No rating yet` と表示されることを確認します。
3. どちらの状態もテストされていることを確認します。
4. Copilot が実行したチェック結果を確認し、失敗があれば修正を依頼します。
5. レビューが完了したら <kbd>Esc</kbd> を押して差分画面を終了します。

> [!NOTE]
> 生成 AI ツールと同様、Copilot の動作は確率的であり、決定論的ではありません。コードが完全に一致することを期待するのではなく、動作をレビューしてください。

## 変更を確認する

コードを読むだけで動作すると判断せず、Copilot に Web サイトを起動させ、Codespaces から転送されたブラウザーで更新された UI を確認します。

1. Copilot にアプリの起動を依頼します。

   ```plaintext
   Start the app so I can inspect the star-rating change in my browser. Tell me the URL and leave the server running.
   ```

2. Codespaces でポート `4321` が利用可能と表示されたら **Open in Browser** を選択します。
3. ゲーム カードに 5 段階の評価が表示されることを確認します。
4. Copilot に戻り、起動したサーバーを停止するよう依頼します。

   ```plaintext
   Stop the development server you started.
   ```

## 最初のプルリクエストを作成してマージする

機能を作成できました。新しいコードをプロジェクトに取り込むため、プルリクエスト (PR) を作成します。

1. 既定のエージェントに変更をコミットするよう依頼します。

   ```plaintext
   Commit the reviewed star-rating changes with an appropriate commit message.
   ```

2. `/pr create` を入力します。Copilot CLI は PR 作成時に既存のコミットをプッシュし、PR の URL を表示します。
3. Copilot CLI が表示した URL を Mac では <kbd>Command</kbd>、Windows/Linux では <kbd>Ctrl</kbd> を押しながら選択し、PR を開きます。
4. 変更ファイルとチェックをレビューします。
5. 準備ができたら **Merge pull request** を選択し、マージを確認します。
6. Codespace に戻り、`/exit` で Copilot CLI を終了します。
7. ローカルの `main` を更新します。

   ```bash
   git checkout main
   git pull
   ```

## まとめと次のステップ

GitHub Copilot CLI を使って最初の変更をリリースしました。

- 機能ブランチで焦点を絞った Copilot の会話を開始しました。
- ゲーム カードへの小さな変更をエージェントに依頼しました。
- `/diff` で変更をレビューしました。
- 転送されたブラウザーでアプリを実行し、星評価を確認しました。
- 最初のプルリクエストを作成してマージしました。

次は[フィルタリング Issue から始め、Plan モードと Autopilot モードを使って][next-lesson]大きな機能を作ります。

## リソース

- [GitHub Copilot CLI について][about-copilot-cli]
- [Copilot CLI コマンド リファレンス][cli-reference]

[previous-lesson]: ../1-install-copilot-cli/
[next-lesson]: ../3-agent-modes/
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
