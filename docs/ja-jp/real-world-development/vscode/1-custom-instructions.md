---
title: "演習 1 - カスタム命令 (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: エージェント モード →][next-lesson]

生成 AI を利用する際は、コンテキストが重要です。タスクを特定の方法で実行する必要がある場合や、Copilot に知っておいてほしい背景情報がある場合は、そのコンテキストを参照できるようにします。Copilot に特定のコンテキストを伝える方法はいくつかあります。その中心となるのが[命令ファイル][instruction-files]です。命令ファイルを使うと、コード生成の指針を Copilot に伝えられます。

この演習では、次の操作を行います。

- リポジトリのカスタム命令とパスを指定した命令ファイルを通じて、プロジェクト固有のコンテキスト、コーディング ガイドライン、ドキュメント標準が Copilot に伝わる方法を確認します。
- 現在の命令を使って、絞り込み機能の最初のデータ部分 (発行元ヘルパー) を作成します。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する標準を追加します。
- 同じプロンプトを再実行し、生成されるコードに新しい標準が反映されることを確認します。
- 次の演習で Copilot が最新のガイダンスを使用できるように、命令の変更と絞り込み機能を `main` にコミットします。

> [!CAUTION]
> 生成されたコードが、設定したすべての標準に従うとは限りません。Copilot の動作は確率的です。この演習では、出力の一文字一文字を一致させるのではなく、命令を更新した後の動作の変化を確認します。

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

- `.github/copilot-instructions.md` は、リポジトリへの**すべての**リクエストで Copilot に送信される単一の命令ファイルです。このファイルには、Copilot に送るチャットや CLI のほとんどに関係するプロジェクト レベルの情報を記載します。使用する技術スタック、構築する内容の概要、ベスト プラクティスなど、全体に適用するガイダンスを含められます。
- `.github/instructions/*.instructions.md` ファイルは、特定のタスクやファイルの種類に作成できます。TypeScript や Astro などの言語別ガイドラインや、UI コンポーネントまたは単体テストの作成などのタスク別ガイダンスを提供します。

> [!NOTE]
> IDE では、命令ファイルは Copilot Chat のコード生成にのみ使われ、コード補完や次の編集候補には使われません。
>
> Copilot Chat、Copilot CLI、Copilot cloud agent は、リポジトリ レベルの命令と `applyTo` フロントマターで対象を指定した `*.instructions.md` ファイルの両方を使ってコードを生成します。
>
> Copilot は AGENTS.md や CLAUDE.md など、ほかの標準に基づく命令ファイルも[サポートしています][custom-instructions-support]。

### 命令ファイルを管理するためのベスト プラクティス

命令ファイルの作成方法を詳しく説明することは、この演習の範囲外です。ただし、サンプル プロジェクトには代表的な方法を示す例が含まれています。基本的には、次のようにします。

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

## 変更用ブランチを作成する

ここでは、一般的なベスト プラクティスに従って変更用ブランチを作成します。

