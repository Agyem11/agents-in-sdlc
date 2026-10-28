---
title: "演習 1 - カスタム指示 (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: Agent mode →][next-lesson]

生成 AI を活用するうえで、コンテキストは重要です。タスクを特定の方法で進める必要がある場合や、Copilot に知っておいてほしい背景情報がある場合は、そのコンテキストを利用できるようにします。Copilot と特定のコンテキストを共有する方法はいくつかあります。その中でも重要なのが、コード生成の指針となる[指示ファイル][instruction-files]です。

この演習では、次の操作を行います。

- リポジトリのカスタム指示とパスで対象を指定した指示ファイルを使って、プロジェクト固有のコンテキスト、コーディング ガイドライン、ドキュメント標準が Copilot に伝わる方法を確認します。
- 現在の指示を使って、フィルター機能の最初のデータ部分 (publishers ヘルパー) を生成します。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する標準を追加します。
- 同じプロンプトを再送信し、生成コードが新しい標準に従う様子を確認します。
- 指示の更新とフィルター機能の一部を `main` にコミットし、次の演習で Copilot が更新したガイダンスを使えるようにします。

> [!CAUTION]
> 生成コードが設定した標準から外れる場合があります。Copilot の動作は確率的です。この演習では、文字単位で同じ出力を得ることではなく、指示の更新後に動作がどのように変化するかを確認します。

## 指示ファイル

### シナリオ

Tailspin Toys には、開発プラクティスに関するガイドラインと要件があります。たとえば、次のとおりです。

- データ レイヤーには、必ずユニット テストを用意します。
- UI はダーク モードにし、現代的な見た目にします。
- TSDoc のドキュメント コメントを使ってコードに説明を追加します。
- 各ファイルの先頭に、ファイルの役割を説明するコメント ブロックを追加します。

指示ファイルを使って、Copilot がこうしたプラクティスに沿って作業できるようにします。

### カスタム指示

カスタム指示を使うと、コーディング スタイルや要件に関するコンテキストと好みを Copilot に伝えられます。規約、ライブラリ、コメントの書き方などを指定して、より適切な提案やコード スニペットを得ることができます。リポジトリ全体、または特定の種類のファイルに対して指示を設定できます。

指示ファイルには次の 2 種類があります。

- `.github/copilot-instructions.md` は、リポジトリに関するすべてのリクエストで Copilot に送信される単一の指示ファイルです。技術スタック、構築するものの概要、ベスト プラクティス、全体的なガイダンスなど、ほとんどのチャットや CLI リクエストに関係するプロジェクト レベルの情報を記述します。
- `.github/instructions/*.instructions.md` は、特定のタスクやファイルの種類に指示を適用します。TypeScript や Astro などの言語、UI コンポーネントの作成、ユニット テストの追加などに関するガイドラインを記述できます。

> [!NOTE]
> IDE では、指示ファイルは Copilot Chat のコード生成にのみ使われ、コード補完や次の編集候補には使われません。
>
> Copilot Chat、Copilot CLI、Copilot cloud agent は、リポジトリ レベルの指示ファイルと、`applyTo` フロントマターがある `*.instructions.md` ファイルの両方を使います。
>
> Copilot は AGENTS.md や CLAUDE.md など、ほかの標準の指示ファイルにも対応しています。詳細は[カスタム指示の対応状況][custom-instructions-support]を参照してください。

### 指示ファイルを管理するためのベスト プラクティス

指示ファイルの作成方法を詳しく説明することはこの演習の目的ではありません。サンプル プロジェクトに含まれるファイルは、一般的な方法を示すものです。大まかな指針は次のとおりです。

- `copilot-instructions.md` には、構築するもの、プロジェクトの構造、全体的なコーディング標準など、プロジェクト レベルのガイダンスを記述します。
- `*.instructions.md` は、ユニット テスト、Astro コンポーネント、データ レイヤーなどのファイルの種類や、特定のタスクに関する指示に使います。
- 自然な言葉で、明確なガイダンスとコードの良い例・悪い例を示します。

指示ファイルや AI の使い方に唯一の正解はありません。試しながら、プロジェクトに合う方法を見つけてください。

