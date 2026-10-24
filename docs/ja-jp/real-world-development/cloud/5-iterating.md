---
title: "演習 5: GitHub Copilot の作業を反復的に改善する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

## 作業をレビューする

このラボでは、ユーザー エクスペリエンスの改善や機能追加に関する複数のタスクで GitHub Copilot を使いました。コードへのドキュメント追加、デザイン チーム向けの関連ゲーム機能の構築、高コントラスト モードとライト モードの切り替えを含むアクセシビリティ機能を依頼しました。コードの変更を確認し、必要に応じて Copilot にフィードバックを送り、改善します。

### シナリオ

生成 AI を使っても、ソフトウェア設計と DevOps の基本は変わりません。生成されたコードをレビューし、通常の DevOps プロセスに従う必要があります。チームにレビューを依頼する前に、ドキュメント、関連ゲーム機能、アクセシビリティ機能について GitHub Copilot が提案した変更を確認しましょう。

## セキュリティと GitHub Copilot cloud agent

Copilot cloud agent は非同期で監督なしにタスクを実行するため、安全性を保つための制約があります。

- Copilot はリポジトリへの読み取りアクセスと、作業用ブランチに限った書き込みアクセスのみを持ちます。
- cloud agent は GitHub Actions 内に独立した一時的な環境を作成して実行されます。
- GitHub Actions ワークフローを実行するには、人間の承認が必要です。
- [外部リソースへのアクセス][agent-firewall]は、MCP サーバーを含め、既定で制限されています。

## 生成されたドキュメントをレビューする

まず、GitHub Copilot cloud agent が最初に作成した、コードへのドキュメント追加に関するプルリクエスト (PR) を確認します。GitHub.com の標準的な PR 画面を使います。

> [!NOTE]
> PR に GitHub Copilot がファイアウォールでブロックされたという警告が表示される場合があります。MCP サーバーへの外部呼び出しなど、外部リソースへのアクセスは既定で制限されるため、これは想定内です。必要なら、[Copilot cloud agent のファイアウォールを設定または無効化][agent-firewall]できます。

1. github.com のリポジトリに戻ります。
2. **Pull Requests** を選択して PR の一覧を開きます。
3. **Add missing documentation** などのタイトルが付いた PR を開きます。

> [!NOTE]
> Copilot が作業中の場合、PR のタイトルには **[WIP]** が付きます。完了するまで待ちます。数分かかる場合があるため、休憩したり、学んだことを振り返ったりしてください。

4. 準備ができたら **Files changed** タブを選択して変更を確認します。

   ![Files changed タブ](../../../_images/shared-pr-files-changed.png)

5. 新しく追加された TSDoc ドキュメント コメントなどの変更を確認します。具体的な変更内容は異なる場合があります。TSDoc コメント、TypeScript の規約、コメント ヘッダーに注目してください。これらは演習 2 の開始時に確認したカスタム指示ファイルに基づいています。
6. レビューが完了したら **Conversation** タブに戻り、下にスクロールします。
7. ワークフローの承認待ちを示す表示を確認します。
8. 承認待ちの場合は **Approve and run workflows** を選択します。

   ![ワークフローを承認して実行する](../../../_images/shared-approve-workflows.png)

9. PR のチェック欄でワークフローがキューに入ったことを確認します。すべて問題なければ、単一 Astro アプリのプロジェクト チェックが成功します。完了まで数分かかる場合があります。

## GitHub Copilot に変更を依頼する

Copilot との PR 作業は一方通行ではありません。チーム メンバーと同じように、PR のコメントやコードへのインライン コメントで Copilot にメンションできます。Copilot はコメントを確認し、対応する新しいセッションを開始します。結果は非決定的なので、依頼文を一字一句指定することはできません。たとえば、次の変更を依頼できます。

- 各コード ファイルの先頭に、役割を説明するコメント ヘッダーを追加する。
- TypeScript と Astro ファイルに TSDoc ドキュメント コメントを追加する。
- Astro アプリの構成を説明する README を作成する。

1. 上記の案などを使い、コードベースのドキュメントへの変更をコメントで依頼します。ほかのユーザーと同様に **@copilot** をメンションします。
2. **View Session** を選択して Copilot の作業を確認します。Copilot が更新のための新しいセッションを開始します。
3. **Back to pull request** を選択して PR に戻れます。

   ![プルリクエストに戻る](../../../_images/ex7-back-to-pr.png)

4. Copilot が変更を完了すると、PR に新しいコミットが追加されます。
5. **Files changed** タブを選択して変更をレビューします。

納得できるまで繰り返し改善できます。満足したら、ドラフト PR をレビュー可能な状態にし、`main` ブランチにマージします。

![PR をレビュー可能な状態にする](../../../_images/ex7-ready-for-review.png)

## 関連ゲーム機能をレビューする

ゲーム詳細ページに関連ゲームを表示する Issue に対して、Copilot が作成した PR を確認します。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Show related games on the game details page** などのタイトルが付いた PR を選択します。
4. **Files changed** タブを選択して生成されたコードを確認します。
5. レビューが完了したら **Conversation** タブに戻り、下にスクロールします。
6. ワークフローの承認待ちを示す表示を確認します。
7. 承認待ちの場合は **Approve and run workflows** を選択します。

   ![ワークフローを承認して実行する](../../../_images/shared-approve-workflows.png)

