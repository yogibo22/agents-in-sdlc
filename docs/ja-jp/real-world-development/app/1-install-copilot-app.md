---
title: "レッスン 1 - GitHub Copilot app のインストール"
description: "GitHub Copilot app をインストールし、テンプレートから作成したリポジトリを接続して、ワークスペースを確認し、クイックチャットを試します。"
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

**[GitHub Copilot app][about-copilot-app]** は、エージェント主導の開発に使うデスクトップアプリケーションです。GitHub Copilot CLI を基盤とし、GitHub とネイティブに統合されているため、リポジトリ、ブランチ、継続的インテグレーション (CI) パイプラインをすぐに利用できます。自分ですべての作業を行うのではなく、それぞれ独立したワークスペースで複数のエージェントを並行して動かし、作業を指示して、繰り返し作業を自動化するワークフロー向けに設計されています。Node.js のインストールとプロジェクトのコピーが完了したので、次はアプリをインストールしてリポジトリを接続します。

このレッスンでは、次のことを行います。

- GitHub Copilot app をインストールしてサインインします。
- GitHub リポジトリからプロジェクトをアプリに追加します。
- テンプレートで準備されたバックログなど、ワークスペースを確認します。
- クイックチャットを試し、アプリ自体について質問します。

## シナリオ

チームは、増え続けるバックログに対応するため人工知能 (AI) エージェントを導入しています。Copilot app では、Issue の選択、エージェントの実行、変更のレビュー、プルリクエストのマージを一か所から指示できます。このレッスンでは、アプリをインストールして接続し、プロジェクトについての会話を始められるようにします。

> [!NOTE]
> 対象となる Copilot プランが必要です。Copilot Free、Copilot Student、または有料プラン (Pro、Pro+、Business、Enterprise) を利用してください。Copilot Business または Copilot Enterprise を使用している場合、管理者が **GitHub Copilot app** ポリシーを有効にしておく必要があります。

## GitHub Copilot app をインストールして構成する

まず GitHub Copilot app をインストールします。Windows、macOS、Linux 向けのバージョンが用意されています。アプリをインストールして認証し、Tailspin Toys リポジトリを追加します。

1. ブラウザーで [GitHub Copilot app のランディングページ][download-app]を開きます。
2. 使用しているプラットフォーム向けのアプリをダウンロードし、ランディングページの手順に従ってインストールします。
3. インストール後、アプリを開きます。
4. **Sign in to GitHub** を選択し、画面の指示に従って認証します。データ所在地を指定して GitHub Enterprise Cloud を使用する場合は、**Use GitHub Enterprise** を選び、入力を求められたら `*.ghe.com` ホスト名を入力します。GitHub Enterprise Server はサポートされていません。
5. オンボーディング中にリポジトリの選択を求められた場合は、テンプレートから作成した Tailspin Toys リポジトリを選択します。リポジトリの選択をスキップし、オンボーディングの後で追加することもできます。
6. テーマの選択を求められたら選択し、オンボーディングを完了します。
7. サイドバーの **Sessions** にリポジトリが表示されていることを確認します。すでに一覧にある場合は、もう一度追加する必要はありません。
8. リポジトリがまだ接続されていない場合は、**Sessions** の横にある **+** を選択して新しいセッションを作成し、**Add GitHub repository** を選びます。前提条件のレッスンで控えた所有者名とリポジトリ名を使います。
9. リポジトリがサイドバーに表示されていることを確認します。この画面では、`tailspin-toys` などのリポジトリフォルダーが **Sessions** の下にまとめられ、各リポジトリの下に個々のセッションが表示されます。

## ワークスペースを確認する

プロジェクトを接続したら、アプリの使い方を確認します。サイドバーには主に次の領域があります。

