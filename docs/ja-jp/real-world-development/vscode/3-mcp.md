---
title: "演習 3 - Playwright MCP サーバーで機能をテストする"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: エージェントモード][previous-lesson] · [次のレッスン: カスタムエージェント →][next-lesson]

エージェントモードでフィルター機能を構築しました。プルリクエストを開く前に、ブラウザーで正しく動作することを確認します。自分でアプリを操作する代わりに、**Playwright MCP サーバー**を接続し、Copilot に実際のブラウザーを操作させて機能をテストします。その後、ブランチを公開して PR を作成します。

この演習では、次のことを行います。

- Model Context Protocol (MCP) と MCP サーバーが Copilot にツールを追加する仕組みを学びます。
- Playwright MCP サーバーをワークスペースに追加します。
- Copilot にサーバーを使わせ、ブラウザーで機能を手動テストします。
- ブランチを公開し、フィルター機能のプルリクエストを作成します。

## Model Context Protocol (MCP) とは

エージェントモードは、エディターの外部にあるツールを利用すると、さらに強力になります。Copilot は Model Context Protocol (MCP) を使って外部ツールやサービスと通信します。

![エージェントモードの内部と、コンテキスト、大規模言語モデル (LLM)、MCP サーバーおよび VS Code 拡張機能が提供するツールとの連携を示す図](../../../_images/ex1-mcp-diagram.png)

[Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) は、AI エージェントが外部のツールやサービスと通信するための仕組みです。MCP を使うと、AI エージェントはリソースを使って最新情報にアクセスしたり、ツールを使って開発者に代わって操作したりできます。

MCP サーバーは、AI エージェントと外部のツールやサービスの間を仲介します。既存のアプリケーションプログラミングインターフェイス (API) や npm パッケージなどのローカルツールとの通信を管理します。各 MCP サーバーは、エージェントがアクセスできるツールとリソースを提供します。

よく利用される MCP サーバーの例を示します。

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**: GitHub リポジトリを管理する API にアクセスし、新しいリポジトリの作成、既存リポジトリの更新、Issue やプルリクエストの管理などを行います。
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)**: Playwright を使ったブラウザー自動化機能を提供します。Web ページの移動、フォームへの入力、ボタンの選択などを行えます。

