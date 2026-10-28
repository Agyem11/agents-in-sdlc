---
title: "レッスン 4 - カスタム指示で Copilot を導く"
description: "リポジトリの指示を確認し、ドキュメント作成の規約を追加して、フィルター機能のコードに適用します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

生成 AI を活用するうえで、コンテキストは重要です。タスクを特定の方法で進める必要がある場合、そのガイダンスを Copilot が利用できるようにします。[指示ファイル][instruction-files]には、求めるコードだけでなく、その構造も記述できます。フィルター機能を実装したので、Copilot が使用した指示を確認し、ドキュメント作成の規約を追加してコードに適用しましょう。

このレッスンでは、次の操作を行います。

- リポジトリの指示とパスで対象を指定した指示ファイルがエージェントに適用される方法を確認します。
- コーディング規約に従うよう、指示ファイルを更新します。
- 指示ファイルがコードに与える影響を確認します。

## シナリオ

Tailspin Toys には開発プラクティスに関するガイドラインがあります。たとえば、次のとおりです。

- コメントにはコードの内容を繰り返すのではなく、意図や自明でない判断を記述します。
- `db/` と `src/lib/` のエクスポートされた関数では、`db` 引数を注入できる場合も含め、目的、パラメーター、戻り値を TSDoc/JSDoc で説明します。
- 再利用可能な Astro コンポーネントでは `Props` の契約を文書化し、関連するコードが変わったらコメントも最新の状態にします。
- 既存のフォーマットと lint のガイダンスを維持します。

指示ファイルを使って、Copilot がこうしたプラクティスに沿って作業できるようにします。

## 指示ファイル

カスタム指示を使うと、コーディング スタイルや要件に関するコンテキストと好みを Copilot に伝えられます。規約、ライブラリ、コメントの書き方などを指定して、より適切な提案やコード スニペットを得ることができます。リポジトリ全体、または特定の種類のファイルに対して指示を設定できます。

指示ファイルには次の 2 種類があります。

- `.github/copilot-instructions.md` は、リポジトリに関するすべてのリクエストで Copilot に送信される単一の指示ファイルです。ほとんどのリクエストに関係するプロジェクト レベルの情報を記述します。
- `.github/instructions/*.instructions.md` は、特定の言語、ファイルの種類、タスクに関するガイドラインを記述します。

> [!NOTE]
> ほかの指示形式への対応状況は環境によって異なります。形式を利用する前に、[カスタム指示の対応状況に関するリファレンス][custom-instructions-support]を確認してください。

## プロジェクトのカスタム指示を確認する

スターター プロジェクトには、すぐに使える指示ファイルが用意されています。変更を加える前に、既存の内容を確認してみましょう。

1. Codespace に戻ります。
2. Codespaces エディターで (ターミナルではなく) `.github/copilot-instructions.md` を開きます。
3. プロジェクトの概要とコーディングに関するガイダンスを確認します。この指示は、このリポジトリでの Copilot とのすべてのやり取りに適用されます。
4. `.github/instructions` フォルダーを開き、ファイルを確認します。Astro ファイル、Drizzle データ レイヤー、テストなどの指示があります。
5. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` フィールドが、適用対象のファイルを指定する glob であることを確認します。
6. `.github/instructions/drizzle.instructions.md` を開き、ほかの指示ファイルやプロジェクト ファイルへのリンクを確認します。大きな指示を小さな再利用可能なファイルに分け、Copilot が参照できる例を示せます。

## チームのガイダンスに合わせて指示ファイルを更新する

既存のファイルは出発点として十分ですが、まだ補足が必要です。コアの `copilot-instructions.md` を変更して、新しく生成される TypeScript に TSDoc コメントを追加するよう指示します。

1. `.github/copilot-instructions.md` で、**Code formatting guidance** というセクションを探します。35 行目付近にあります。
2. 最後の箇条書きに次の内容を追加します。

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

変更は自動的に保存されます。

## 更新したガイダンスを使う

Copilot CLI は会話の開始時にリポジトリの指示を読み込みます。指示を保存した後、フィルター機能のコンテキストを保ったまま会話を再開し、Copilot に更新後のガイダンスを適用してもらいます。

1. フィルター機能の会話が開いている場合は、`/exit` を入力して終了します。Copilot CLI は会話を自動的に保存します。
2. リポジトリのルートにある `game-filters-cli` ブランチから、保存済みの会話を再開します。

   ```bash
   copilot --resume
   ```

   選択を求められた場合は、フィルター機能の会話を選択し、`game-filters-cli` ブランチがチェックアウトされていることを確認します。
3. Copilot に更新したガイダンスを適用するよう依頼します。

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. `/diff` を入力し、変更された TypeScript ファイルを確認します。新しい TSDoc コメントが追加され、コードを正確に説明していることを確認します。

## まとめと次のステップ

Copilot CLI が指示ファイルからコンテキストを取得する方法を確認し、新しい規約を機能に適用しました。具体的には、次の操作を行いました。

- リポジトリの指示とパスで対象を指定した指示ファイルがエージェントに適用される方法を確認しました。
- コーディング規約に従うよう指示ファイルを更新しました。
- 指示ファイルがコードに与える影響を確認しました。

次は、[再利用可能な quality-checks スキルをカスタマイズして実行し][next-lesson]、lint とテストを一貫して実行できるようにします。

## リソース

- [Copilot CLI にカスタム指示を追加する][instruction-files]
- [カスタム指示の対応状況][custom-instructions-support]
- [カスタム指示のベスト プラクティス][instructions-best-practices]
- [Awesome Copilot — 指示ファイルなどのコレクション][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
