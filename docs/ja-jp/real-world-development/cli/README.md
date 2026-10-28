---
slug: ja-jp/real-world-development/cli
title: "GitHub Copilot CLI"
description: "Copilot CLI のモード、カスタマイズ、MCP ツール、プル リクエスト自動化を学びながら、Tailspin Toys に 2 つの変更を加え、検証してリリースします。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

**[GitHub Copilot CLI][about-copilot-cli]** は、エージェント型のコーディング アシスタントとして GitHub Copilot をターミナルで利用できます。コードベースを探索し、コードを生成し、コマンドを実行し、外部ツールに接続できます。すべてコマンド ラインから行えるため、グラフィカル エディターに切り替えずに作業を続けられます。

このワークショップでは、Tailspin Toys の一連のワークフローに沿って進めます。

1. GitHub Codespaces でプロジェクトを準備し、Copilot CLI をインストールして、プロジェクトの概要を把握します。
2. 星評価の小さな変更を加え、ブラウザーで確認して、最初のプル リクエスト (PR) を手動でマージします。
3. フィルター機能の Issue から開始し、Plan モードで実装方法を決め、Autopilot モードで構築して、Interactive モードでレビューします。
4. リポジトリの指示を更新し、フィルター機能に適用します。
5. 既存の `quality-checks` スキルをカスタマイズし、プロジェクトのチェックを実行します。
6. Playwright Model Context Protocol (MCP) サーバーを追加し、ブラウザーでフィルター機能を確認します。
7. 品質保証 (QA) カスタム エージェントを作成して、要件、テスト カバレッジ、検証の証拠をレビューします。
8. フィルター機能全体をレビューし、その PR で Agent Merge を使用します。
9. コンテキスト、モデル、共有、任意の cloud agent への委任に使う便利なスラッシュ コマンドを確認します。

ワークショップでは、星評価の PR と、指示の更新、スキルの更新、QA プロファイル、テストを含むフィルター機能の PR の 2 つを作成します。フィルター機能と品質ワークフローは、会話とブランチを共有するため、各ツールを試しながら作業を積み重ねられます。

## レッスン

| レッスン | トピック | 説明 |
| ------ | ----- | ----------- |
| [0. 前提条件][ex0] | セットアップ | リポジトリと Codespace を作成します |
| [1. Copilot CLI のインストール][ex1] | インストール | Copilot CLI をインストールして認証し、使い方を確認します |
| [2. 星評価の追加: 小さな成果を得る][ex2] | 最初の変更 | 既存の評価と評価なしの状態を表示し、最初の PR をマージします |
| [3. エージェント モード: Plan と Autopilot][ex3] | エージェント モード | Issue から機能を計画し、Autopilot で構築して、Interactive モードでレビューします |
| [4. カスタム指示で Copilot を導く][ex4] | コンテキスト | 指示を確認して更新し、フィルター機能に適用します |
| [5. quality-checks スキルをカスタマイズして使う][ex5] | 繰り返し可能なチェック | 既存のスキルを調べ、レポート形式を変更して実行します |
| [6. Playwright MCP で機能を検証する][ex6] | ブラウザーでの確認 | CLI で MCP を設定し、フィルターの動作を確認します |
| [7. QA エージェントを作成して使う][ex7] | 要件とカバレッジ | 専門プロファイルを作成して選択し、最終検証の証拠を集めます |
| [8. 機能 PR を作成してマージする][ex8] | レビューとマージ | 変更全体をレビューし、PR を作成して Agent Merge を使います |
| [9. GitHub Copilot CLI のスラッシュ コマンド][ex9] | CLI 機能 | コンテキスト、モデル、共有、cloud agent への任意の委任を確認します |
| [10. 振り返りと次のステップ][ex10] | まとめ | ワークフロー、再利用可能なカスタマイズ、参考情報を振り返ります |
| [オプション: Foundry を組み込む][foundry] | ホスト型エージェント | モデルを準備し、カタログに基づくエージェントをデプロイして、Web サイトに接続します |

## 前提条件

このワークショップに参加する前に、次を準備してください。

- [ ] **Copilot Student、Pro、Pro+、Business、Enterprise** のいずれかの有効なプランがある GitHub アカウント
- [ ] リポジトリと Codespace を作成する権限
- [ ] ターミナルまたはコマンド ライン操作の基本的な知識

> [!TIP]
> 有料プランがありませんか。認証済みの学生は [GitHub Education][student-plan] を通じて GitHub Copilot を無料で利用できます。**Copilot Student** プランには、このワークショップで使用する agent、MCP、コードレビュー、Copilot CLI の機能が含まれています。

> [!NOTE]
> Copilot Business または Copilot Enterprise を使用している場合は、管理者が Copilot CLI を有効にしていることを確認してください。

## はじめに

**[前提条件から開始 →][ex0]**

[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[student-plan]: https://github.com/education/students
[ex0]: 0-prerequisites/
[ex1]: 1-install-copilot-cli/
[ex2]: 2-add-star-rating/
[ex3]: 3-agent-modes/
[ex4]: 4-custom-instructions/
[ex5]: 5-agent-skills/
[ex6]: 6-mcp-playwright/
[ex7]: 7-qa-agent/
[ex8]: 8-create-pull-request/
[ex9]: 9-cli-power-tools/
[ex10]: 10-review/
[foundry]: 8-foundry-agent/
