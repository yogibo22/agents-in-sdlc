---
title: "演習 5 - GitHub Copilot の作業を反復して改善する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

## 作業をレビューする

このラボでは、ユーザー体験の改善と機能追加に関する複数のタスクを GitHub Copilot に依頼しました。コードへのドキュメント追加、デザインチーム向けの関連ゲーム機能、高コントラストとライトモードの切り替えを実装しました。ここでは、生成された変更を確認し、必要に応じて Copilot にフィードバックを送って改善します。

### シナリオ

生成 AI を導入しても、ソフトウェア設計と DevOps の基本は変わりません。生成されたコードをレビューし、通常の DevOps プロセスに沿って作業します。チームがレビューを始める前に、Copilot が作成したドキュメント、関連ゲーム機能、アクセシビリティ機能を確認します。

## GitHub Copilot cloud agent とセキュリティ

Copilot cloud agent は非同期かつ人間の監視なしにタスクを実行するため、安全性を保つための制約があります。

- Copilot はリポジトリへの読み取りアクセスのみを持ち、書き込みは作業用のブランチに限られます。
- cloud agent は GitHub Actions 内で、作業用の独立した一時環境を作成します。
- GitHub Actions ワークフローの実行には、人間による承認が必要です。
- Model Context Protocol (MCP) サーバーなど、[外部リソースへのアクセスは既定で制限されています](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall)。

## 生成されたドキュメントをレビューする

最初に、GitHub Copilot cloud agent がコードにドキュメントを追加したプルリクエスト (PR) を、GitHub.com の標準的な PR 画面で確認します。

> [!NOTE]
> PR を開くと、GitHub Copilot がファイアウォールでブロックされたという警告が表示されることがあります。Copilot は MCP サーバーなどの外部リソースに既定で制限付きでアクセスするため、これは**想定される動作**です。必要であれば、[Copilot cloud agent のファイアウォールをカスタマイズまたは無効化][agent-firewall]できます。

1. github.com のリポジトリに戻ります。
2. **Pull Requests** を選択して PR の一覧を開きます。
3. **Add missing documentation** などのタイトルの PR を開きます。

> [!NOTE]
> Copilot がまだ作業中の場合、PR のタイトルには **[WIP]** が含まれます。完了するまで待ちます。数分かかることがあります。

4. **Files changed** タブを選択し、変更をレビューします。

   ![Files changed タブ](../../../_images/shared-pr-files-changed.png)

5. TSDoc コメントやその他のドキュメントが追加されたコードを確認します。具体的な変更内容は状況によって異なります。

   レビューでは、演習 2 で確認した指示ファイルから反映される TSDoc コメント、TypeScript の規約、ファイルヘッダーに注目します。

6. 問題がなければ **Conversation** タブに戻り、下にスクロールします。
7. ワークフローが承認待ちになっていることを示す表示を探します。
8. ワークフローが承認待ちの場合は、**Approve and run workflows** を選択します。

   ![ワークフローを承認して実行する操作](../../../_images/shared-approve-workflows.png)

9. PR の checks セクションにワークフローが登録されたことを確認します。問題がなければ、単一 Astro アプリのプロジェクトチェックが成功します。完了まで数分かかる場合があります。

## GitHub Copilot に変更を依頼する

Copilot と PR 上でやりとりできます。チームメンバーと同じように PR のコメントやコードのインラインコメントで Copilot にメンションすると、コメントに対応する新しいセッションが始まります。生成結果は確率的なため、必ずしも同じプロンプトを使う必要はありません。次のような変更を依頼できます。

- 各コードファイルの先頭に、役割を説明するコメントヘッダーを追加します。
- TypeScript と Astro ファイルに TSDoc コメントを追加します。
- Astro アプリの構成を説明する README を作成します。

1. **@copilot** をメンションし、上記の例などを使って、生成されたドキュメントの変更をコメントで依頼します。
2. **View Session** を選択し、Copilot が作業する様子を確認します。Copilot は更新用の新しいセッションを開始します。
3. **Back to pull request** を選択すると PR に戻れます。

   ![Back to pull request](../../../_images/ex7-back-to-pr.png)

4. Copilot の作業完了後、PR に新しいコミットが追加されたことを確認します。
5. **Files changed** タブを選択し、変更をレビューします。

納得できるまで反復できます。満足したら PR をドラフトからレビュー可能な状態に変更し、`main` ブランチにマージします。

![PR をレビュー可能な状態に変更する操作](../../../_images/ex7-ready-for-review.png)

## 関連ゲーム機能をレビューする

ゲーム詳細ページに関連ゲームを表示する Issue に対し、Copilot が作成した PR を確認します。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Show related games on the game details page** などのタイトルの PR を選択します。
4. **Files changed** タブを選択して、生成されたコードをレビューします。
5. 問題がなければ **Conversation** タブに戻り、下にスクロールします。
6. ワークフローが承認待ちになっていることを示す表示を探します。
7. ワークフローが承認待ちの場合は、**Approve and run workflows** を選択します。

   ![ワークフローを承認して実行する操作](../../../_images/shared-approve-workflows.png)

