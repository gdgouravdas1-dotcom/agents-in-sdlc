---
title: "レッスン 3 - エージェント モード: Plan と Autopilot"
description: "Plan で進め方を合意し、Autopilot で Issue からフィルタリング機能を作成し、Interactive モードで結果をレビューします。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

まず小さな機能をプロジェクトに追加しましたが、大きな変更にはより堅牢なプロセスが必要です。GitHub Copilot CLI は組織の既存のフローに合わせて動作し、適切な方法で必要なものを構築できます。このレッスンは、Issue から新機能を作成し、コードの妥当性と動作を確かめ、プロジェクトに正しくマージする、エージェント主導の一般的な開発プロセスを扱う複数レッスンの最初です。

> [!NOTE]
> 機能のワークフローを進める間、同じ会話とブランチを使います。通常は変更するファイルの種類ごとにブランチや PR を分けますが、ここでは基本概念に集中するため簡略化します。

このレッスンでは、次のことを行います。

- GitHub Issue から Copilot の新しい会話を始めます。
- Plan モードで要件を定義します。
- Autopilot モードで新機能を実装します。
- コードをレビューします。
- 転送されたブラウザーで機能を手動検証します。

この機能の作業を続けながら、リポジトリの指示を更新し、既存の `quality-checks` スキルをカスタマイズし、MCP 検証を追加し、QA エージェントを作成して、機能 PR を開きます。

## シナリオ

Tailspin Toys のカタログが成長し、訪問者がカテゴリと出版社でゲームを絞り込む必要があります。バックログの Issue には機能が記載されていますが、カテゴリを組み合わせる方法など、実装前に合意すべき詳細があります。Plan モードでその決定を行い、Autopilot に範囲を限定した実装を許可します。

## 背景

AI コーディング エージェントを開発フローに導入しても、基本は変わりません。むしろ、これまで以上に重要です。多くの開発者は次のような流れで作業します。

1. 実装内容を説明する Issue から始めます。
2. 作成するものの計画を立てます。
3. コードを作成してレビューします。
4. テストを実行して検証します。
5. 新しい機能を手動で検証します。
6. プルリクエスト (PR) を作成します。
7. コードレビューが完了し、継続的インテグレーションが成功したらマージします。

> [!NOTE]
> チームや組織によって詳細は異なりますが、ほとんどは上記の流れを変形したものです。

この標準的な方法を守ることで、AI が生成したコードも要件を満たし、手書きのコードと同様の審査を受けられます。

## 会話モード

**会話モード**は、エージェントにどの程度の自律性を与えるかを制御します。<kbd>Shift</kbd>+<kbd>Tab</kbd> を押してモードを切り替えます。

- **Interactive**: エージェントと協力して作業します。エージェントは変更を提案し、続行前に入力を待ちます。
- **Plan**: まず計画を作成します。プロジェクト ファイルの編集はできません。
- **Autopilot**: エージェントがコードを書き、テストを実行し、タスク完了まで反復的に作業します。

最初に Plan モードで計画をレビューしてから、Autopilot で実装します。

## Issue から始める

フィルタリング作業を始める前に、Codespace とターミナルを準備します。

1. Codespace に戻ります。停止している場合は再起動します。
2. 星評価の PR がマージされたことを確認します。
3. ターミナルが開いていない場合は、<kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押します。
4. `main` を更新し、フィルタリング作業用ブランチを作成します。

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. Copilot CLI を起動します。

   ```bash
   copilot --yolo
   ```

6. <kbd>Tab</kbd> を 2 回押して **Issues** タブを開きます。
7. <kbd>A</kbd> を押してすべての Issue を表示します。
8. 矢印キーで **Allow users to filter games by category and publisher** というタイトルの Issue を選択します。
9. <kbd>C</kbd> を押して Issue をプロンプトに追加し、**Session** タブに戻ります。

プロンプトの先頭に `#7` (または同様の番号) が表示されます。`#` を使うと、GitHub の Issue やプルリクエスト (PR) をコンテキストに追加できます。

## フィルタリング機能を計画する

計画を立てると、機能の実装やタスクの実行を Copilot に任せる前に、進め方を定義できます。複雑な作業では時間をかけて計画することをお勧めします。Plan モードに切り替え、計画の作成を依頼します。

1. <kbd>Shift</kbd>+<kbd>Tab</kbd> を押して Plan モードに切り替えます。プロンプト下部のモード表示が **Plan** であることを確認します。
2. 前の手順で追加した Issue の後に、次のプロンプトを入力します。

   ```plaintext
   Create a plan for implementing this feature.
   ```

Copilot はプロジェクトを調査し、最適な方法を検討して計画を作成します。

3. フィルタリングの動作について質問されたら、希望に基づいて回答します。唯一の正解はありません。
4. 計画が完成したら、<kbd>Control</kbd>+<kbd>E</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>E</kbd> (Windows/Linux) を押して展開します。
5. 計画をスクロールして確認します。
6. 決定と異なる部分があれば、修正を依頼します。

## Autopilot を承認する

計画を作成してレビューしたので、実装に進みます。Autopilot を使って Copilot に作業させましょう。

Autopilot では、完了したと判断するまで Copilot が問題に取り組み、繰り返し改善します。

1. **Accept plan and build on autopilot (recommended)**、またはインストール済みバージョンで同等のオプションを選択します。
2. プロンプト下のモード表示が **Autopilot** であることを確認します。
3. Copilot が計画に沿って反復し、コードを生成してテストを実行する様子を確認します。

> [!NOTE]
> 承認するとすぐ実装が始まる場合があるため、先に計画をレビューしてください。依存関係がない、またはポートが競合していると Copilot が報告したら、チェックを完了と見なす前に環境の問題を解決します。

## 実装をレビューして検証する

生成されたコードも、ほかのコードと同様にマージ前にレビューします。コードを確認し、サイトを実行して問題がないことを確かめます。

1. <kbd>Shift</kbd>+<kbd>Tab</kbd> を押して Interactive モードに切り替えます。モード表示が **Plan** または **Autopilot** ではないことを確認します。
2. `/diff` を入力し、フィルタリングの実装とテストを確認します。
3. 結果を Issue と計画時の決定と比較します。
4. コードをレビューしたら <kbd>Esc</kbd> を押して差分表示を終了します。
5. プロジェクトのチェック結果を確認し、失敗があれば Copilot に解決を依頼します。

## 新しい機能を確認する

コードが問題なさそうでも、実際に動作するとは限りません。前回と同様にアプリを起動し、Codespaces の転送ポートを使ってサイトを開きます。

1. Copilot にアプリを起動するよう依頼します。

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Codespaces でポート `4321` が利用可能と表示されたら **Open in Browser** を選択します。
3. 計画で合意したカテゴリ、出版社、それらの組み合わせによる絞り込みを試します。
4. リセット時と結果が空の場合の動作が Issue と決定内容に沿っていることを確認します。
5. Codespace に戻り、起動した開発サーバーを停止するよう Copilot に依頼します。

## まとめと次のステップ

異なる会話モードを使って機能を構築・レビューしました。

- GitHub Issue から Copilot の新しい会話を始めました。
- Plan モードで要件を定義しました。
- Autopilot モードで新機能を実装しました。
- コードをレビューしました。
- 転送されたブラウザーで機能を手動検証しました。

次は、コード生成が文書化されたプラクティスに従うように、[カスタム指示][next-lesson]を使います。

## リソース

- [GitHub Copilot CLI の Autopilot][autopilot]
- [Copilot CLI コマンド リファレンス][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