- **New** - Copilot との新しいチャットセッションを開始します。
- **Pull requests** - アプリの GitHub ネイティブ統合を通じてプルリクエストを表示します。アプリを離れずに参照、絞り込み、CI 状態の確認、レビューができます。
- **Issues** - Issue を表示し、参照、絞り込み、Issue からのセッション開始ができます。
- **Automations** - スケジュールまたはオンデマンドで実行する、保存済みのエージェントタスクです。ToDo リストの管理、定期的なプロジェクト保守など、手間のかかる作業を委任できます。まとめでは次のステップとして紹介しますが、ワークショップ内の演習ではありません。
- **Customize** - Model Context Protocol (MCP) サーバー、プラグイン、スキルなどのコンポーネントを追加して、Copilot app の機能を拡張します。Playwright MCP の設定で使います。
- **Chats** - 独自のブランチやワークスペースを必要としない、質問やブレインストーミング向けの軽い会話です。最初のチャットを始めると表示されます。表示されていない場合は、**Sessions** の横にある **+** を選択してチャットを開始します。
- **Sessions** - リポジトリフォルダーと、その中のエージェントセッションをまとめます。`tailspin-toys` などのリポジトリ名を選択するとリポジトリビューが開き、その下のセッションを選択すると会話に戻ります。各セッションは独立したワークスペースで実行されるため、変更を競合させずに複数のセッションを同時に実行できます。最初のセッションは、星評価を追加するレッスンで開始します。

ワークショップを進めながら、ワークスペースを確認していきます。

> [!TIP]
> 操作方法がわからない場合や、何かが可能か知りたい場合は、Copilot に質問してください。方法を案内してくれます。

### 用意されたバックログを確認する

多くのプロジェクトにはバックログがあり、Tailspin Toys も例外ではありません。テンプレートからリポジトリを作成したときに追加されたバックログを確認します。

1. サイドバーの **Sessions** で、個々のセッションではなく Tailspin Toys のリポジトリフォルダーを選択します。次に、リポジトリの上部ナビゲーションで **Issues** を選択します。
2. Issue 番号を決めつけず、次のタイトルを探します。
   - Allow users to filter games by category and publisher
   - Update our repository coding standards
3. Issue を選択して詳細を読みます。各 Issue はエージェントセッションを開始する場所でもあります。最初の小さな変更を完了した後、フィルター機能の Issue から作業を始めます。

> [!NOTE]
> **Issues** と **Pull requests** の一覧には、Copilot app に追加したリポジトリの項目だけが自動的に表示されます。ほかのリポジトリの作業を表示する場合は、そのリポジトリをアプリに追加します。

## クイックチャットを試す

アプリ自体について質問すると、アプリの使い方をすばやく確認できます。**quick chat** は、ブランチや worktree を作成せずに質問やブレインストーミングを行う機能です。独立したセッションを必要としない、その場限りの質問に適しています。

1. サイドバーで **Chats** の横にある **+** を選択し、新しいチャットを開きます。**Chats** がまだ表示されていない場合は、**Sessions** の横にある **+** で最初のチャットを開始します。開始後に **Chats** が表示されます。
2. アプリのセッションの仕組みを尋ねます。

   ```plaintext
   How does the GitHub Copilot app use worktrees?
   ```

3. 会話ビューで回答を読みます。各セッションは独立した git worktree で実行されるため、変更を競合させずに複数のエージェントを並行して実行できます。会話は続けることも、いつでも新しいチャットを開始することもできます。

## まとめと次のステップ

GitHub Copilot app をインストールし、プロジェクトを接続して、ワークスペースを確認しました。次のことを行いました。

- アプリをインストールして GitHub にサインインしました。
- GitHub リポジトリからプロジェクトを追加しました。
- ワークスペースを確認し、**Issues** で用意されたバックログを見つけました。
- クイックチャットで、その場限りの質問をしました。

次は、[最初のエージェントセッションを開始し][next-lesson]、ゲームカードに星評価を表示します。

## リソース

- [GitHub Copilot app について][about-copilot-app]
- [GitHub Copilot app の使用を開始する][getting-started]
- [GitHub Copilot app でエージェントセッションを操作する][agent-sessions]

[next-lesson]: ../2-add-star-rating/
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
[getting-started]: https://docs.github.com/copilot/how-tos/github-copilot-app/getting-started
[agent-sessions]: https://docs.github.com/copilot/how-tos/github-copilot-app/agent-sessions
[download-app]: https://gh.io/app