> [!TIP]
> GitHub Copilot を使うプロジェクトには、十分な指示ファイルを用意してください。このプロジェクトには、[UI 更新][ui-instructions]や [Astro][astro-instructions] など、さまざまなタスク向けのファイルがあります。
>
> Copilot に指示ファイルを生成してもらうこともできます。たとえば、VS Code の **Configure Chat → Generate Agent Instructions** や Copilot CLI の `/init` など、各環境で方法が異なります。必要な場合は、利用中の環境に合わせたレッスンで説明します。
>
> テンプレートや出発点を探していますか。[awesome-copilot][awesome-copilot] には指示ファイル、カスタム エージェント、その他のリソースが集められています。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## このプロジェクトのカスタム指示ファイルを確認する

このリポジトリに含まれる指示ファイルを確認しましょう。中心となる `copilot-instructions.md` と、タスク別の `*.instructions.md` ファイルがあります。エディターまたは GitHub Web UI で開きます。

1. `.github/copilot-instructions.md` を開きます。
2. プロジェクトの簡単な説明と、**Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。**Code standards** の下には **GitHub Actions Workflows** の指針もあります。これらは Copilot とのあらゆるやり取りに適用されます。
3. `.github/instructions` フォルダーを開き、Astro ファイル、Drizzle データ レイヤー、テストなどの指示を確認します。
4. `.github/instructions/unit-tests.instructions.md` を開き、先頭の `applyTo` フィールドを確認します。このフィールドはリポジトリのルートからの glob を指定します。ここでは、たとえば `**/*.test.ts` に一致する TypeScript のテスト ファイルに指示が適用されます。
5. このプロジェクトでユニット テストを作成する際の指示を確認します。
6. `.github/instructions/drizzle.instructions.md` を開き、末尾まで確認します。`unit-tests.instructions.md` などの指示ファイルや、プロジェクト内の既存ファイルへのリンクがあります。大きな指示を再利用しやすい小さなファイルに分け、Copilot が参考にできる例を示せます (ここにあるパスは指示ファイルからの相対パスです)。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションにはプロジェクトのコーディング標準が記載されていますが、コード内のドキュメントはまだ求めていません。次の手順で、TSDoc コメントとファイル ヘッダーの規則を追加します。

## 変更用のブランチを作成する

ここではベスト プラクティスに従い、変更用のブランチを作成します。

