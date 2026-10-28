---
title: "レッスン 7 - QA エージェントを作成して使う"
description: "Issue の要件、quality-checks スキル、Playwright MCP を組み合わせた QA カスタム エージェントを作成します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

`quality-checks` スキルを使って自動チェックを実行し、Playwright MCP でブラウザーからフィルター機能を確認しました。次は、明確な QA プロセスを持つカスタム エージェントにこれらの機能をまとめます。

このレッスンでは、次の操作を行います。

- カスタム エージェントが指示、スキル、MCP ツールを使う方法を確認します。
- 再利用可能な品質保証プロファイルを作成して確認します。
- QA エージェントを選択し、フィルター機能の Issue に対する調査結果をレビューします。

## シナリオ

Tailspin Toys は、プル リクエスト (PR) を作成する前に、要件、コード品質、自動チェック、テスト カバレッジ、ブラウザー上の動作を一貫してレビューしたいと考えています。カスタム エージェントで QA プロセスを調整すれば、再利用可能なレポートを作成できます。

## カスタム エージェントとは

カスタム エージェントは、Markdown プロファイルで定義する専門化された Copilot です。プロファイルには、エージェントの目的、指示、利用できるツールを記述します。このワークショップでは、`.github/agents/qa.agent.md` に QA の役割を定義し、Copilot CLI で選択します。

これまでに使ったカスタマイズには、それぞれ異なる役割があります。リポジトリの指示はチームの標準を記述します。`quality-checks` スキルは繰り返し実行できるチェックをまとめます。Playwright MCP はブラウザー ツールを提供します。QA プロファイルは、要件を評価して調査結果を報告するために、Copilot がそれらの機能をどう使うかを示します。これらの仕組みを置き換えるものではなく、別の会話も必要ありません。

## QA プロファイルを作成する

機能の PR を作成する前に、再利用可能な QA プロファイルを Copilot に作成してもらいます。プロファイルには、QA で実行するチェックと守るべき境界の両方を記述します。

1. Codespace に戻り、フィルター機能の会話が Interactive モードであることを確認します。
2. 既定のエージェントに、次のカスタム エージェントを作成するよう依頼します。

   ```plaintext
   Create a custom agent named QA in .github/agents/qa.agent.md. It should check features against their issues and agreed requirements, follow the repository instructions, run the quality-checks skill, use Playwright MCP to verify behavior, and add tests when coverage is missing.

   Have it report each requirement as pass, fail, or blocked with supporting evidence. It must ask before changing implementation code, and it must not commit changes or open pull requests.

   Just create the profile for now so I can review it.
   ```

## プロファイルを確認する

新しいエージェントを使う前に、Copilot が意図した QA のワークフローと境界を反映したか確認します。検証だけを依頼したのに、エージェントが機能を変更してしまう事態を防げます。

1. `/diff` を入力し、`.github/agents/qa.agent.md` を開きます。
2. フロントマターを確認します。`description` は必須です。`name` は省略できますが、設定するとエージェントの表示名が明確になります。
3. プロファイルの指示を読み、QA が要件を確認し、リポジトリの指示に従い、`quality-checks` スキルと Playwright MCP を使うことを確認します。
4. QA が根拠を示して報告し、実装コードを変更する前に確認を求め、変更をコミットしたりプル リクエストを開いたりしないことを確認します。
5. これらの責任や境界がプロファイルに含まれていない場合は、続行する前に既定のエージェントに修正を依頼します。

## Issue に対して QA を実行する

Copilot CLI は会話の開始時にプロジェクトのエージェントを読み込みます。プロファイルを作成した後、同じフィルター機能の会話を再開し、QA を選択します。そうすると、Issue と計画の決定事項をコンテキストとして使えます。

1. 次のプロンプトでエージェントを有効にします。

   ```plaintext
   /agent QA
   ```

> [!NOTE]
> エージェントを作成した直後は、利用可能なエージェントの一覧に表示されない場合があります。コマンドを実行すると有効になります。

2. 次のプロンプトで、QA エージェントに機能をレビューするよう依頼します。

   ```plaintext
   Review the filtering feature against the issue and the decisions in our plan. Is it ready for a PR?
   ```

3. QA エージェントが作業を開始します。
4. 作業が完了したら、レポートを確認します。

## まとめと次のステップ

ワークフローに再利用可能な専門的な役割を追加し、その作業をレビューしました。このレッスンでは、次の操作を行いました。

- カスタム エージェントが指示、スキル、MCP ツールを使う方法を確認しました。
- 再利用可能な品質保証プロファイルを作成して確認しました。
- QA エージェントを選択し、フィルター機能の Issue に対する調査結果をレビューしました。

実装、スキルの更新、QA プロファイル、テスト、検証レポートの準備ができました。次は[機能 PR にまとめ、Agent Merge を使います][next-lesson]。

## リソース

- [Copilot CLI 用のカスタム エージェントを作成する][create-agents]
- [カスタム エージェントの構成][agent-config]

[previous-lesson]: ../6-mcp-playwright/
[next-lesson]: ../8-create-pull-request/
[create-agents]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli
[agent-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
