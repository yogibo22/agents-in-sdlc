---
slug: ja-jp/real-world-development
title: "実践的な開発"
authors:
  - geektrainer
lastUpdated: 2026-09-16
---

現実的なシナリオ、完成したアプリケーション、共有バックログを使って、ソフトウェア開発ライフサイクル全体で GitHub Copilot を体験します。変更の計画、コードの作成、動作のテスト、プルリクエストのレビュー、エージェントを使った作業の管理を反復して行います。

## シナリオ

架空の会社 Tailspin Toys で開発者として働きます。同社は、ボードゲームのクラウドファンディングを提供しています。チームのバックログは GitHub Issue として登録済みで、すぐに着手できます。ゲームの絞り込みやページネーションなどの機能開発に加え、アクセシビリティやコーディング規約などの品質改善も含まれています。

## 作業環境を選ぶ

GitHub Copilot は、普段の作業環境で利用できます。開発方法に合った環境を選んでください。各ワークショップは個別のセットアップから始まり、共通の Tailspin Toys シナリオを使います。

### [GitHub Copilot CLI][cli]

ターミナルでエージェント型アシスタントを使います。Plan モードと Autopilot モード、指示、スキル、カスタムエージェント、Playwright の Model Context Protocol (MCP)、Agent Merge、実用的なスラッシュコマンドを扱います。

### [GitHub Copilot app][app]

複数のエージェントセッションを並行して実行し、セッションモードを切り替え、キャンバスで共同作業し、デスクトップアプリで GitHub Issue とプルリクエストを管理します。

### [GitHub Copilot cloud agent][cloud]

GitHub Issue を非同期エージェントに割り当て、作業を案内し、進行状況を監視して、作成されたプルリクエストをレビューします。

### [Visual Studio Code][vscode]

Visual Studio Code と GitHub Codespaces で GitHub Copilot を使います。エディターを離れずに、Copilot Chat のエージェントモード、Model Context Protocol (MCP) サーバー、カスタムエージェントを扱います。

[cli]: cli/
[app]: app/
[cloud]: cloud/
[vscode]: vscode/