1. 前の演習で使用した Codespace に戻ります。
2. <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押して、新しいターミナルを開きます。
3. 新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions
   ```

## 指示を更新する前に Copilot Chat を使う

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の演習を始める前に Codespace に戻り、Copilot Chat パネルを開いて **New Chat** を選択し、新しい会話を始めます。モードとモデルの選択は演習によって異なります。必要な場合は、各手順で説明します。

カスタム指示の効果を確認するため、まず現在の指示を使ってプロンプトを送信します。その後、指示を更新して同じプロンプトを再送信し、違いを確認します。

1. Copilot が必要なコンテキストだけを取得するよう、前の演習で開いたエディター タブを閉じます。
2. `src/lib/publishers.ts` を開き、ヘルパーを配置する場所を Copilot に示します。
3. Chat ビューのエージェント ドロップダウンから **Agent** を選択し、Copilot がファイルを変更できるようにします。

   ![Chat ビューのエージェント ピッカー](../../../_images/shared-chat-mode-selector.png)

4. 次のプロンプトを送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot がプロジェクトを調べてコードを更新します。多くの場合、ヘルパー ファイルとそのテストに変更が及びます。
6. 提案されたヘルパーが、最初の引数に `db` クライアントを受け取る型付き関数で、発行元の型付き配列を返すことを確認します。これは `src/lib/*.ts` に適用される `.github/instructions/drizzle.instructions.md` のデータ レイヤー規約に基づいています。
7. 提案されたコードに TSDoc コメントとファイル レベルのコメント ヘッダーがないことを確認します。

> [!CAUTION]
> Copilot の動作は確率的なため、指示がなくてもコメントを追加する場合があります。その場合も問題ありません。指示を更新した後に一貫性が向上することを確認するのが、この演習の目的です。

## リポジトリ全体に適用する標準を追加する

`.github/copilot-instructions.md` は、プロジェクト レベルの情報を Copilot に提供します。コード提案を改善するため、コーディング標準を記述しましょう。

1. `.github/copilot-instructions.md` を開き直します。
2. **Code formatting requirements** セクションを探します。27 行目付近です。コーディング標準は記載されていますが、コード内のドキュメントに関する規則がまだありません。そのため、生成されたヘルパーにドキュメント コメントがありませんでした。
3. 既存の標準のすぐ下に次の Markdown を追加し、ファイル コメント ヘッダーと TSDoc コメントを追加するよう Copilot に指示します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> 前のレッスンで確認したように、リポジトリ全体に適用する指示は `.github/copilot-instructions.md` に、言語、ファイルの種類、特定のタスク向けの指示は `*.instructions.md` に記述します。プロジェクト全体の標準には、リポジトリ レベルのファイルが適しています。

## プロンプトを再送信して変化を確認する

1. Copilot Chat に戻り、**New Chat** を選択して会話を新しくします。
2. `src/lib/publishers.ts` を再び開き、Copilot に対象ファイルを示します。
3. 先ほどと**同じプロンプト**を送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 提案されたファイルの先頭に、次のようなコメント ブロックが追加されていることを確認します。

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. 提案された関数に、次のような TSDoc コメントが追加されていることを確認します。

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

新しいプロジェクト標準に従うよう Copilot に指示し、次の演習で使う実際のコードに適用できました。

## 指示をコミットしてブランチをプッシュする

指示ファイルもリポジトリのほかのファイルと同じように、ソース管理で管理します。変更をコミットしてブランチをプッシュしましょう。

1. <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押して、新しいターミナルを開きます。
2. 次のコマンドを実行し、指示とヘルパーへの変更があることを確認します。

   ```bash
   git status
   ```

3. 指示の更新と、ヘルパーの基盤のために Copilot が変更したすべてのファイルをステージしてコミットします。

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # git status にテストなどの追加変更が表示された場合は、それらのファイルも追加します。
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. ブランチをリポジトリにプッシュします。

   ```bash
   git push -u origin custom-instructions
   ```

## プル リクエストを作成してマージする

ブランチをプッシュしたら、バックログにあるドキュメント標準の Issue に関連付けたプル リクエストを作成します。手動で行う代わりに、プロジェクトに接続済みの GitHub ツールを使って、Copilot に Issue を探し、Issue をクローズする PR を作成してマージするよう依頼します。

> [!NOTE]
> この演習では、Copilot が GitHub 上で初めて操作します。プロジェクト テンプレートには GitHub ツールがすでに設定されています。Copilot がツールを初めて使うとき、VS Code で GitHub へのサインインを求められることがあります。画面の指示に従って認証してください。後の演習では、この接続の仕組み (Model Context Protocol) について学びます。

1. Codespace で Copilot Chat を開きます。
2. <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) を押して Copilot Chat ビューを開き、エージェント ピッカーで **Agent** が選択されていることを確認します。
3. 指示ファイルの更新に関する Issue を探し、現在のブランチから PR を作成して、指示の更新と新しい publishers ヘルパーの基盤の両方を説明するよう Copilot に依頼します。

   ```
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. GitHub の操作を許可するよう Copilot に求められた場合は、コマンドを確認して適切に **Allow** を選択します。
5. PR が作成されたら、次のプロンプトでマージを依頼し、ローカル ブランチを `main` に戻します。

   ```
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

6. GitHub の操作やシェル コマンドの実行を許可するよう求められた場合は、内容を確認して適切に **Allow** を選択します。

GitHub Copilot の支援でプル リクエストを作成してマージできました。

## まとめと次のステップ

Copilot が指示ファイルからコンテキストを得る方法を確認し、VS Code の Copilot Chat を使って次の操作を行いました。

- 既存の指示を使ってコード生成プロンプトを送り、Copilot の出力を確認しました。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する標準を追加しました。
- 同じプロンプトを再送信し、生成コードが新しい標準に従う様子を確認しました。
- 次の演習で使えるよう、指示の更新とヘルパーの基盤を `main` にコミットしました。

次は、Copilot がコードベース全体に新機能を追加する際に、[agent mode で指示を活用します][next-lesson]。

## リソース

- [GitHub Copilot のカスタム指示ファイル][instruction-files]
- [カスタム指示を作成するためのベスト プラクティス][instructions-best-practices]
- [Copilot に適切なカスタム指示を書くための 5 つのヒント][copilot-instructions-five-tips]
- [GitHub Copilot の個人用カスタム指示][personal-instructions]
- [Awesome Copilot — 指示ファイルなどのコレクション][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
