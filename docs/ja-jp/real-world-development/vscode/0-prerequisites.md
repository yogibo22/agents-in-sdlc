---
title: "演習 0: 前提条件"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

VS Code の演習を始める前に、必要なものを準備します。Tailspin Toys リポジトリのコピーを作成し、作業用の [codespace][codespaces] を起動して、エディターで GitHub Copilot Chat が動作することを確認します。

## ワークショップ用リポジトリを設定する

コードを作成するためのリポジトリとして、[テンプレート][template-repository]から自分用のインスタンスを作成します。新しいリポジトリにはワークショップに必要なファイルが含まれます。

1. 新しいブラウザーウィンドウで [Tailspin Toys テンプレート][tailspin-template]を開きます。
2. リポジトリのページで **Use this template** ボタンを選択し、**Create a new repository** を選びます。

   ![Use this template ボタン](../../../_images/ex0-use-template.png)

3. GitHub または Microsoft が主催するイベントでワークショップを行う場合は、メンターの案内に従います。それ以外の場合は、GitHub Copilot を利用できる組織に新しいリポジトリを作成できます。

   ![リポジトリテンプレートの設定を入力する画面](../../../_images/ex0-repository-settings.png)

4. 作成したリポジトリのパス (**organization-or-user-name/repository-name**) を控えます。以降の演習で使用します。

> [!NOTE]
> **バックログは準備済みです**
>
> テンプレートからリポジトリを作成すると、GitHub Issue のバックログも自動的に作成されます。ワークショップではこの Issue を使うため、自分で作成する必要はありません。

## codespace を作成する

次に、演習で使う codespace を作成します。

[GitHub Codespaces][codespaces] は、ブラウザーからコードを書き、実行し、デバッグできるクラウド開発環境です。複数のプログラミング言語、拡張機能、ツールを利用できる統合開発環境 (IDE) を提供します。

1. 作成したリポジトリを開きます。
2. 緑色の **Code** ボタンを選択します。

   ![Code ボタンを選択する画面](../../../_images/ex0-code-button.png)

3. **Codespaces** タブを選び、**+** ボタンを選択して新しい Codespace を作成します。

   ![新しい codespace を作成する画面](../../../_images/ex0-create-codespace.png)

codespace の作成には数分かかりますが、すべてのサービスを手動でインストールするよりも早く完了します。作成を待つ間、次に説明する GitHub Copilot の機能を確認できます。

> [!CAUTION]
> 今後の演習で codespace に戻ります。今はブラウザーのタブで開いたままにしてください。

> [!NOTE]
> このワークショップは codespace またはローカルの [dev container][dev-containers] で実行できます。どちらも必要な前提条件がインストールされた環境です。ローカルで実行する場合は、クローンしたリポジトリを VS Code で開き、プロンプトが表示されたら **Reopen in Container** を選択します。codespace と同じ dev container が構築されます。

[codespaces]: https://github.com/features/codespaces
[dev-containers]: https://code.visualstudio.com/docs/devcontainers/containers

## GitHub Copilot Chat とエージェントモードを使う

GitHub Copilot Chat のエージェントモードを使うには、IDE に GitHub Copilot Chat 拡張機能をインストールする必要があります。GitHub Codespace を使っている場合は、通常すでにインストールされています。

> [!TIP]
> GitHub Copilot Chat 拡張機能がインストールされていない場合は、[Visual Studio Code Marketplace からインストール][copilot-chat-extension]できます。または、Visual Studio Code で Extensions ビューを開き、**GitHub Copilot Chat** を検索して **Install** を選択します。

拡張機能をインストールしたら、利用を有効にするために GitHub アカウントで認証が必要になる場合があります。

1. codespace に戻ります。
2. エディターの右側に Copilot Chat が表示されていない場合は、codespace の上部にある **Copilot Chat** アイコンを選択します。
3. Copilot Chat ウィンドウに「Hello world」などのメッセージを入力して <kbd>Enter</kbd> を押します。Copilot Chat が有効になります。
4. 認証されていない場合は、GitHub アカウントへのサインインを求められます。表示される手順に従って認証します。

   ![Copilot Chat の認証を求める画面の例](../../../_images/ex1-copilot-authentication.png)

5. 認証後、Copilot Chat ウィンドウが表示されていることを確認します。

## まとめ

ワークショップ用リポジトリのコピーを作成し、コードを書くための codespace の作成を開始しました。

## 次のステップ

Copilot を使った作業を始めましょう。次は、プロジェクトの規約を Copilot に伝える[演習 1 - カスタム指示][next-lesson]に進みます。

## リソース

- [GitHub Codespaces の概要][codespaces]
- [テンプレートからリポジトリを作成する][template-repository]
- [Codespaces を使い始める][codespaces-quickstart]

[tailspin-template]: https://github.com/github-samples/tailspin-toys
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[codespaces-quickstart]: https://docs.github.com/codespaces/getting-started/quickstart
[copilot-chat-extension]: https://marketplace.visualstudio.com/items?itemName=GitHub.copilot-chat
[next-lesson]: ../1-custom-instructions/