1. 前の演習で使った Codespace に戻ります。
2. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して、新しいターミナルを開きます。
3. 新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions
   ```

## 命令を更新する**前**に Copilot Chat を使う

> [!TIP]
> **Copilot Chat を開く**
>
> 次の演習を始める前に Codespace に戻り、Copilot Chat パネルを開いて **New Chat** を選択し、新しい会話を始めます。モードとモデルの選択は演習によって異なります。必要な場合は、各手順で説明します。

カスタム命令の影響を確認するため、まず現在の命令ファイルを使った状態でプロンプトを送信します。その後、命令を更新して同じプロンプトを再送信し、違いを確認します。

1. 前の演習で開いたエディター タブを閉じ、Copilot が必要なコンテキストだけを使うようにします。
2. `src/lib/publishers.ts` を開き、ヘルパーを置く場所を Copilot に伝えます。
3. Chat ビューのエージェント ドロップダウンから **Agent** を選択し、Copilot がファイルを変更できるようにします。

   ![Chat ビューでエージェント ピッカーを表示したスクリーンショット](../../../_images/shared-chat-mode-selector.png)

4. 次のプロンプトを送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot はプロジェクトを調べ、ヘルパー ファイルやテストなど、コードの更新が必要な箇所に変更を適用します。
6. 提案されたヘルパーは、最初の引数として `db` クライアントを受け取る型付き関数で、発行元の型付き配列を返すことを確認します。これは `src/lib/*.ts` に適用される `.github/instructions/drizzle.instructions.md` のデータ レイヤー規則に基づいています。
7. 提案されたコードに TSDoc のドキュメント コメントとファイル レベルのコメント ヘッダーが**ない**ことを確認します。

> [!CAUTION]
> Copilot の動作は確率的なため、命令がなくてもドキュメント コメントが追加される場合があります。その場合も問題ありません。この演習では、命令を更新した後に動作が一貫して改善されることを確認します。

## 新しいリポジトリ標準を追加する

`.github/copilot-instructions.md` は、Copilot にプロジェクト レベルの情報を提供します。コード提案を改善するために、リポジトリのコーディング標準を文書化しましょう。

1. `.github/copilot-instructions.md` を再度開きます。
2. **Code formatting requirements** セクション (27 行目付近) を見つけます。既存のコーディング標準を説明していますが、生成されたヘルパーにドキュメント コメントがなかったように、コード内のドキュメントに関する規則はまだありません。
3. ファイル コメント ヘッダーと TSDoc のドキュメント コメントを追加するよう Copilot に指示するために、既存の標準の直後に次の Markdown を追加します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> 前のレッスンで確認したように、リポジトリ全体に適用する `.github/copilot-instructions.md` と、言語、ファイルの種類、タスクに適用する `*.instructions.md` のどちらにも命令を記載できます。ここで追加したドキュメント コメント規則など、プロジェクト全体の標準はリポジトリ レベルのファイルに記載します。

## プロンプトを再実行して変化を確認する

1. Copilot Chat に戻り、**New Chat** を選択して会話を新しくします。
2. `src/lib/publishers.ts` を選択し、Copilot が適切なファイルを認識するようにします。
3. 前と**同じプロンプト**を送信します。

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

Copilot をガイドして新しい標準に従わせ、次の演習で活用する実際のコードに適用しました。

## 命令とブランチをコミットしてプッシュする

命令ファイルもリポジトリ内のほかのアセットと同じように、ソース管理で管理します。変更をコミットしてブランチをリポジトリにプッシュしましょう。

1. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して、新しいターミナルを開きます。
2. ターミナルで `git status` を実行し、命令とヘルパーの変更が存在することを確認します。

   ```bash
   git status
   ```

3. ターミナルで命令ファイルと Copilot がヘルパーの基礎作成に使用したファイルをステージしてコミットします。

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # If git status shows additional supporting updates (for example tests), add those files too.
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. ブランチをリポジトリにプッシュします。

   ```bash
   git push -u origin custom-instructions
   ```

## プル リクエストを作成してマージする

ブランチをプッシュしたら、バックログにあるドキュメント標準の Issue に関連付けてプル リクエストを作成します。手動で操作する代わりに、すでにプロジェクトに接続されている GitHub ツールを使って Copilot に依頼できます。Issue を探し、Issue をクローズする PR を作成して、マージするように指示します。

> [!NOTE]
> ここでは初めて Copilot に GitHub 上の操作を実行させます。プロジェクト テンプレートでは GitHub のツールがあらかじめワークスペースに設定されているため、追加のセットアップは不要です。Copilot が初めて GitHub ツールを使用するとき、VS Code で GitHub へのサインインを求められることがあります。指示に従って許可してください。後の演習では、Model Context Protocol について詳しく学びます。

1. Codespace で Copilot Chat を開きます。
2. <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) を押して Copilot Chat ビューを開き、エージェント ピッカーで **Agent** が選択されていることを確認します。
3. 命令ファイルを更新する Issue を探し、現在のブランチから PR を作成するよう Copilot に依頼します。PR では命令の更新と、今後の絞り込み機能のために作成した発行元ヘルパーの両方を説明します。

   ```plaintext
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot が Issue を探して PR を作成します。
5. GitHub の操作を許可するよう Copilot に求められた場合は、コマンドを確認してから **Allow** を選択します。
6. PR が作成されたら、次のプロンプトを使ってマージし、ブランチを `main` に戻すよう Copilot に依頼します。

   ```plaintext
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. GitHub の操作やシェル コマンドを許可するよう Copilot に求められた場合は、コマンドを確認してから許可します。

GitHub Copilot の支援でプル リクエストを作成してマージできました。

## まとめと次のステップ

このプロジェクトの命令ファイルから Copilot がコンテキストを取得する方法を確認し、VS Code の Copilot Chat で次の作業を行いました。

- コード生成のプロンプトを送信し、**既存の**命令を使って Copilot が生成する内容を確認しました。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する標準を追加しました。
- 同じプロンプトを再実行し、提案されたコードに新しい標準が反映されることを確認しました。
- 次の演習で使えるように、命令の更新とヘルパーの基礎を `main` にコミットしました。

次は、Copilot がコードベース全体に新機能を追加するときに、[エージェント モードで命令を活用します][next-lesson]。

## リソース

- [GitHub Copilot のカスタマイズに使う命令ファイル][instruction-files]
- [カスタム命令のベスト プラクティス][instructions-best-practices]
- [Copilot 向けのよりよいカスタム命令を書くための 5 つのヒント][copilot-instructions-five-tips]
- [GitHub Copilot の個人用カスタム命令][personal-instructions]
- [Awesome Copilot — 命令ファイルなどのリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