8. PR の checks セクションにワークフローが登録されたことを確認します。問題がなければ、単一 Astro アプリのプロジェクトチェックが成功します。完了まで数分かかる場合があります。
9. **任意:** Codespace でこのブランチに切り替えて、関連ゲーム機能を手動テストできます。Codespace でターミナルを開き、Copilot が作成したブランチ名 (例: **copilot/fix-8**) に置き換えて、次のコマンドを実行します。

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

Copilot が関連ゲーム機能を実装しました。PR の **Conversation** タブでコメントを追加すると、cloud agent に変更を依頼して反復できます。表示する関連ゲームの数を調整する、コメントヘッダーや TSDoc コメントを追加する、といった依頼ができます。この Issue はカスタム指示を追加する**前**に割り当てたため、指示の変更を反映させたい場合は追加で依頼します。

## アクセシビリティ機能をレビューする

最後に、アクセシビリティ用カスタムエージェントが実装した機能をレビューします。この PR には、演習 3 で依頼した高コントラストモードと、演習 4 で Mission control から追加依頼したライトモードの両方が含まれます。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Add high contrast mode to website** などのタイトルの PR を選択します。

> [!NOTE]
> Copilot がまだ作業中の場合、PR のタイトルには **[WIP]** が含まれます。完了するまで待ちます。数分かかる場合があります。

4. **Files changed** タブを選択し、生成されたコードをレビューします。
5. 実装を確認し、特に次の点を確認します。
   - 表示モードを切り替える UI コンポーネント。
   - ユーザー設定を保存する local storage の使用。
   - 高コントラストモードとライトモードの CSS またはスタイルの変更。
   - Accessible Rich Internet Applications (ARIA) ラベルなどのアクセシビリティ属性、キーボード操作。
   - モード切り替えを管理する JavaScript/TypeScript コード。

6. 問題がなければ **Conversation** タブに戻り、下にスクロールします。
7. ワークフローが承認待ちになっていることを示す表示を探します。
8. ワークフローが承認待ちの場合は、**Approve and run workflows** を選択します。

   ![ワークフローを承認して実行する操作](../../../_images/shared-approve-workflows.png)

9. PR の checks セクションにワークフローが登録されたことを確認します。問題がなければ、単一 Astro アプリのプロジェクトチェックが成功します。完了まで数分かかる場合があります。
10. **任意:** Codespace でこのブランチに切り替えてアクセシビリティ機能を手動でテストできます。Copilot が作成したブランチ名に置き換えて、次のコマンドを実行します。

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

   アプリを起動し、ブラウザーで高コントラストとライトモードのトグルを試します。ページを再読み込みしても設定が保持されることを確認します。

アクセシビリティカスタムエージェントが、アクセシビリティのベストプラクティスに沿って機能を実装するよう Copilot を導いたことを確認します。アクセシビリティに関する懸念や改善点があれば、前の PR と同様にコメントで **@copilot** に更新を依頼できます。

## 任意の演習 - さらに作業を委任する

cloud agent に実際のバックログ作業を任せ、自分は別の作業を進める方法を試します。リポジトリに Issue を作成し、Copilot に割り当てます。たとえば、次のタスクを依頼できます。

- ゲーム詳細ページに支援者向けの問い合わせフォームを追加します。
- ゲーム一覧ページにページネーションを実装します。
- データアクセスヘルパーに入力検証とエラー処理を追加します。

## まとめ

Cloud agent ハーネスを完了しました。このレッスンでは、次のことを行いました。

- **リポジトリに用意されたカスタム指示ファイルを確認し**、cloud agent の出力に与える影響を確認しました。
- **Copilot cloud agent に Issue を割り当て**、環境の準備、計画、非同期の作業を確認しました。
- **アクセシビリティ用カスタムエージェントを作成して使い**、高コントラストとライトモードのトグルを追加しました。
- **Copilot agents ページを Mission control として使い**、アクセシビリティセッションを監視して作業の途中で方向付けしました。
- **cloud agent のプルリクエストをレビューして反復し**、`@copilot` に変更を依頼して、ワークフローを承認しました。

## レビューと次のステップ

Cloud agent ハーネスを完了しました。別の環境で同様のシナリオを試したい場合は、ほかのハーネスに進みます。

- 🖥️ **[VS Code ハーネス](../../vscode/)** — IDE 内で Copilot Chat のエージェントモードと MCP の連携を確認します。
- 💻 **[CLI ハーネス](../../cli/)** — Plan モードと Autopilot モード、エージェントスキル、QA カスタムエージェント、Playwright MCP、Agent Merge、cloud agent に戻る任意の `/delegate` 連携を使い、ターミナルから2つの変更をレビューしてリリースします。

自分のリポジトリでは、次の方法も試せます。

- リファクタリングやテストカバレッジの Issue を cloud agent に割り当てます。
- 別の分野向けのカスタムエージェントを作成します。
- 別の技術スタック向けに `copilot-setup-steps.yml` を設定します。

## リソース

- [GitHub Copilot][github-copilot]
- [Copilot agents について][copilot-agents]
- [GitHub Issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent のセットアップワークフローに関するベストプラクティス][cloud-agent-best-practices]
- [Copilot cloud agent のファイアウォールを構成する][agent-firewall]

---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
