---
title: "レッスン 10 - 振り返りと次のステップ"
description: "Copilot CLI のワークフロー、2 つのプル リクエスト、再利用可能なカスタマイズ、追加のリソースを振り返ります。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

Tailspin Toys の一連のワークフローで GitHub Copilot CLI を使いました。次のことを行いました。

- Codespace を準備し、Copilot CLI をインストールし、プロジェクトを確認して、シード済みのフィルター Issue を見つけました。
- 星評価を追加し、転送されたブラウザーで結果を確認して、最初のプル リクエスト (PR) を手動でマージしました。
- フィルター Issue から開始し、Plan モードで実装方法を決め、Autopilot モードで構築して、Interactive モードでレビューしました。
- カスタム指示でエージェントを導き、既存の `quality-checks` スキルをカスタマイズして、ユニット テスト、lint、型チェックを実行しました。
- Playwright Model Context Protocol (MCP) サーバーを追加し、実際のブラウザーでフィルター機能を確認しました。
- 品質保証 (QA) カスタム エージェントを作成して選択し、要件、カバレッジ、スキルの結果、ブラウザーでの検証の証拠を評価しました。
- フィルター機能全体をレビューし、フィルター PR に Agent Merge を許可しました。
- コンテキスト、モデル、共有、cloud agent への任意の委任に使うスラッシュ コマンドを確認しました。

## リリースした内容

このワークショップでは、更新済みの `main` からそれぞれブランチを作成し、2 つの PR を作成しました。

1. **星評価:** 既存の `starRating` と、評価がない場合の状態をゲーム カードに表示します。
2. **フィルター機能と品質ワークフロー:** フィルターを実装し、指示を更新して機能に適用し、`quality-checks` のレポートをカスタマイズし、QA プロファイルを作成して関連テストを追加します。

フィルター機能の計画から PR 作成まで、同じ会話とブランチを使いました。ワークショップを効率化するため、これらの作業を 1 つの PR にまとめました。

## さまざまな検証方法

自動テスト、自分で行うブラウザー確認、MCP を使った Copilot のブラウザー操作など、複数の方法でコードを確認しました。`quality-checks` スキルはユニット テスト、lint、型チェックを実行し、新しい形式で結果を報告しました。QA は要件とテスト カバレッジのレビューにその結果を加えて、PR の前にまとめて確認しました。

追加するテストは、実際に不足しているカバレッジを補うものである必要があります。QA の結果、テストを追加する必要がない場合もあります。マージを許可する前に、コードと検証の証拠を確認してください。変更によって影響する証拠は、変更後に再確認します。

## ベスト プラクティス

Copilot に与えるコンテキストとツールが、作業に影響します。このワークショップでは、指示を更新し、スキルをカスタマイズし、QA プロファイルを作成し、MCP サーバーを設定しました。会話をまたいでカスタマイズを再利用し、チームのニーズに合わせて調整します。指示は標準を設定し、スキルは繰り返し実行するタスクを説明し、カスタム エージェントは専門的な役割を定義し、MCP サーバーは外部ツールに接続します。エージェントの要約だけでなく、実際の変更とツールの結果を確認してください。

タスクに合った**モードとモデル**を選びます。構築前に進め方を検討するときは **Plan**、範囲の絞られた変更を対話しながら進めるときは **Interactive**、明確なタスクを任せるときは **Autopilot** を使います。定型的な編集には速いモデルを、複雑な作業にはより高性能なモデルを選びます。

インフラストラクチャと同じように、コンテキストも重要です。何を、なぜ、どのように作るかを明確に説明すると、出力が大きく変わります。

## さらに試す

ワークショップの主要なワークフローは完了しました。次の CLI 機能も確認できます。

- `/review` でコード レビュー エージェントに変更を分析してもらいます。
- `/rubber-duck` で問題を説明し、別の視点から意見を得ます。
- `/fleet` で独立したサブタスクを並行して調整します。
- `/worktree` で別のタスクを分離します。
- `/delegate` でタスクを Copilot cloud agent に送ります。

## 次のステップ

どのツールでも、上達する一番の方法は使い続けることです。実際のコード、趣味のコード、長い間作りたかった小さなアプリなどで活用してください。チームと学びを共有し、お互いの経験から学びましょう。ドキュメントも引き続き確認してください。

ターミナルから Tailspin Toys を拡張したい場合は、[Foundry Backer Concierge のオプション シリーズ][foundry]に進みます。別の環境も比べたい場合は、[VS Code ワークショップ][vscode]、[GitHub Copilot app ワークショップ][app]、または [Copilot cloud agent ワークショップ][cloud]を確認してください。

## リソース

- [GitHub Copilot CLI について][about-cli]
- [Copilot CLI コマンド リファレンス][cli-reference]
- [Copilot CLI をカスタマイズする][customize-cli]
- [Copilot CLI でプル リクエストを管理する][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
