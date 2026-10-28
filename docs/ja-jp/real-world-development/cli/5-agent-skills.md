---
title: "レッスン 5 - quality-checks スキルをカスタマイズして使う"
description: "既存の quality-checks スキルを確認し、結果のレポート形式をカスタマイズして、フィルター機能の検証に使います。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

コードを書くこと以外にも、開発には多くの作業があります。これまでは、コードが正しく動作することを手動で検証し、指示ファイルを使ってコーディング規約に従うようにしてきました。テストや lint、継続的インテグレーション (CI) のほかの作業はどうでしょうか。

このような作業には、**エージェント スキル**が適しています。スキルを使うと、こうした操作を適切に実行する方法を Copilot に伝えられます。

このレッスンでは、次の操作を行います。

- 既存の `quality-checks` スキルを確認します。
- 結果の形式をカスタマイズします。
- スキルを再読み込みして実行します。

## シナリオ

Tailspin Toys では、ユニット テスト、lint、型チェックに `quality-checks` スキルを使っています。チームは、結果を読みやすくするためにレポートを改善したいと考えています。

## 指示、スクリプト、リソース

エージェント スキルは、エージェントが必要なときに読み込む、再利用可能なタスク指示、実行可能なスクリプト、補助リソースをまとめたものです。基本的には、スキル名が付いたフォルダーと、その中の `SKILL.md` で構成されます。Markdown ファイルには、スキルの内容を定義する `name` と `description` を含むフロントマター、概要、呼び出すタイミングのガイダンスを記述します。スキルのフォルダーには、スクリプトやその他のリソース用のサブフォルダーを含めることもできます。

> [!NOTE]
> スキルに追加のフォルダーやファイルは必須ではありません。Tailspin Toys の `quality-checks` スキルは、プロジェクトに既存のコマンドを使うため、`SKILL.md` のみで構成されています。

スキルをプロジェクトの `.github/skills` フォルダーに置くと、リポジトリの共有リソースとしてチームで再利用できます。また、ユーザー スキル フォルダー `~/.copilot/skills` に配置することもできます。

## スキルを確認する

Tailspin Toys チームがユニット テスト、lint、型チェックを実行するために作成した `quality-checks` スキルを確認しましょう。

1. Codespace に戻ります。Codespaces エディターで `.github/skills/quality-checks/SKILL.md` を開きます。
2. ファイルの先頭にある `name` と `description` を読みます。description は、Copilot がスキルをいつ呼び出すか判断するのに役立ちます。
3. 指示を読み、テストと lint の手順をどのように案内しているか確認します。
4. スキルに **Results output formatting** セクションがまだないことを確認します。

## 変更前にスキルを実行する

スキルは Copilot CLI から直接呼び出すことも、自然言語で指示することもできます。3つのチェックを実行するよう Copilot に依頼します。

1. フィルター機能の会話に戻り、Interactive モードにします。
2. 次のプロンプトを使います。

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. 最後に出力されるレポートを確認します。

## レポートをカスタマイズする

実行したチェック、その成否、ツールが実際に報告した内容を確認できるレポートに改善しましょう。

1. `.github/skills/quality-checks/SKILL.md` に戻ります。
2. ファイルの末尾に次のセクションを追加します。

   ```markdown
   ## Results output formatting

   Upon completion, report each command that ran and whether it passed, failed, or was blocked. Include test counts, durations, errors, warnings, and other metrics only when the tool reports them. Identify the next action for any failure or blocker, and never describe a skipped or incomplete check as passed.
   ```

3. ファイルは自動的に保存されます。

## 更新したスキルを実行する

Copilot CLI では、会話を再起動せずに編集したスキルを再読み込みできます。

1. 次を入力します。

   ```plaintext
   /skills reload
   ```

2. 先ほどとまったく同じプロンプトを使います。

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. 最後のレポートを確認し、最初のレポートと比較します。

## まとめと次のステップ

既存のエージェント スキルをカスタマイズして使いました。このレッスンでは、次の操作を行いました。

- ユニット テスト、lint、型チェックを実行する `quality-checks` スキルを確認しました。
- 結果の形式をカスタマイズしました。
- スキルを再読み込みして実行しました。

この変更は、フィルター機能とともに機能 PR に含めます。次は、[Playwright MCP サーバーを使って][next-lesson] Copilot がサイトを直接操作できるようにします。

## スキルの例

以下のコミュニティの例は参考資料であり、追加の課題ではありません。

- [Agent Skills 仕様][skill-spec]
- [コントリビューション ワークフロー: `make-repo-contribution`][contribution-example]
- [要件ドキュメント: `prd`][prd-example]
- [図と同梱のエクスポート スクリプト: `drawio`][drawio-example]
- [ブラウザー テスト: `webapp-testing`][browser-example]

[previous-lesson]: ../4-custom-instructions/
[next-lesson]: ../6-mcp-playwright/
[skill-spec]: https://agentskills.io/specification
[contribution-example]: https://github.com/github/awesome-copilot/tree/main/skills/make-repo-contribution
[prd-example]: https://github.com/github/awesome-copilot/tree/main/skills/prd
[drawio-example]: https://github.com/github/awesome-copilot/tree/main/skills/drawio
[browser-example]: https://github.com/github/awesome-copilot/tree/main/skills/webapp-testing
