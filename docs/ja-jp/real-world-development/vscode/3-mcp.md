---
title: "演習 3 - Playwright MCP サーバーで機能をテストする"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: エージェント モード][previous-lesson] · [次のレッスン: カスタム エージェント →][next-lesson]

エージェント モードでフィルタリング機能を作成しました。プルリクエストを作成する前に、ブラウザーで実際に動作することを確認します。自分でアプリを操作する代わりに、**Playwright MCP サーバー**を接続し、Copilot に実際のブラウザーで機能をテストさせます。その後、ブランチを公開して PR を作成します。

この演習では、次のことを行います。

- Model Context Protocol (MCP) と、MCP サーバーが Copilot にツールを追加する仕組みを学びます。
- Playwright MCP サーバーをワークスペースに追加します。
- Copilot にブラウザーを操作させ、フィルタリング機能を手動テストします。
- ブランチを公開し、フィルタリング作業のプルリクエストを作成します。

## Model Context Protocol (MCP) とは

エージェント モードは、エディターの外部にアクセスできるとさらに便利になります。Copilot がそれを実現する方法が Model Context Protocol (MCP) です。エージェントが外部のツールやサービスと通信するための標準です。

![エージェント モードの内部と、MCP サーバーや VS Code 拡張機能が提供するツールを含むコンテキスト、LLM、ツールとの連携を示す図](../../../_images/ex1-mcp-diagram.png)

[Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) を使うと、AI エージェントが外部のツールやサービスと通信できます。MCP によって、エージェントはリソースを使って最新情報にアクセスし、ツールを使ってユーザーに代わって操作できます。

ツールとリソースには、AI エージェントと外部ツールやサービスの橋渡しとなる MCP サーバーを介してアクセスします。MCP サーバーは、既存 API や NPM パッケージなどのローカル ツールとの通信を管理します。各サーバーはエージェントが利用できる別々のツールとリソースを提供します。

よく使われる MCP サーバーをいくつか紹介します。

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**: GitHub リポジトリ、Issue、プルリクエストを管理する API を利用できます。
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)**: Playwright を使ったブラウザー自動化機能を利用できます。Web ページへの移動、フォーム入力、ボタン選択などをエージェントが実行できます。

