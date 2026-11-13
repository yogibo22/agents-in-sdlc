---
title: "レッスン 9 - GitHub Copilot CLI のスラッシュコマンド"
description: "コンテキストの管理、モデルの選択、セッションの共有、並列作業、任意のクラウド委任に使うコマンドを確認します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

多くの CLI ツールと同様に、GitHub Copilot CLI には対話に使えるスラッシュコマンドが多数あります。これらのコマンドで、便利な機能、内部情報、追加の構成オプションを利用できます。これまでに `/diff`、`/mcp`、`/skills`、`/agent`、`/pr` などを使いました。ここでは、ほかの便利なコマンドを確認します。

このレッスンでは、次のことを行います。

- `/context` と `/compact` を使い、Copilot が会話のコンテキストを管理する方法を確認します。
- `/model` で利用可能なモデルを確認します。
- `/share` でセッションをエクスポートまたは共有する方法を学びます。
- 並列作業、worktree、cloud agent への委任に使う任意のコマンドを確認します。

## Copilot CLI のコンテキストを確認する

大規模または複雑なタスクでは、モデルのコンテキストウィンドウの上限に達する場合があります。Copilot CLI は必要に応じて会話を自動的に圧縮します。また、スラッシュコマンドを使って状態を確認したり、手動で圧縮したりできます。

1. Codespace に戻り、Copilot CLI が開いていない場合はリポジトリのルートから起動します。
2. 次のコマンドを入力します。

   ```plaintext
   /context
   ```

3. モデル、現在のトークン使用量、システム指示、ツール、メッセージ、空き領域にコンテキストがどう配分されているかを確認します。
4. 会話を圧縮します。

   ```plaintext
   /compact
   ```

5. もう一度 `/context` を実行し、結果を比較します。会話が短い場合は、大きな違いがないこともあります。

> [!NOTE]
> コンテキストウィンドウが埋まると、Copilot CLI は自動的にコンテキストを圧縮します。タイミングを選びたい場合は `/compact` を使います。無関係なタスクに移るときに新しい会話を始めるには、`/clear` または `/new` を使います。

## モデルを選ぶ

モデルごとに得意分野が異なり、開発者によって好みも異なります。Copilot CLI では、モデルの一覧を表示し、使用するモデルを選べます。

1. 次のコマンドを入力します。

   ```plaintext
   /model
   ```

2. 使用可能なモデルと利用状況を確認します。
3. 現在のモデルを維持するか、別のモデルを選ぶか、<kbd>Esc</kbd> を押して一覧を閉じます。

## セッションを共有する

チームで協力して学びを共有すると、人工知能 (AI) ツールの活用方法を改善できます。`/share` コマンドでセッションを Markdown または HyperText Markup Language (HTML) ファイルにエクスポートしたり、共有リンクや GitHub gist を作成したりできます。

1. `/help` を入力し、インストール済みのバージョンに表示される `/share` の選択肢を確認します。
2. セッションを共有する場合は、ローカルの Markdown ファイルにエクスポートする `/share file` など、目的に合った方法を選びます。
3. エクスポートした内容を送信または公開する前に確認します。セッションのエクスポートには、プロンプト、応答、プロジェクトの詳細が含まれる場合があります。

リンクや gist の公開は任意です。チームが共有を意図していないリポジトリや会話の内容は公開しないでください。

## 任意: 作業を拡張または委任する

ワークショップの主要な流れはここで完了です。Copilot CLI には、大規模なタスクに役立つ次のコマンドもあります。

- `/fleet` は、独立したサブタスクをサブエージェントに分配して並行実行できます。
- `/worktree` は、別のタスク用に分離された Git worktree を作成できます。
- `/delegate` は、非同期で作業してプルリクエストを作成することもある Copilot cloud agent にタスクを送信できます。

追加の worktree やリモート作業が発生するため、これらのコマンドは任意です。試す場合は、範囲が明確な新しいタスクから始め、通常のワークフローに沿って結果を確認してください。非同期エージェントの作業について詳しく知りたい場合は、[cloud agent ワークショップ][cloud-workshop]に進んでください。

## まとめと次のステップ

Copilot CLI のスラッシュコマンドを使って、構成、セッション共有、内部状態の確認ができます。このレッスンでは、次のことを行いました。

- `/context` と `/compact` を使ってコンテキストの管理方法を確認しました。
- `/model` で利用できるモデルを確認しました。
- `/share` でセッションをエクスポートまたは共有する方法を学びました。
- 並列作業、worktree、cloud agent への委任に使う任意のコマンドを確認しました。

Copilot CLI には、まだ多くのスラッシュコマンドがあります。次は、[学んだことを振り返り、次のステップを確認します][next-lesson]。

## リソース

- [Copilot CLI コマンドリファレンス][cli-reference]
- [Copilot CLI のコンテキスト管理][context-management]
- [Copilot cloud agent について][about-cloud-agent]

[previous-lesson]: ../8-create-pull-request/
[next-lesson]: ../10-review/
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[context-management]: https://docs.github.com/copilot/concepts/agents/copilot-cli/context-management
[about-cloud-agent]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-workshop]: ../../cloud/
