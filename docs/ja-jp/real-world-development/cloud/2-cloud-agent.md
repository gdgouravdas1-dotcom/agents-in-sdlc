---
title: "演習 2 - GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: カスタム指示][previous-lesson] |
|:--|

技術的負債に悩まされていない組織はほとんどありません。未解決のセキュリティ問題、更新が必要なレガシー コード、実装する時間がなくバックログに残った機能要望などが考えられます。GitHub Copilot cloud agent は、コードの更新や機能追加といったタスクを自律的に実行できます。作業が終わると、人がレビューできるドラフト PR を作成します。面倒な作業を任せて開発を加速し、開発者はより大きな課題に集中できます。

この演習では、Copilot cloud agent を使って次のことを行います。

- コード生成環境をカスタマイズします。
- 操作を安全に行う方法を確認します。
- Issue の範囲を明確にする重要性を学びます。
- Issue を Copilot に割り当てます。

## シナリオ

Tailspin Toys には対応したい技術的負債があります。最初のサイトを作った開発会社がドキュメントを十分に整備しなかったため、まずアプリケーションのすべてのエクスポート関数に TSDoc ドキュメント コメントを追加します。

また、デザイン チームはゲームを見つけやすくしたいと考えています。各ゲームの詳細ページに、同じカテゴリに属する関連ゲームを表示し、閲覧を続けられるようにします。現時点では UX の受け入れテストに使える実装で十分です。この作業はブロッカーですが、今は優先度の高い Issue がほかにあります。

どちらも優先順位が下がりがちな作業の例です。Copilot cloud agent に割り当てれば、非同期で処理している間に別の作業を進め、後から成果をレビューできます。

## GitHub Copilot cloud agent について

[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) は、人間の開発者と同じようにバックグラウンドでタスクを実行できます。[GitHub Issue を Copilot に割り当てる][assign-issue]方法などがあります。割り当てると、Copilot は進捗を追跡するドラフト プルリクエストを作成し、環境を用意して作業を始めます。実行中または完了後にセッションを確認できます。提案された解決策のレビュー準備が整うと、プルリクエストで通知されます。

## 適切な範囲の指示が重要

GitHub Copilot に魔法はありません。考える作業を省略できるわけではありません。一見単純な作業でも、掘り下げると複雑な場合があります。[Copilot cloud agent に依頼する作業の範囲][cloud-agent-best-practices]を慎重に決めてください。AI ペア プログラマーとして段階的に使い、学び、試し、改善しましょう。生成 AI を使っても、ソフトウェア開発の基本は変わりません。

## このリポジトリのカスタム指示

前の演習ではカスタム指示と、それが Copilot を導く方法を確認しました。すでにカスタム指示の演習を完了していれば `.github/copilot-instructions.md` の動作を見ているはずです。まだの場合は、この機会に確認しましょう。cloud agent に作業を割り当てる前に、このリポジトリに含まれる指示ファイルを読み取り専用で確認し、後でその効果を見つけられるようにします。

リポジトリの GitHub Web UI、または起動済みの Codespace で、次のファイルを開きます。

- `.github/copilot-instructions.md` — **Code standards** セクション、特に TypeScript の規約と TSDoc ドキュメント コメントの要件を確認します。
- `.github/instructions/unit-tests.instructions.md` — `applyTo` フロントマターを確認します。この指示は `**/*.test.ts` に適用されます。

次のセクションで `Code lacks documentation` Issue を cloud agent に割り当てると、これらの指示ファイルから TypeScript 規約、TSDoc コメント、ファイル ヘッダーがどのように反映されるか確認できます。後の演習で PR をレビューするときにも取り上げます。

## Copilot cloud agent の開発環境を準備する

誰がコードを作成する場合でも、通常は特定の環境と、状態を整えるためのセットアップ スクリプトが必要です。ソフトウェア エンジニアと同様に作業する Copilot にタスクを割り当てる場合も同じです。

cloud agent は作業環境に [GitHub Actions][github-actions] を使います。`.github/workflows/copilot-setup-steps.yml` に設定する[セットアップ ワークフロー][setup-workflow]を作成すると、作業開始前に必要な開発ツールと依存関係を用意できます。このラボでは学習の流れを円滑にするため、事前に設定されています。単一 Astro アプリ用の Node.js、プロジェクト依存関係、エンドツーエンド テスト用ブラウザー、マイグレーションとシードを適用した SQLite データベースを用意します。

```yaml
name: "Copilot Setup Steps"

# Allows you to test the setup steps from your repository's "Actions" tab
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Set the permissions to the lowest permissions possible needed for *your steps*. Copilot will be given its own token for its operations.
    permissions:
      # If you want to clone the repository as part of your setup steps, for example to install dependencies, you'll need the `contents: read` permission. If you don't clone the repository in your setup steps, Copilot will do this for you automatically after the steps complete.
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Frontend / app setup - Node.js (the whole app is now Astro + Drizzle/libSQL)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # Migrate + seed the local SQLite database so builds and tests have data.
      - name: Set up the database
        run: npm run db:setup
```

