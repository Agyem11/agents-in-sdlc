---
title: "レッスン 8 - 機能の PR の作成とマージ"
description: "フィルター機能、指示、スキルの更新、QA プロファイル、テストをまとめてレビューし、PR を作成して Agent Merge を使用します。"
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

フィルター機能の実装、指示の更新、スキルの更新、品質保証 (QA) プロファイル、テストを1つのブランチに保存しました。これらをまとめてレビューし、プル リクエストを作成します。星評価のプル リクエスト (PR) は自分でマージしましたが、今回は **Agent Merge** にプロセスの管理を任せます。

> [!NOTE]
> 通常は、機能、指示の更新、スキルの更新、QA エージェントをいくつかの別々の PR に分けます。ワークショップを簡潔にするため、フィルター機能と品質ワークフロー全体を1つのセッションとブランチで進め、そのすべての作業をこの PR にまとめました。

このレッスンでは、次の操作を行います。

- Agent Merge の概要と、マージのライフサイクルを自動化する仕組みを学びます。
- 機能 PR 全体と検証の証拠を確認します。
- レビュー後にのみ Agent Merge を承認し、PR のマージを確認します。

## シナリオ

フィルター機能のワークフロー全体を通じて、Copilot を使って機能を計画、実装、検証しました。Tailspin Toys は、マージの承認を開発者の管理下に置きながら、残りの PR 作業を自動化したいと考えています。

## Agent Merge の概要

**Agent Merge** は、GitHub Copilot app で pull request をマージするまでの残りの作業を自動化します。有効にすると、アプリのセッションが pull request を読み取り、失敗した継続的インテグレーション (CI) チェックの修正、レビューコメントへの対応、必要に応じたリベースなど、マージを妨げている問題に対処します。GitHub でマージが許可されると、PR をマージします。バックグラウンドで動作し、アプリの再起動後も継続し、PR がマージされると自動的に無効になります。

ここまでは、自分で **Merge pull request** を選択していました。Agent Merge にその作業を任せることもできますが、コードの編集やマージには引き続き明示的な承認が必要です。マージを許可する前に、許可される操作と作業内容をレビューしてください。

## Agent Merge で PR を管理する

コードの作成とレビューが完了したので、Agent Merge に PR プロセスを管理させましょう。

1. エージェント ピッカーで **Default agent** を選択します。
2. **Create PR** の横にあるドロップダウンを選択します。
3. **Agent Merge** を選択します。ボタンが **Agent Merge** に変わります。
4. **Agent Merge** ボタンを選択してプロセスを開始します。ドロップダウンのオプションを選択しただけでは開始されません。

Agent Merge のプロセスが始まり、次の処理を行います。

- タイトルと説明を含むプル リクエストを作成します。
- Issue からセッションを開始した場合は、説明の本文で関連する Issue を参照します。
- ターゲット ブランチに対してリベースを行うか、マージ競合に対処します。
- CI を監視し、すべてのチェックが成功することを確認します。
- 他の開発者または Copilot code review からのフィードバックがないか PR を監視し、コメントを解決するために更新します。
- 必要に応じて、すべてのチェックが成功した後に PR を自動的にマージします。

すべてのチェックが成功したら、Agent Merge に PR もマージさせましょう。

5. マージを許可する前に、PR の差分と検証の証拠をレビューします。
6. **Agent Merge** の横にあるドロップダウンを選択します。
7. **Merge pull request** の横にチェックが付いていることを確認します。
8. エージェントに明示的にマージを依頼します。

    ```plaintext
    Merge the PR once all required checks and approvals pass. Do not bypass repository protections.
    ```

9. 進捗を確認し、報告された問題を解決します。続行する前に、PR の状態が **Merged** であることを確認してください。マージ権限が有効になっていることや、エージェントが動作中であることだけでは、マージ完了の確認になりません。

> [!IMPORTANT]
> Agent Merge は、リポジトリの保護や権限不足を回避しません。続行前に、それらの問題を解決してください。

## まとめと次のステップ

コードの生成、テストと検証、プル リクエストのプロセスなど、開発プロセスの複数の作業を自動化しました。具体的には、次の操作を行いました。

- Agent Merge の概要と、マージのライフサイクルを自動化する仕組みを学びました。
- 機能 PR 全体と検証の証拠を確認しました。
- レビュー後にのみ Agent Merge を承認し、PR がマージされたことを確認しました。

次は、[既存の Canvas を使い、トリアージ Canvas を作成して][next-lesson]、エージェントと一緒に作業を確認、計画、視覚化する方法を学びます。

## リソース

- [GitHub Copilot app で Issue とプル リクエストを管理する][managing-issues-prs]
- [GitHub Copilot app について][about-copilot-app]

[next-lesson]: ../9-canvases/
[managing-issues-prs]: https://docs.github.com/copilot/how-tos/github-copilot-app/managing-issues-and-pull-requests
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
