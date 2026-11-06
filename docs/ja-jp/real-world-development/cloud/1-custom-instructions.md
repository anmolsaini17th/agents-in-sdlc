---
title: "演習 1 - カスタム命令 (Cloud agent)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: Copilot cloud agent →][next-lesson]

生成 AI を利用する際は、コンテキストが重要です。タスクを特定の方法で実行する必要がある場合や、Copilot に知っておいてほしい背景情報がある場合は、そのコンテキストを参照できるようにします。[命令ファイル][instruction-files]を使うと、Copilot に「何を」するかだけでなく、「どのように」実行するかも伝えられます。

この演習では、次の操作を行います。

- リポジトリのカスタム命令とパスを指定した命令ファイルを通じて、プロジェクト固有のコンテキスト、コーディング ガイドライン、ドキュメント標準が Copilot に伝わる方法を確認します。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する標準を追加します。

> [!NOTE]
> VS Code および CLI のハーネスとは異なり、ここでは変更前後のプロンプトを比較しません。Copilot cloud agent は GitHub Issue に対して非同期で動作するため、リアルタイムの並列比較は困難です。このハーネスの後半で cloud agent が作成したプル リクエストをレビューするときに、命令ファイルの影響を確認します。

## 命令ファイル

### シナリオ

Tailspin Toys には、開発プラクティスに関するガイドラインと要件があります。たとえば、次のような内容です。

- データ レイヤーには、必ず単体テストを用意します。
- UI はダーク モードにし、現代的な見た目にします。
- TSDoc のドキュメント コメントをコードに追加します。
- 各ファイルの先頭に、ファイルの役割を説明するコメント ブロックを追加します。

命令ファイルを使い、Copilot がこれらのプラクティスに沿ってタスクを実行するための情報を提供します。

### カスタム命令

カスタム命令を使うと、コーディング スタイルや要件を Copilot に伝え、適切な提案やコード スニペットを生成できるようにコンテキストと設定を提供できます。好みのコーディング規則、ライブラリ、コードに含めるコメントの種類などを指定できます。リポジトリ全体、または特定のファイルの種類やタスクに適用する命令を作成できます。

命令ファイルには次の 2 種類があります。

- `.github/copilot-instructions.md` は、リポジトリへの**すべての**リクエストで Copilot に送信される単一の命令ファイルです。Copilot に送るチャットや CLI のほとんどに関係するプロジェクト レベルの情報を記載します。たとえば、使用している技術スタック、構築するものの概要、ベスト プラクティスなど、全体に適用するガイダンスを含められます。
- `.github/instructions/*.instructions.md` ファイルは、特定のタスクやファイルの種類に作成できます。TypeScript や Astro などの言語別ガイドラインや、UI コンポーネントまたは単体テストの作成などのタスク別ガイダンスを提供します。

> [!NOTE]
> IDE では、命令ファイルは Copilot Chat のコード生成にのみ使われ、コード補完や次の編集候補には使われません。
>
> Copilot Chat、Copilot CLI、Copilot cloud agent は、リポジトリ レベルの命令と `applyTo` フロントマターで対象を指定した `*.instructions.md` ファイルの両方を使ってコードを生成します。
>
> Copilot は AGENTS.md や CLAUDE.md など、ほかの標準に基づく命令ファイルも[サポートしています][custom-instructions-support]。

### 命令ファイルを管理するためのベスト プラクティス

命令ファイルの作成について詳しく説明することは、このワークショップの範囲外です。ただし、サンプル プロジェクトには代表的な方法を示す例が含まれています。基本的には、次のようにします。

- `copilot-instructions.md` には、構築する内容、プロジェクトの構造、全体的なコーディング標準など、プロジェクト レベルのガイダンスを記載します。
- `*.instructions.md` には、単体テスト、Astro コンポーネント、データ レイヤーなどのファイルの種類や、特定のタスクに関するガイダンスを記載します。
- 自然な言葉で、明確なガイダンスを書きます。コードのよい例と悪い例を示します。

命令ファイルの使い方や AI の使い方に唯一の正解はありません。プロジェクトで試しながら、最適な方法を見つけてください。

> [!TIP]
> GitHub Copilot を使用するすべてのプロジェクトには、十分な命令ファイルを用意してください。このプロジェクトには、[UI 更新][ui-instructions]や [Astro][astro-instructions]など、さまざまなタスク向けのファイルがあります。
>
> Copilot に命令ファイルを生成してもらうこともできます。各環境で方法は異なります (VS Code の **Configure Chat → Generate Agent Instructions**、Copilot CLI の `/init` など)。現在使用している環境のレッスンで、関連する手順を説明します。
>
> テンプレートや出発点を探している場合は、命令ファイル、カスタム エージェント、その他のリソースをまとめた [awesome-copilot][awesome-copilot] を確認してください。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## このプロジェクトのカスタム命令ファイルを確認する

