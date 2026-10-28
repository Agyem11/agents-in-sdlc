---
title: "演習 1 - カスタム指示 (Cloud agent)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: Copilot cloud agent →][next-lesson]

生成 AI を活用するうえで、コンテキストは重要です。タスクを特定の方法で進める必要がある場合や、Copilot に知っておいてほしい背景情報がある場合は、そのコンテキストを利用できるようにします。[指示ファイル][instruction-files]を使うと、Copilot に何を実行するかだけでなく、どのように実行するかも伝えられます。

この演習では、次の操作を行います。

- リポジトリのカスタム指示とパスで対象を指定した指示ファイルを使って、プロジェクト固有のコンテキスト、コーディング ガイドライン、ドキュメント標準が Copilot に伝わる方法を確認します。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する標準を追加します。

> [!NOTE]
> VS Code や CLI のハーネスとは異なり、ここでは「変更前と変更後」のプロンプトを使いません。Copilot cloud agent は GitHub Issue に対して非同期で動作するため、影響をリアルタイムで並べて示すのが難しいためです。このハーネスの後半で cloud agent が作成したプル リクエストをレビューするときに、指示ファイルの効果を確認できます。

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
- `.github/instructions/*.instructions.md` は、言語やタスク (TypeScript や Astro、UI コンポーネントの作成、ユニット テストの追加など) に応じたガイドラインを記述します。

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

このリポジトリには、`copilot-instructions.md` と、タスク別の `*.instructions.md` ファイルがあります。エディターまたは GitHub の Web UI で開いて確認します。

1. `.github/copilot-instructions.md` を開きます。
2. プロジェクトの簡単な説明と、**Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。**Code standards** の下には **GitHub Actions Workflows** の指針もあります。これらは Copilot とのあらゆるやり取りに適用されます。
3. `.github/instructions` フォルダーを開き、Astro ファイル、Drizzle データ レイヤー、テストなどの指示を確認します。
4. `.github/instructions/unit-tests.instructions.md` を開き、先頭の `applyTo` フィールドを確認します。これはリポジトリのルートからの glob で、たとえば `**/*.test.ts` に一致する TypeScript のテスト ファイルに適用されます。
5. このプロジェクトでユニット テストを作成する際の指示を確認します。
6. `.github/instructions/drizzle.instructions.md` を開いて末尾まで確認します。`unit-tests.instructions.md` などほかの指示ファイルや、プロジェクトの既存ファイルへのリンクがあります。大きな指示を再利用しやすい小さなファイルに分け、Copilot が参考にできる例を示せます (ここにあるパスはリポジトリのルートではなく、指示ファイルからの相対パスです)。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションにはプロジェクトのコーディング標準が記載されていますが、コード内のドキュメントはまだ求めていません。次の手順で、TSDoc コメントとファイル ヘッダーの規則を追加します。

## リポジトリ全体に適用する標準を追加する

次に行う編集は、TSDoc コメントとファイル レベルのコメント ヘッダーを使ってコードにドキュメントを追加する、プロジェクト全体の規則です。後の演習で Issue に割り当てると、cloud agent がこの規則に従います。

編集の前に、作業ブランチを作成します (cloud agent は Issue の対象ブランチから指示を読み込みますが、変更を分けてコミットしておきます)。

1. Codespace のターミナルで、新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions-cloud
   ```

`.github/copilot-instructions.md` はプロジェクト レベルの情報を Copilot に提供します。コーディング標準を記述して、コード提案を改善しましょう。

1. `.github/copilot-instructions.md` を開き直します。
2. **Code formatting requirements** セクションを探します。27 行目付近です。コーディング標準は記載されていますが、生成コード内のドキュメントの規則はなく、そのため生成されたヘルパーにドキュメント コメントがありません。
3. 既存の標準のすぐ下に次の Markdown を追加し、ファイル コメント ヘッダーと TSDoc コメントを追加するよう Copilot に指示します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> 前のレッスンで確認したように、リポジトリ全体に適用する指示は `.github/copilot-instructions.md` に、言語、ファイルの種類、特定のタスク向けの指示は `*.instructions.md` に記述します。プロジェクト全体の規則には、リポジトリ レベルのファイルが適しています。

## 指示の更新をコミット、プッシュしてマージする

cloud agent は、Issue の対象ブランチにある指示ファイルを読み込みます。次の演習で割り当てる Issue に対して Copilot が指示を適用できるように、変更を `main` に反映します。

1. ステージしてコミットします。

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. ブランチをプッシュします。

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. github.com で `custom-instructions-cloud` から `main` に向けたプル リクエストを開き、マージします。次の演習で Issue を割り当てるとき、cloud agent はこれらの指示を読み込みます。

> [!TIP]
> このワークショップで `main` に直接作業する場合は、ブランチの作成を省略して、`main` に直接コミットしてもかまいません。ここでは、実際のプロジェクトに近い手順を体験できるよう、ブランチを使っています。

## まとめと次のステップ

Copilot が指示ファイルからコンテキストを得る方法を確認し、リポジトリ全体に適用する標準を `.github/copilot-instructions.md` に追加しました。このハーネスの後半では、cloud agent が作成するプル リクエストでこの標準が適用されているか確認します。

次は、[最初の Issue を Copilot cloud agent に割り当てます][next-lesson]。

## リソース

- [GitHub Copilot の指示ファイル][instruction-files]
- [カスタム指示を作成するためのベスト プラクティス][instructions-best-practices]
- [Copilot に適切なカスタム指示を書くための 5 つのヒント][copilot-instructions-five-tips]
- [Awesome Copilot — 指示ファイルなどのコレクション][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