さまざまなツールやリソースを利用できる MCP サーバーがあります。GitHub が提供する [MCP registry](https://github.com/mcp) で、エコシステム内のサーバーを探したり、コントリビュートしたりできます。

> [!CAUTION]
> セキュリティの観点から、MCP サーバーはほかの依存関係と同じように扱ってください。利用前にソースコードをよく確認し、発行元を検証して、セキュリティへの影響を検討します。信頼できる MCP サーバーのみを使い、機密リソースや操作へのアクセスを許可する際は注意してください。

## MCP の構成を確認する

`.vscode/mcp.json` ファイルで、この VS Code ワークスペースから利用できる MCP サーバーを構成します。

1. Codespace で `.vscode/mcp.json` を開きます。
2. 次のように、`github` サーバーがすでに設定されていることを確認します。

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

`github` エントリはプロジェクトのテンプレートに含まれています。そのため、前の演習で Copilot が Issue のバックログを読み取れました。この設定は[リモート GitHub MCP サーバー][remote-github-mcp-server]を使います。ローカルにインストールする必要はありません。VS Code は HTTP 経由で接続し、Copilot が初めてツールを使用するときに GitHub で認証します。

## Playwright MCP サーバーを追加する

次に2つ目のサーバーを追加します。[Playwright MCP サーバー][playwright-mcp-server]を使うと、Copilot が操作できるブラウザーを利用でき、機能のテストに役立ちます。

1. `.vscode/mcp.json` の `github` と同じ階層に `playwright` エントリを追加し、ファイルを次の内容にします。

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

リモートの `github` サーバーとは異なり、`playwright` はローカルサーバーです。VS Code が `npx @playwright/mcp@latest` を実行して起動します。`--headless` フラグを指定すると、ブラウザーは画面に表示されずに動作します。デスクトップ画面がない codespace ではこの設定が必要です。

> [!NOTE]
> Tailspin Toys ではエンドツーエンドテストに Playwright を使っているため、通常はブラウザーもインストール済みです。Copilot からブラウザーが見つからないと報告された場合は、`npx playwright install chromium` を実行して再試行するよう依頼します。

## サーバーを起動して信頼する

VS Code は MCP サーバーを**必要に応じて起動**します。Copilot がサーバーのツールを初めて必要としたとき、VS Code がサーバーを起動し、信頼するか確認します。

1. `.vscode/mcp.json` の `playwright` エントリの上に **Start** 操作が表示されます。選択してサーバーを起動し、構成が正しいことを確認します。
2. VS Code からサーバーを信頼するか確認された場合は、構成を確認して、サーバーを起動できるよう信頼します。

手動での起動は省略できます。その場合、次の手順で Copilot がツールを初めて使用するときに VS Code が起動し、信頼するか確認します。

## フィルター機能をテストする

[Playwright MCP サーバー][playwright-mcp-server]を使うと、Copilot が実際のブラウザーを操作できます。開発者がアプリを手動で確認する代わりに、エージェントがページを開き、移動し、フィルターを適用して、結果を読み取って要約します。会話を離れずに機能を確認できます。

Playwright MCP サーバーは、スクリーンショットではなくページの[アクセシビリティツリー][playwright-mcp-server]を使って動作します。これにより、支援技術と同じように、ボタン、リンク、リスト項目などのラベル付き要素を基にエージェントが判断します。機能の簡単な確認が、アクセシビリティの基本確認にもなります。

サーバーを接続し、アプリを起動した状態で、Copilot に新しい機能を試させます。

```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

Copilot は Playwright MCP サーバーを通じてブラウザーを起動し、各手順を実行して結果を報告します。報告内容を Issue の受け入れ条件と照らし合わせます。動作に問題がある場合は、プルリクエストを開く前に追加の質問をするか、コードの修正を依頼します。

> [!NOTE]
> テストするには、アプリが `http://localhost:4321` で実行されている必要があります。開発サーバーを停止した場合は、プロンプトを送る前に再起動します。Copilot が Playwright MCP サーバーを初めて使うときに、ブラウザーをダウンロードする場合があります。ブラウザーがないと報告されたら、`npx playwright install chromium` の実行を依頼して再試行します。

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## ブランチを公開してプルリクエストを作成する

機能が動作することを確認できたら、チームでレビューするためにプルリクエスト (PR) を作成します。まず `filtering-vscode` ブランチを公開します。

1. codespace の **Source Control** パネルに移動し、Copilot が行った変更を確認します。
2. **+** アイコンを選択して変更をステージします。
3. **Sparkle** ボタンでコミットメッセージを生成します。

   ![変更内容を表示する Source Control パネルのスクリーンショット](../../../_images/ex3-source-control-changes.png)

4. **Publish** を選択して、ブランチをリポジトリにプッシュします。

## プルリクエストを作成する

GitHub.com や GitHub コマンドラインインターフェイス (CLI) などでプルリクエスト (PR) を作成できます。ここでは GitHub Copilot に作成を依頼します。関連する Issue を見つけ、PR と関連付けられます。

1. Copilot Chat パネルに移動し、**New Chat** を選択して新しいセッションを始めます。
2. GitHub ツールを使えるよう、エージェント選択メニューで **Agent** が選択されていることを確認します。
3. 次のプロンプトで PR の作成を依頼します。

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. Copilot が情報を収集して操作を行う際に **Continue** を選択します。GitHub ツールを初めて使うときに GitHub への認証を求められた場合は、プロンプトに従って許可します。
5. Copilot が Issue を検索し、該当する Issue を見つけて PR を作成する様子を確認します。
6. Copilot が生成したリンクを選択し、PR をレビューします。まだ**マージしないでください**。

## まとめと次のステップ

この演習では、次のことを行いました。

- Model Context Protocol (MCP) と MCP サーバーが Copilot にツールを追加する仕組みを学びました。
- Playwright MCP サーバーをワークスペースに追加しました。
- Playwright MCP を使ってブラウザーでフィルター機能を手動テストしました。
- ブランチを公開し、フィルター機能の PR を作成しました。

MCP サーバーを使って機能をテストしました。MCP は Copilot を拡張する方法の1つです。次は、アクセシビリティレビューなど、対象を絞ったタスクを効率化する[カスタムエージェントを作成します][next-lesson]。

## リソース

- [MCP とは何か、なぜ話題になっているのか][mcp-blog-post]
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