一般的な GitHub ワークフローと同様ですが、次の点に注目してください。

- `copilot-setup-steps` という単一のジョブがあり、Copilot がプルリクエストで作業を始める前に GitHub Actions で実行されます。
- `workflow_dispatch` トリガーを使っているため、リポジトリの **Actions** タブから手動実行できます。Copilot の実行を待たずに、ワークフローが成功するか確認できます。

## ドキュメントを追加する

ドキュメントの重要性は知られていますが、情報が古かったり、まったく存在しなかったりするプロジェクトは少なくありません。このような技術的負債は放置されやすく、生産性を下げ、コードベースの保守や新しいメンバーの参加を難しくします。Copilot はドキュメント作成が得意なので、cloud agent に割り当てるのに適した Issue です。バックグラウンドで必要なドキュメントを作成し、次の演習で成果を確認します。

1. 新しいブラウザー タブで github.com のリポジトリを開きます。
2. **Issues** タブを選択します。
3. **New issue** を選択して新しい Issue を開きます。
4. **Blank issue** を選択します。
5. **Title** に `Code lacks documentation` と入力します。
6. **Description** に次の内容を入力します。

   ```plaintext
   Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
   ```

7. **Create** を選択して Issue を作成します。
8. 右側の **Assign to Copilot** を選択して割り当てダイアログを開きます。

   ![Issue に Copilot を割り当てる画面](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。

   ![Copilot の割り当て詳細](../../../_images/ex4-assign-copilot-details.png)

10. **Pull Requests** タブを選択します。
11. 新しく作成されたプルリクエスト (PR) を開きます。タイトルは `[WIP]: Code lacks documentation` のようになります。PR が一覧に表示されない場合は、少し待ってからページを更新します。
12. 数分後、Copilot が TODO リストを作成したことを確認します。

> [!NOTE]
> Copilot が TODO リストを作成するまで数分かかる場合があります。前述のワークフローを実行して環境を準備し、プロジェクトを分析して、作業方法を決めているためです。

13. リストと、実行予定のタスクを確認します。
14. プルリクエストのタイムラインを下にスクロールし、Copilot が Issue の作業を開始したことを確認します。
15. **View session** ボタンを選択します。

   ![Copilot セッションの表示](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 更新された状態が表示されるまで、ページの更新が必要な場合があります。

16. ライブ セッションをスクロールし、Copilot の作業を確認します。コードを調べて状態を把握し、計画を検討して決定した後、コードを作成します。

作業には数分かかる場合があります。Copilot cloud agent の主な目的のひとつは、非同期でタスクを実行している間にほかの作業を可能にすることです。この特長を活用し、もうひとつのタスクを割り当てたら、アプリへの機能追加に取り組みましょう。

## ゲーム詳細ページに関連ゲームのセクションを追加する

GitHub Copilot cloud agent の大きな利点は作業を分担できることです。デザイン チームが希望する関連ゲームの表示に時間がかからないとしても、その時間を別の作業に使えます。Copilot cloud agent に割り当てましょう。

1. github.com のリポジトリに戻ります。
2. **Issues** タブを選択します。
3. **New issue** を選択します。
4. **Blank issue** を選択します。
5. **Title** に `Show related games on the game details page` と入力します。
6. **Description** に次の内容を入力します。

   ```markdown
   We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
   ```

7. **Create** を選択して Issue を作成します。
8. 右側の **Assign to Copilot** を選択します。

   ![Issue に Copilot を割り当てる画面](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。

まもなく Issue の最初のコメントに 👀 が表示され、Copilot が作業中であることが分かります。

![Copilot が Issue に取り組んでいることを示す目の絵文字](../../../_images/ex4-issue-eyes-emoji.png)

Copilot は 2 つ目の依頼にも取り組み始めました。Copilot cloud agent はソフトウェア エンジニアと同様に作業するため、常に監視する必要はありません。完了後にレビューできます。次はカスタム エージェントを作成して使います。

## まとめと次のステップ

[GitHub Copilot cloud agent][copilot-agents] を使うと、Issue を割り当てて非同期で作業させられます。技術的負債への対応、新しい機能の作成、別のフレームワークへのコード移行などに利用できます。

この演習では、次のことを行いました。

- コード生成環境をカスタマイズしました。
- 操作を安全に行う方法を確認しました。
- Issue の範囲を明確にする重要性を学びました。
- Issue を Copilot に割り当てました。

cloud agent がバックグラウンドで作業している間に、カスタム エージェントの作成と利用に進みます。[Copilot cloud agent は MCP サーバーも利用でき][cloud-agent-mcp]、前のモジュールで確認したカスタム指示も使えます。

## リソース

- [Copilot cloud agent について][copilot-agents]
- [GitHub Issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent セットアップ ワークフローのベスト プラクティス][cloud-agent-best-practices]

| [次のレッスン: カスタム エージェント →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
