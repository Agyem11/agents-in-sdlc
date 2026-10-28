---
title: "レッスン 8 - 機能 PR を作成してマージする"
description: "フィルター機能、指示、スキルの更新、QA プロファイル、テストをまとめてレビューし、PR を作成して Agent Merge を使います。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

フィルター機能の実装、指示の更新、スキルの更新、品質保証 (QA) プロファイル、テストは、すべて 1 つのブランチに保存されています。まとめてレビューして、プル リクエストを作成しましょう。星評価のプル リクエスト (PR) は自分でマージしました。今回は **Agent Merge** にマージの作業を任せます。

> [!NOTE]
> 通常は、機能、指示の更新、スキルの更新、QA エージェントを複数の PR に分けます。ワークショップを簡潔にするため、フィルター機能と品質ワークフローを 1 つの会話とブランチにまとめ、この PR に含めています。

このレッスンでは、次の操作を行います。

- Agent Merge と、マージ ライフサイクルを自動化する方法を学びます。
- 機能全体の変更と検証の証拠を確認します。
- フィルター機能の PR を作成します。
- レビュー後にのみ Agent Merge を有効にし、PR がマージされたことを確認します。

## シナリオ

フィルター機能のワークフローでは、Copilot を使って機能を計画、実装、検証しました。Tailspin Toys は、マージの承認を開発者が管理したまま、残りの PR 作業を自動化したいと考えています。

## Agent Merge について

**Agent Merge** は、プル リクエストをマージするために必要な残りの作業を自動化します。有効にすると、Copilot は PR をブロックしている問題 (失敗した継続的インテグレーション (CI) チェックの修正、レビュー コメントへの対応、必要に応じたリベースなど) に取り組みます。その後、リポジトリが許可していれば GitHub の自動マージを有効にします。

これまでは自分で **Merge pull request** を選択していました。Agent Merge にこの作業を任せることもできます。Agent Merge を有効にする前に、変更をレビューして準備ができていることを確認してください。

## Agent Merge で PR を管理する

コードの作成が終わったら、まとめてレビューし、PR を作成して、残りの作業を Agent Merge に任せましょう。

1. Codespace に戻ります。
2. `/agent` を入力してエージェント ダイアログを開きます。
3. オプション一覧から **Default** を選択し、<kbd>Enter</kbd> を押します。
4. `/pr create` コマンドで新しい PR を作成します。
5. `/pr agentmerge` を使って Agent Merge を有効にします。
6. Copilot は PR の継続的インテグレーションの状況を監視します。すべて成功すると、マージを実行します。
7. Copilot から `PR #14 was squash-merged successfully.` のようなメッセージが表示されたことを確認します。

> [!IMPORTANT]
> Agent Merge は、必須の承認、ブランチ保護、マージ キュー、リポジトリ設定、権限の不足を回避しません。ブロックされた場合は報告された理由を確認し、リポジトリで許可されている場合はレビュー済みの変更を手動でマージします。

## まとめと次のステップ

コードの生成、テストと検証、プル リクエストの処理など、開発プロセスの一部を自動化しました。このレッスンでは、次の操作を行いました。

- Agent Merge と、マージ ライフサイクルを自動化する方法を学びました。
- 機能全体の変更と検証の証拠を確認しました。
- フィルター機能の PR を作成しました。
- レビュー後に Agent Merge を有効にし、PR がマージされたことを確認しました。

次は、コンテキスト、モデル、共有、cloud agent への任意の委任に使う[便利な Copilot CLI スラッシュ コマンド][next-lesson]を確認します。

## リソース

- [Copilot CLI でプル リクエストを管理する][manage-prs]
- [Copilot CLI コマンド リファレンス][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