8. PR のチェック欄でワークフローがキューに入り、単一 Astro アプリのプロジェクト チェックが成功することを確認します。完了まで数分かかる場合があります。
9. **任意:** Codespace でこのブランチに切り替えて、関連ゲーム機能を手動でテストできます。Codespace のターミナルを開き、`<branch-name>` を Copilot が作成したブランチ名 (例: **copilot/fix-8**) に置き換えて、次のコマンドを実行します。

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

Copilot は関連ゲーム機能を実装しました。Copilot cloud agent と反復的に作業し、表示件数の変更や、カスタム指示の更新前に割り当てた作業へのコメント ヘッダーや TSDoc コメントの追加を依頼できます。これまでと同様に **Conversation** タブのコメントに書けば、Copilot が新しいセッションを開始します。

## アクセシビリティ機能をレビューする

最後に、アクセシビリティ カスタム エージェントが実装した機能を確認します。この PR には演習 3 で依頼した高コントラスト モードと、演習 4 の mission control から追加依頼したライト モードの両方が含まれているはずです。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Add high contrast mode to website** などのタイトルが付いた PR を選択します。

> [!NOTE]
> Copilot がまだ作業中の場合、PR のタイトルに **[WIP]** が付きます。完了まで待ちます。数分かかる場合があります。

4. **Files changed** タブを選択して、生成されたコードをレビューします。
5. 実装の次の点を確認します。
   - モードを切り替える UI コンポーネント
   - ユーザー設定を保持する local storage の利用
   - 高コントラスト モードとライト モードの CSS またはスタイル変更
   - アクセシビリティ属性 (ARIA ラベル、キーボード ナビゲーションなど)
   - モード切り替えを処理する JavaScript/TypeScript コード
6. レビューが完了したら **Conversation** タブに戻り、下にスクロールします。
7. ワークフローの承認待ちを示す表示を確認します。
8. 承認待ちの場合は **Approve and run workflows** を選択します。

   ![ワークフローを承認して実行する](../../../_images/shared-approve-workflows.png)

9. PR のチェック欄でワークフローがキューに入り、単一 Astro アプリのプロジェクト チェックが成功することを確認します。完了まで数分かかる場合があります。
10. **任意:** Codespace でブランチを切り替え、アクセシビリティ機能を手動でテストできます。Codespace のターミナルで `<branch-name>` を Copilot が作成したブランチ名に置き換え、次のコマンドを実行します。

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

   アプリを起動し、ブラウザーで高コントラスト モードとライト モードの切り替えをテストします。ページを再読み込みしても設定が保持されることを確認します。

アクセシビリティ カスタム エージェントが、ベスト プラクティスに従ってこれらの機能を実装するよう Copilot を導いたことを確認できます。アクセシビリティ上の懸念や改善点があれば、前の PR と同様にコメントで **@copilot** に更新を依頼できます。

## 任意の演習 — さらに作業を委任する

cloud agent はバックログの実作業を任せ、その間に別の作業を進めるのに適しています。リポジトリに追加の Issue を作成して Copilot に割り当て、この習慣を身に付けましょう。次のような案があります。

- ゲーム詳細ページに支援者向け関心フォームを作成する。
- ゲーム一覧ページにページネーションを実装する。
- データ アクセス ヘルパーに入力検証とエラー処理を追加する。

## まとめ

Cloud agent ハーネスを完了しました。各レッスンを通じて、次のことを行いました。

- このリポジトリに含まれるカスタム指示ファイルを確認し、cloud agent の成果に与える影響を把握しました。
- Issue を Copilot cloud agent に割り当て、環境の準備、計画、非同期実行を確認しました。
- アクセシビリティ向けのカスタム エージェントを作成・利用し、高コントラストとライト モードの切り替えを追加しました。
- Copilot agents ページを mission control として使用し、実行中のセッションを監視・誘導しました。
- cloud agent の PR をレビューして反復的に改善し、`@copilot` に変更を依頼してワークフローを承認しました。

## レビューと次のステップ

Cloud agent ハーネスを完了しました。ほかのハーネスでは、ここで学んだ内容をさらに深められます。

- 🖥️ **[VS Code ハーネス](../../vscode/)** — IDE から Copilot Chat のエージェント モードと MCP 統合を体験します。
- 💻 **[CLI ハーネス](../../cli/)** — Plan モードと Autopilot モード、エージェント スキル、カスタム QA エージェント、Playwright MCP、Agent Merge、cloud agent への `/delegate` を使い、ターミナルから 2 つの変更をレビューしてリリースします。

自分のリポジトリで、次のアイデアを試してください。

- リファクタリングやテスト カバレッジの Issue を cloud agent に割り当てる。
- 別の分野向けカスタム エージェントを作成する。
- 別の技術スタック用に `copilot-setup-steps.yml` を設定する。

## リソース

- [GitHub Copilot][github-copilot]
- [Copilot agents について][copilot-agents]
- [GitHub Issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent セットアップ ワークフローのベスト プラクティス][cloud-agent-best-practices]
- [Copilot cloud agent のファイアウォールを設定する][agent-firewall]

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
