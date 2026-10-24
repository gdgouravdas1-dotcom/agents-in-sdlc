---
title: "演習 0: 前提条件"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Copilot cloud agent の演習を始める前に、必要な準備をします。Tailspin Toys リポジトリのコピーを作成し、指示ファイルの編集や cloud agent の成果のレビューに使う [Codespace][codespaces] を起動します。

## 演習用リポジトリをセットアップする

コードを作成するためのリポジトリのコピーとして、[テンプレート][template-repository]から自分用のインスタンスを作成します。このインスタンスには演習に必要なファイルが含まれており、各演習で使用します。

1. 新しいブラウザー ウィンドウで [Tailspin Toys テンプレート][tailspin-template]を開きます。
2. 演習用リポジトリのページで **Use this template** を選択し、続けて **Create a new repository** を選択します。

   ![Use this template ボタン](../../../_images/ex0-use-template.png)

3. GitHub または Microsoft が主催するイベントでワークショップを受講している場合は、メンターの指示に従います。それ以外の場合は、GitHub Copilot を利用できる組織に新しいリポジトリを作成できます。

   ![リポジトリ テンプレートの設定を入力する画面](../../../_images/ex0-repository-settings.png)

4. 作成したリポジトリのパス (**organization-or-user-name/repository-name**) を控えておきます。演習の後半で使用します。

## Codespace を作成する

次に、Codespace を使って演習を進めます。

[GitHub Codespaces][codespaces] は、ブラウザーから直接コードの記述、実行、デバッグができるクラウド開発環境です。複数のプログラミング言語、拡張機能、ツールをサポートするフル機能の IDE を利用できます。

1. 作成したリポジトリを開きます。
2. 緑色の **Code** ボタンを選択します。

   ![Code ボタンを選択する画面](../../../_images/ex0-code-button.png)

3. **Codespaces** タブを選び、**+** ボタンを選択して新しい Codespace を作成します。

   ![新しい Codespace を作成する画面](../../../_images/ex0-create-codespace.png)

Codespace の作成には数分かかりますが、必要なサービスを手動でインストールするよりもずっと早く準備できます。その間に GitHub Copilot のほかの機能を確認してもよいでしょう。

> [!CAUTION]
> 今後の演習でこの Codespace に戻ります。今はブラウザーのタブで開いたままにしてください。

> [!NOTE]
> このワークショップは Codespace またはローカルの [開発コンテナー][dev-containers]で実行できるように構成されています。どちらも必要な前提条件を備えているため、スムーズに進められます。ローカルで実行する場合は、クローンしたリポジトリを VS Code で開き、プロンプトが表示されたら **Reopen in Container** を選択します。Codespace と同じ開発コンテナーが構築されます。

[codespaces]: https://github.com/features/codespaces
[dev-containers]: https://code.visualstudio.com/docs/devcontainers/containers

## まとめ

演習用リポジトリのコピーを作成し、Copilot cloud agent と並行して作業するための Codespace の作成を開始しました。

## 次のステップ

cloud agent が従うカスタム指示を追加します。[演習 1 - カスタム指示][next-lesson]に進んでください。

## リソース

- [GitHub Codespaces の概要][codespaces]
- [テンプレートからリポジトリを作成する][template-repository]
- [Codespaces を使い始める][codespaces-quickstart]

[tailspin-template]: https://github.com/github-samples/tailspin-toys
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[codespaces-quickstart]: https://docs.github.com/codespaces/getting-started/quickstart
[next-lesson]: ../1-custom-instructions/