さまざまなツールやリソースを利用できる MCP サーバーがほかにもあります。GitHub は、見つけやすさとエコシステムへの貢献を促す [MCP registry](https://github.com/mcp) をホストしています。

> [!CAUTION]
> セキュリティの観点から、MCP サーバーはプロジェクトのほかの依存関係と同様に扱ってください。利用前にソース コードを慎重に確認し、発行元を検証して、セキュリティ上の影響を検討します。信頼できるサーバーだけを使い、機密情報や操作へのアクセス権を付与する場合は注意してください。

## MCP の構成を確認する

`.vscode/mcp.json` ファイルで、この VS Code ワークスペースで使う MCP サーバーを設定します。

1. Codespace で `.vscode/mcp.json` を開きます。
2. `github` サーバーがすでに設定されています。

   ```json
   {
     "servers": {
       "github": {
         "type": "http",
         "url": "https://api.githubcopilot.com/mcp/"
       }
     }
   }
   ```

この `github` エントリはプロジェクト テンプレートに含まれているため、前の演習で Copilot がバックログの Issue を読み取れました。[リモート GitHub MCP サーバー][remote-github-mcp-server]を使うため、ローカルにインストールする必要はありません。VS Code は HTTP で接続し、Copilot が初めてツールを使うときに GitHub で認証します。

## Playwright MCP サーバーを追加する

次に 2 つ目のサーバーを追加します。[Playwright MCP サーバー][playwright-mcp-server]は Copilot が操作できるブラウザーを提供します。今回の機能テストに役立ちます。

1. `.vscode/mcp.json` で `github` と並べて `playwright` エントリを追加し、次のようにします。

   ```json
   {
     "servers": {
       "github": {
         "type": "http",
         "url": "https://api.githubcopilot.com/mcp/"
       },
       "playwright": {
         "command": "npx",
         "args": ["@playwright/mcp@latest", "--headless"]
       }
     }
   }
   ```

2. ファイルを保存します。

リモートの `github` サーバーとは異なり、`playwright` は**ローカル** サーバーです。VS Code は `npx @playwright/mcp@latest` を実行して起動します。`--headless` フラグは画面を表示せずにブラウザーを動かします。デスクトップ画面のない Codespace 内ではこれが必要です。

> [!NOTE]
> Tailspin Toys ではエンドツーエンド テストに Playwright を使っているため、必要なブラウザーは通常インストール済みです。Copilot からブラウザーがないと報告されたら、`npx playwright install chromium` を実行して再度試すよう依頼してください。

## サーバーを起動して信頼する

VS Code は必要に応じて MCP サーバーを起動します。Copilot が初めてサーバーのツールを必要とすると、VS Code はサーバーを起動し、信頼するか確認します。

1. `.vscode/mcp.json` の `playwright` エントリの上に **Start** アクションが表示されます。選択してサーバーを起動し、設定が正しいことを確認します。
2. VS Code でサーバーを信頼するか確認された場合は、設定を確認し、サーバーを起動するために信頼します。

手動起動は任意です。省略した場合は、次の手順で Copilot がツールを必要としたときに VS Code が起動と信頼確認を行います。

## フィルタリング機能をテストする

[Playwright MCP サーバー][playwright-mcp-server]を使うと、Copilot が実際のブラウザーを操作できます。自分でアプリを操作して確認する代わりに、エージェントにページを開かせ、移動し、フィルターを適用し、結果を読み取って報告させられます。会話を離れずに機能の動作をすばやく確認できます。

Playwright MCP サーバーはスクリーンショットではなく、ページの[アクセシビリティ ツリー][playwright-mcp-server]を使います。エージェントはボタン、リンク、リスト項目など、ラベル付きの構造化要素を使って動作を判断します。これは支援技術と同じ方法なので、簡単な機能テストがアクセシビリティの基本確認にもなります。

サーバーを接続し、アプリを起動した状態で、Copilot に作成したフィルタリング機能をテストするよう依頼します。

```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

Copilot は Playwright MCP サーバーからブラウザーを起動し、手順に沿って各項目を確認して結果を報告します。Issue の受け入れ基準と照らして概要を確認します。問題があれば、プルリクエストを作成する前に質問を続けるか、コードを修正するよう依頼します。

> [!NOTE]
> テスト中はアプリが `http://localhost:4321` で実行されている必要があります。開発サーバーを停止した場合は、プロンプトを送る前に再起動してください。Copilot が Playwright MCP サーバーを初めて使うときにブラウザーをダウンロードする場合があります。ブラウザーがないと報告されたら `npx playwright install chromium` を実行して再度試すよう依頼してください。

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## ブランチを公開してプルリクエストを作成する

機能が動作することを確認できたので、チームがレビューできるようにプルリクエスト (PR) を作成します。まず `filtering-vscode` ブランチを公開します。

1. Codespace の **Source Control** パネルに移動し、Copilot の変更を確認します。
2. **+** アイコンを選択して変更をステージします。
3. **Sparkle** ボタンでコミット メッセージを生成します。

   ![変更を示す Source Control パネル](../../../_images/ex3-source-control-changes.png)

4. **Publish** を選択してブランチをリポジトリにプッシュします。

## プルリクエストを作成する

github.com や GitHub コマンド ライン インターフェイス (CLI) など、PR の作成方法はいくつかあります。ここでは GitHub Copilot に PR を作成させます。関連する Issue を探し、PR に関連付けられます。

1. Copilot Chat パネルを開き、**New Chat** を選択して新しいセッションを開始します。
2. エージェント ドロップダウンで **Agent** を選び、Copilot が GitHub ツールを使えるようにします。
3. 次のプロンプトで PR の作成を依頼します。

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. Copilot に情報収集や操作の続行を求められたら **Continue** を選択します。GitHub ツールを初めて使うときは GitHub 認証を求められる場合があります。画面の指示に従ってください。
5. Copilot が Issue を検索して該当するものを見つけ、PR を作成することを確認します。
6. Copilot が生成したリンクを選択して PR を確認しますが、**まだマージしないでください**。

## まとめと次のステップ

この演習では、次のことを行いました。

- Model Context Protocol (MCP) と、MCP サーバーが Copilot にツールを追加する仕組みを学びました。
- Playwright MCP サーバーをワークスペースに追加しました。
- 出荷前にブラウザーでフィルタリング機能を手動テストしました。
- ブランチを公開し、フィルタリング作業のプルリクエストを作成しました。

MCP サーバーを使って機能をテストしましたが、Copilot を拡張する方法は MCP だけではありません。次はアクセシビリティ レビューなどに使える[カスタム エージェントを作成][next-lesson]します。

## リソース

- [MCP とは何か、なぜ話題なのか][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [GitHub MCP Server][github-mcp-server]
- [GitHub MCP Registry][mcp-registry]
- [VS Code の MCP サーバー][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
