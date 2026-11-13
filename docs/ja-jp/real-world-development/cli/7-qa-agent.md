---
title: "レッスン 7 - QA エージェントを作成して使う"
description: "Issue の要件、quality-checks スキル、Playwright MCP を組み合わせた QA カスタムエージェントを作成します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

これまでに `quality-checks` スキルで自動チェックを実行し、Playwright MCP でブラウザー上のフィルター機能を確認しました。次は、QA の手順を明確に定義したカスタムエージェントに、これらの機能をまとめます。

このレッスンでは、次のことを行います。

- カスタムエージェントと指示、スキル、MCP ツールとの連携方法を確認します。
- 再利用可能な品質保証プロファイルを作成して内容を確認します。
- QA エージェントを選び、フィルター機能の Issue に照らして報告内容を確認します。

## シナリオ

Tailspin Toys では、プルリクエスト (PR) を作成する前に、要件、コード品質、自動チェック、テストカバレッジ、ブラウザーでの動作を一貫して確認したいと考えています。カスタムエージェントを使うと QA プロセスをまとめ、再利用できるレポートを作成できます。

## カスタムエージェントとは

カスタムエージェントは、Markdown のプロファイルで定義する Copilot の専門エージェントです。プロファイルにエージェントの目的、指示、利用可能なツールを記述します。このワークショップでは `.github/agents/qa.agent.md` に QA の役割を定義し、Copilot CLI で選択します。

これまでに使用したカスタマイズには、それぞれ異なる役割があります。リポジトリの指示はチームの規約を示します。`quality-checks` スキルは繰り返し実行できるチェックをまとめます。Playwright MCP はブラウザーツールを提供します。QA プロファイルはこれらをどう使って要件を評価し、結果を報告するかを Copilot に伝えます。既存の機能を置き換えるものではなく、別の会話も必要ありません。

## QA プロファイルを作成する

機能の PR を開く前に、再利用できる QA プロファイルの作成を Copilot に依頼します。QA が行うチェックと守るべき境界の両方を定義します。

1. Codespace に戻り、フィルター機能の会話が Interactive モードであることを確認します。
2. 既定のエージェントに次の内容を依頼します。

   ```plaintext
   Create a custom agent named QA in .github/agents/qa.agent.md. It should check features against their issues and agreed requirements, follow the repository instructions, run the quality-checks skill, use Playwright MCP to verify behavior, and add tests when coverage is missing.

   Have it report each requirement as pass, fail, or blocked with supporting evidence. It must ask before changing implementation code, and it must not commit changes or open pull requests.

   Just create the profile for now so I can review it.
   ```

## プロファイルを確認する

エージェントを使う前に、Copilot が意図した QA の手順と制約を記述したことを確認します。これにより、検証だけを行いたいときに、指示が抜けたり範囲の広すぎる変更を加えたりするのを防げます。

1. `/diff` を入力し、`.github/agents/qa.agent.md` を開きます。
2. frontmatter を読みます。`description` は必須で、`name` は省略できますが、設定するとエージェントの表示名が明確になります。
3. 指示を読み、QA が要件を出発点とし、リポジトリの指示に従い、`quality-checks` スキルを実行して Playwright MCP を使うことを確認します。
4. QA が根拠を示して報告し、実装コードを変更する前に確認を求め、変更をコミットせず PR も作成しないことを確認します。
5. これらの責任や境界がプロファイルに反映されていない場合は、続行する前に既定のエージェントに修正を依頼します。

## Issue に対して QA を実行する

Copilot CLI は会話の開始時にプロジェクトのエージェントを読み込みます。プロファイルの作成後に同じフィルター機能の会話を再開し、QA を選ぶと、Issue と計画の決定事項を引き続き参照できます。

1. 次のプロンプトを使って QA エージェントを有効にします。

   ```plaintext
   /agent QA
   ```

> [!NOTE]
> 作成したばかりのエージェントは、利用可能なエージェントの一覧にまだ表示されない場合があります。その場合も、上記のコマンドで有効にできます。

2. 次のプロンプトで、QA エージェントに機能をレビューさせます。

   ```plaintext
   Review the filtering feature against the issue and the decisions in our plan. Is it ready for a PR?
   ```

3. QA エージェントが作業する様子を確認します。
4. 完了したら、エージェントのレポートを読みます。

## まとめと次のステップ

再利用可能な専門エージェントを追加して、その作業をレビューしました。このレッスンでは、次のことを行いました。

- カスタムエージェントと指示、スキル、MCP ツールとの連携方法を確認しました。
- 再利用できる品質保証プロファイルを作成して内容を確認しました。
- QA エージェントを選び、フィルター機能の Issue に対する指摘を確認しました。

これで、実装、スキルの更新、QA プロファイル、テスト、検証レポートが揃いました。次は、[機能 PR をまとめて Agent Merge を使います][next-lesson]。

## リソース

- [Copilot CLI 用のカスタムエージェントを作成する][create-agents]
- [カスタムエージェントの構成][agent-config]

[previous-lesson]: ../6-mcp-playwright/
[next-lesson]: ../8-create-pull-request/
[create-agents]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli
[agent-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
