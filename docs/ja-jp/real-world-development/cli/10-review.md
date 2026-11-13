---
title: "レッスン 10 - 振り返りと次のステップ"
description: "Copilot CLI の開発フロー、2つのプルリクエスト、再利用可能なカスタマイズ、追加リソースを振り返ります。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

GitHub Copilot CLI を使い、Tailspin Toys の一連のワークフローに取り組みました。次のことを行いました。

- Codespace を準備し、Copilot CLI をインストールして、プロジェクトを確認し、登録済みのフィルター機能の Issue を見つけました。
- 星評価を追加し、転送されたブラウザーで結果を確認して、最初のプルリクエスト (PR) を手動でマージしました。
- フィルター機能の Issue から始め、Plan モードでアプローチを決め、Autopilot モードで構築して、Interactive モードでレビューしました。
- カスタム指示でエージェントを導き、既存の `quality-checks` スキルをカスタマイズして、ユニットテスト、lint、型チェックを実行しました。
- Playwright Model Context Protocol (MCP) サーバーを追加し、実際のブラウザーでフィルター機能を確認しました。
- 品質保証 (QA) カスタムエージェントを作成して選び、要件、カバレッジ、スキルの結果、ブラウザーでの検証の証拠を確認しました。
- フィルター機能の変更全体をレビューし、Agent Merge による PR の処理を許可しました。
- コンテキスト、モデル、共有、任意のクラウド委任に使うスラッシュコマンドを確認しました。

## リリースしたもの

このワークショップには、更新済みの `main` から開始した2つの PR があります。

1. **星評価**: 既存の `starRating` と未評価状態をゲームカードに表示します。
2. **フィルター機能と品質ワークフロー**: フィルター機能を実装し、指示を更新して機能に適用し、`quality-checks` のレポートをカスタマイズし、QA プロファイルと関連テストを追加します。

フィルター機能の計画から PR 作成までは、同じ会話とブランチを使いました。ワークショップを効率よく進めるため、一連の作業を1つの PR にまとめました。

## 複数の方法で検証する

自動テスト、自分でのブラウザー確認、MCP を通じた Copilot のブラウザー操作でコードを確認しました。`quality-checks` スキルはユニットテスト、lint、型チェックを実行し、新しい形式で結果を報告しました。QA はこれらの結果と要件、テストカバレッジのレビューをまとめて PR の確認を支援しました。

追加するテストは、実際の不足を補うものである必要があります。QA の実行で追加テストが不要と判断されても問題ありません。マージを許可する前にコードと検証の証拠を確認し、変更後は影響を受ける証拠を更新してください。

## ベストプラクティス

Copilot に与えるコンテキストとツールが、作業結果を左右します。このワークショップでは、指示を更新し、スキルをカスタマイズし、QA プロファイルを作成し、MCP サーバーを設定しました。会話をまたいでこれらのカスタマイズを再利用し、チームのニーズに合わせて調整できます。指示は規約を示し、スキルは繰り返し実行できるタスクを記述し、カスタムエージェントは専門的な役割を定義し、MCP サーバーは外部ツールに接続します。エージェントの要約だけでなく、実際の変更とツールの結果を確認してください。

タスクに合った**モードとモデル**を選びます。構築前にアプローチを検討するには **Plan**、範囲を絞った変更に関与し続けるには **Interactive**、範囲が明確な作業には **Autopilot** を使います。定型的な編集には高速なモデルを、複雑な作業には高性能なモデルを選びます。

ツールやインフラストラクチャと同じように、コンテキストも重要です。何を、なぜ、どのように構築するかを明確に伝えると、結果が大きく変わります。

## さらに試す

主要なワークフローを終えた後に、次の CLI 機能を試せます。

- `/review` でコードレビューエージェントに変更を分析させます。
- `/rubber-duck` で問題を整理し、別の視点を得ます。
- `/fleet` で独立したサブタスクを並行して調整します。
- `/worktree` で別のタスクを分離します。
- `/delegate` で Copilot cloud agent にタスクを送信します。

## 次のステップ

ツールの使い方を上達させるには、使い続けることが一番です。本番コード、趣味のコード、長年作りたいと思っていた小さなアプリなどで活用してください。チームと学びを共有し、お互いの経験から学びましょう。必要に応じてドキュメントも確認してください。

ターミナルから Tailspin Toys を拡張する場合は、任意の [Foundry Backer Concierge シリーズ][foundry]に進みます。ほかの環境と比較するには、[VS Code ワークショップ][vscode]、[GitHub Copilot app ワークショップ][app]、または [Copilot cloud agent ワークショップ][cloud]を確認してください。

## リソース

- [GitHub Copilot CLI について][about-cli]
- [Copilot CLI コマンドリファレンス][cli-reference]
- [Copilot CLI のカスタマイズ][customize-cli]
- [Copilot CLI でプルリクエストを管理する][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
