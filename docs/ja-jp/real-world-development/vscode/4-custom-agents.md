---
title: "演習 4 - カスタム エージェント"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## カスタム エージェントとは

GitHub Copilot の[カスタム エージェント][custom-agents-concept]を使うと、開発ワークフローの特定の作業や分野に合わせた専門 AI アシスタントを作成できます。リポジトリの `.github/agents` フォルダーに Markdown ファイルを置き、焦点を絞った指示、ベスト プラクティス、コーディング パターン、分野固有の知識を Copilot に与えることで、特定の作業をより効果的に実行させられます。チームの専門知識を再利用可能なエージェントとして形式化できます。たとえば [WCAG][wcag] 準拠を確認するアクセシビリティ エージェント、安全なコーディング規約に従うセキュリティ エージェント、一貫したテスト パターンを維持するテスト エージェントを作成できます。

カスタム エージェントは、プロジェクトの `.github/agents` フォルダーまたはグローバルな `~/.copilot/agents` に Markdown ファイルとして定義します。各ファイルには少なくとも `name` と `description` を持つ YAML フロントマターがあり、その後にエージェントの動作、専門分野、指示を定義する Markdown プロンプトが続きます。

### カスタム エージェントとエージェント スキルの違い

カスタム エージェントと[エージェント スキル][agent-skills-concept]には共通点があります。どちらも主に Markdown ファイルで定義され、AI に作業方法を伝えます。簡単に言うと、**カスタム エージェントは作業者、スキルはツール**です。

カスタム エージェントには独自のコンテキスト ウィンドウがあり、作業の一部としてスキルや別のエージェントを指揮します。このラボでは、アクセシビリティ向けカスタム エージェントがガイドラインに照らしてサイトをレビューし、更新します。その際、プルリクエスト ワークフローやテストの実行・管理などのスキルを呼び出すこともできます。

> [!NOTE]
> カスタム エージェントの作成方法に唯一の正解はありません。AI を使うほかの作業と同様に、環境やシナリオに合わせて試し、改善してください。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## シナリオ

Tailspin Toys は、視覚能力や好みにかかわらず、すべてのユーザーがクラウドファンディング プラットフォームを利用できるように取り組んでいます。最近、現在のダーク テーマは背景色と文字色のコントラストが十分でなく、読みづらいという利用者の声がありました。この課題に対応するため、デザイン チームはユーザーが切り替えられる高コントラスト モードを求めています。

アクセシビリティは重要なので、カスタム エージェントを使って機能をすばやく実装します。

この演習では、次のことを行います。

- 既存のアクセシビリティ向けカスタム エージェントを確認します。
- Copilot Chat でアクセシビリティ エージェントを使って高コントラスト モードを実装します。

## アクセシビリティ カスタム エージェントを確認する

アクセシビリティ向けのカスタム エージェントはすでに作成されています。Copilot を導く方法を理解するため、その内容を確認します。

Codespace に戻り、ターミナルを開いて `main` からアクセシビリティ作業用の新しいブランチを作成します。演習 3 のフィルタリング PR とは分けておきます。

```bash
git checkout main
git pull
git checkout -b accessibility-vscode
```

1. `.github/agents/accessibility.md` を開きます。
2. `name` と `description` フィールドを含む YAML フロントマターを確認します。

> [!CAUTION]
> カスタム エージェントには `name` と `description` を含むフロントマターが必要です。

3. 続くセクションを確認します。アクセシブルな Web サイトを生成する際の主要な責任、アクセシビリティのベスト プラクティス、HTML・CSS・JavaScript のコード例、よくある問題が記載されています。

## Copilot Chat でカスタム エージェントを使う

VS Code の Copilot Chat ビュー下部にあるエージェント ドロップダウンには、`.github/agents` に定義したカスタム エージェントが表示されます。エージェントを選択すると、その指示とツールを使うようにチャット セッションを設定できます。

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の演習を始める前に Codespace に戻り、Copilot Chat パネルを開いて **New Chat** を選択し、新しい会話を始めます。モードとモデルの選択は演習によって異なります。必要な箇所で説明します。

1. Chat ビューのエージェント ドロップダウンで **Agent** を選択します。すでに選択されている場合はそのまま進みます。

   ![Chat ビューのエージェント選択画面](../../../_images/shared-chat-mode-selector.png)

2. チャット ビュー下部のエージェント ドロップダウンを選択します。現在のエージェント (既定では **default**) が表示されています。
3. 利用可能なエージェント一覧から **Accessibility agent** を選択します。
4. アクセシビリティ エージェントに次のプロンプトを送信します。

   ```plaintext
   Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
   ```

5. Copilot Chat がコードベースを調査し、変更を提案してプロジェクト ファイルに適用します。各編集はファイル パスと差分とともにチャット内に表示されます。
6. 編集が適用されると、エージェントが変更した作業ファイルの一覧を示す **Files changed** の表示が更新されます。

> [!NOTE]
> 実際の変更に数分かかる場合があります。Copilot は Astro コンポーネント、CSS、関連テストなどを更新します。

次の演習では、進行中の作業をレビューして指示を送ります。

## まとめと次のステップ

このレッスンでは、特定の作業や分野に合わせた専門 AI アシスタントである[カスタム エージェント][custom-agents]を確認しました。チームの専門知識や標準を再利用可能なエージェントにまとめ、Copilot の作業を導けます。

次のことを学びました。

- カスタム エージェントの定義方法
- Copilot Chat のエージェント モードでカスタム エージェントを使う方法

次は[エージェントの作業を監視して誘導][next-lesson]します。変更を確認しながら、同じセッションにライト モード切り替えの追加も依頼します。

## リソース

- [カスタム エージェントについて][custom-agents]
- [IDE でカスタム エージェントを作成する][creating-custom-agents-ide]
- [VS Code のカスタム エージェント][custom-agents-vscode]
- [カスタム エージェントの構成][custom-agents-config]
- [awesome-copilot のカスタム エージェント][awesome-copilot-agents]

---

| [← 前のレッスン: Playwright MCP サーバーで機能をテストする][previous-lesson] | [次のレッスン: エージェントの監視と管理 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