このリポジトリには、中心となる `copilot-instructions.md` と、タスク別の `*.instructions.md` ファイルがいくつか含まれています。GitHub の Web UI またはエディターでファイルを開き、内容を確認します。

1. `.github/copilot-instructions.md` を開きます。
2. プロジェクトの簡単な説明や **Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。**Code standards** の下には、入れ子になった **GitHub Actions Workflows** のガイダンスがあります。これらの命令は、Copilot とのあらゆるやり取りに適用されます。
3. `.github/instructions` フォルダーを開きます。Astro ファイル、Drizzle データ レイヤー、テストなどに関する命令があります。
4. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` フィールドは、命令を適用するファイルをリポジトリのルートからの glob で指定します。ここでは、`**/*.test.ts` に一致する TypeScript のテスト ファイルが対象です。
5. このプロジェクトの単体テスト作成に関する命令を確認します。
6. 最後に `.github/instructions/drizzle.instructions.md` を開き、末尾までスクロールします。`unit-tests.instructions.md` などの命令ファイルや、プロジェクト内の既存ファイルへのリンクがあります。大きな命令セットを再利用可能な小さなファイルに分け、コード生成の参考となる例にリンクできます。ここに記載されたパスは、リポジトリのルートではなく命令ファイルを基準とします。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションにはプロジェクトのコーディング標準が記載されていますが、コード内のドキュメントはまだ必須ではありません。次の手順で、TSDoc コメントとファイル ヘッダーの規則を追加します。

## リポジトリの標準を追加する

`.github/copilot-instructions.md` は、Copilot にプロジェクト レベルの情報を提供するファイルです。コード提案を改善するために、TSDoc のドキュメント コメントとファイル レベルのコメント ヘッダーを追加する標準を記載します。

編集する前に、作業用のブランチを作成します。cloud agent は Issue の対象ブランチから命令を読み取りますが、変更を整理してコミットできるようにブランチを用意します。

1. Codespace のターミナルで、新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions-cloud
   ```

`.github/copilot-instructions.md` は Copilot にプロジェクト レベルの情報を提供します。標準を文書化して、コード提案が改善されるようにしましょう。

1. `.github/copilot-instructions.md` を再度開きます。
2. 27 行目付近にある **Code formatting requirements** セクションを見つけます。既存のコーディング標準を説明していますが、コード内のドキュメントに関する規則がまだないことを確認します。
3. 既存の標準の直後に、次の Markdown を追加します。これにより、ファイル ヘッダーと TSDoc ドキュメント コメントの追加を Copilot に指示できます。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> 前のレッスンで確認したように、リポジトリ全体に適用する `.github/copilot-instructions.md` と、言語、ファイルの種類、タスクに適用する `*.instructions.md` のどちらにも命令を記載できます。ここで追加したドキュメント コメント規則など、プロジェクト全体の標準はリポジトリ レベルのファイルに記載します。

## 命令の更新をコミットしてマージする

cloud agent は、Issue の対象ブランチから命令ファイルを読み込みます。次の演習で Copilot に Issue を割り当てると、`main` からブランチが作成されます。cloud agent が新しい命令を使えるようにするには、変更を `main` にマージする必要があります。

1. 変更をステージしてコミットします。

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. ブランチをプッシュします。

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. github.com で `custom-instructions-cloud` から `main` へのプル リクエストを作成してマージします。次の演習で Issue を割り当てるとき、cloud agent はこの命令を読み込みます。

> [!TIP]
> このワークショップでは `main` に直接作業してもかまいません。その場合はブランチを作成せず、`main` に直接コミットします。ブランチを使う手順は、実際のプロジェクトでの作業方法を反映しています。

## まとめと次のステップ

このプロジェクトの命令ファイルを確認し、リポジトリ全体に適用する標準を追加しました。cloud agent がこのハーネスで作成するプル リクエストをレビューするときに、この標準が適用される様子を確認できます。

次は [Copilot cloud agent に最初の Issue を割り当てます][next-lesson]。

## リソース

- [GitHub Copilot のカスタマイズに使う命令ファイル][instruction-files]
- [カスタム命令のベスト プラクティス][instructions-best-practices]
- [Copilot 向けのよりよいカスタム命令を書くための 5 つのヒント][copilot-instructions-five-tips]
- [Awesome Copilot — 命令ファイルなどのリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
