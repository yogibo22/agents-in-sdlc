---
title: "レッスン 5 - quality-checks スキルをカスタマイズして使う"
description: "既存の quality-checks スキルを確認し、結果のレポート形式をカスタマイズして、フィルター機能を検証します。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

コードを書くこと以外にも、考慮すべき作業があります。これまで手動でコードを検証し、指示ファイルを使って規約に沿ったコードを作成しました。次はテスト、lint、継続的インテグレーション (CI) のほかの作業を確認します。

こうした作業には**エージェントスキル**が適しています。スキルは、テストなどの操作を正しく実行する方法を Copilot に伝えます。

このレッスンでは、次のことを行います。

- 既存の `quality-checks` スキルを確認します。
- 結果の書式をカスタマイズします。
- スキルを再読み込みして実行します。

## シナリオ

Tailspin Toys では、ユニットテスト、lint、型チェックに `quality-checks` スキルを使っています。チームは、結果を読みやすくするためにレポートを改善したいと考えています。

## 指示、スクリプト、リソース

エージェントスキルは、再利用可能なタスクの指示、実行可能なスクリプト、補助リソースをまとめたものです。基本的には、スキル名のフォルダーと、その中にある `SKILL.md` ファイルで構成されます。Markdown ファイルには、スキルの名前と説明を定義する frontmatter、概要、呼び出すタイミングの指針を記載します。必要に応じて、スクリプトやほかのリソース用のサブフォルダーも追加できます。

> [!NOTE]
> スキルに追加のフォルダーやファイルは必須ではありません。Tailspin Toys の `quality-checks` スキルは、プロジェクトに既存のコマンドを使うため、`SKILL.md` のみで構成されています。

スキルは、チームで共有して再利用するプロジェクト資産として `.github/skills` に置くことも、ユーザーのスキル用フォルダー `~/.copilot/skills` に置くこともできます。

## スキルを確認する

Tailspin Toys チームがユニットテスト、lint、型チェックに使う `quality-checks` スキルを確認します。

1. Codespace に戻り、Codespaces のエディターで `.github/skills/quality-checks/SKILL.md` を開きます。
2. 先頭の `name` と `description` を読みます。説明は Copilot がスキルを呼び出すタイミングを判断するのに役立ちます。
3. 指示を読み、テストと lint の手順をどう案内するか確認します。
4. **Results output formatting** セクションがまだないことを確認します。

## 変更前にスキルを実行する

Copilot にスキルを実行させ、現在のレポートを確認します。

1. Interactive モードでフィルター機能の会話に戻ります。
2. 次のプロンプトを使います。

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. 最後に出力されるレポートを確認します。

## レポートをカスタマイズする

何を実行したか、成功したか、ツールがどのような結果を報告したかが分かるように、スキルを更新します。

1. `.github/skills/quality-checks/SKILL.md` に戻ります。
2. ファイルの最後に次のセクションを追加します。

   ```markdown
   ## Results output formatting

   Upon completion, report each command that ran and whether it passed, failed, or was blocked. Include test counts, durations, errors, warnings, and other metrics only when the tool reports them. Identify the next action for any failure or blocker, and never describe a skipped or incomplete check as passed.
   ```

3. ファイルが自動保存されたことを確認します。

## 更新したスキルを実行する

Copilot CLI は会話を再起動せずに、編集したスキルを再読み込みできます。

1. 次のコマンドを入力します。

   ```plaintext
   /skills reload
   ```

2. 先ほどと同じプロンプトを使います。

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. 新しいレポートを確認し、最初のレポートと比較します。

## まとめと次のステップ

既存のエージェントスキルをカスタマイズして実行しました。このレッスンでは、次のことを行いました。

- ユニットテスト、lint、型チェックに使う `quality-checks` スキルを確認しました。
- 結果の書式をカスタマイズしました。
- スキルを再読み込みして実行しました。

この変更は、フィルター機能とともに機能 PR に含まれます。次は、[Playwright MCP サーバーを使って Copilot がサイトを直接操作できるようにします][next-lesson]。

## スキルの例

以下は参考情報であり、追加の作業ではありません。

- [Agent Skills 仕様][skill-spec]
- [コントリビューションのワークフロー: `make-repo-contribution`][contribution-example]
- [要件ドキュメント: `prd`][prd-example]
- [図とエクスポートスクリプト: `drawio`][drawio-example]
- [ブラウザーテスト: `webapp-testing`][browser-example]

[previous-lesson]: ../4-custom-instructions/
[next-lesson]: ../6-mcp-playwright/
[skill-spec]: https://agentskills.io/specification
[contribution-example]: https://github.com/github/awesome-copilot/tree/main/skills/make-repo-contribution
[prd-example]: https://github.com/github/awesome-copilot/tree/main/skills/prd
[drawio-example]: https://github.com/github/awesome-copilot/tree/main/skills/drawio
[browser-example]: https://github.com/github/awesome-copilot/tree/main/skills/webapp-testing
