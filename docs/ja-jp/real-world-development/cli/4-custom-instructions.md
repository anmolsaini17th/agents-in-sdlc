---
title: "レッスン 4 - カスタム命令で Copilot をガイドする"
description: "リポジトリの命令を確認し、ドキュメントの標準を追加して、絞り込み機能のコードに適用します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

生成 AI を利用する際は、コンテキストが重要です。特定の方法でタスクを実行したい場合、そのガイダンスを Copilot が参照できるようにします。[命令ファイル][instruction-files]では、必要なコードだけでなく、その構成方法も指定できます。絞り込み機能を実装したので、Copilot が参照した命令を確認し、ドキュメント作成の標準を追加してコードに適用します。

このレッスンでは、次の操作を行います。

- リポジトリの命令とパスを指定した命令ファイルがエージェントに適用される方法を確認します。
- コーディング標準が守られるように命令ファイルを更新します。
- 命令ファイルがコードに与える影響を確認します。

## シナリオ

Tailspin Toys には、開発プラクティスに関するガイドラインと要件があります。たとえば、次のような内容です。

- コメントでは、コードをそのまま言い換えるのではなく、意図や自明ではない判断を説明します。
- `db/` と `src/lib/` のエクスポートされた関数では、目的、パラメーター、戻り値を TSDoc/JSDoc で説明します。`db` 引数を注入できる場合は、その引数についても記述します。
- 再利用可能な Astro コンポーネントでは `Props` コントラクトを説明し、関連するコードを変更するときはコメントも最新の状態にします。
- 既存の書式設定と lint のガイダンスを維持します。

命令ファイルを使用して、Copilot がこれらのプラクティスに沿ってタスクを実行できるようにします。

## 命令ファイル

カスタム命令を使うと、コーディング スタイルや要件を Copilot に伝え、適切な提案やコード スニペットを生成できるようにコンテキストと設定を提供できます。好みのコーディング規則、ライブラリ、コメントの書き方などを指定できます。リポジトリ全体、または特定の種類のファイルに対する命令を作成できます。

命令ファイルには次の 2 種類があります。

- `.github/copilot-instructions.md` は、リポジトリへのすべてのリクエストで Copilot に送信される単一の命令ファイルです。Copilot に送るチャットや CLI の多くのリクエストに関係する、プロジェクト レベルの情報を記載します。
- `.github/instructions/*.instructions.md` は、言語、ファイルの種類、タスクに応じたガイドラインを記載します。

> [!NOTE]
> その他の命令形式とサポート状況は、環境によって異なります。特定の形式を使用する前に、[カスタム命令のサポート リファレンス][custom-instructions-support]を確認してください。

## このプロジェクトのカスタム命令ファイルを確認する

スターター プロジェクトには、すでに命令ファイルがいくつか含まれています。変更を加える前に、既存のファイルを確認して、変更の影響を比較できるようにします。

1. Codespace に戻ります。
2. Codespaces のエディターで (ターミナルではなく)、`.github/copilot-instructions.md` を開きます。
3. プロジェクトの概要とコーディング ガイダンスを確認します。この命令は、このリポジトリでの Copilot とのすべてのやり取りに適用されます。
4. `.github/instructions` フォルダーを開き、ファイルを確認します。Astro ファイル、Drizzle データ レイヤー、テストなどの命令があります。
5. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` フィールドは、命令を適用するファイルを指定する glob です。
6. `.github/instructions/drizzle.instructions.md` を開き、ほかの命令ファイルや既存のプロジェクト ファイルへのリンクを確認します。これらのリンクを利用すると、大きな命令セットを再利用可能な小さなファイルに分け、Copilot が参考にできる例を示せます。

## チームのガイダンスに合わせて命令ファイルを更新する

既存のファイルは有用ですが、まだ追加できることがあります。`copilot-instructions.md` を変更し、新しく生成する TypeScript に TSDoc コメントを追加するようにします。

1. `.github/copilot-instructions.md` の **Code formatting guidance** セクション (35 行目付近) を見つけます。
2. 最後の箇条書きとして、次の行を追加します。

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

ファイルは自動的に保存されます。

## 更新したガイダンスを適用する

Copilot CLI は会話の開始時にリポジトリの命令を読み込みます。フィルター機能のコンテキストを保ったまま、新しいガイダンスが読み込まれるように、会話を終了して再開します。

1. 絞り込み機能の会話が開いている場合は、`/exit` を入力して終了します。Copilot CLI は会話を自動的に保存します。
2. リポジトリのルートにある `game-filters-cli` ブランチから、保存した会話を再開します。

   ```bash
   copilot --resume
   ```

   求められた場合は、絞り込み機能の会話を選択し、`game-filters-cli` ブランチがチェックアウトされていることを確認します。

3. 更新したガイダンスを適用するよう Copilot に依頼します。

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. `/diff` を入力し、変更された TypeScript ファイルを確認します。追加された TSDoc コメントがコードを正確に説明していることを確認します。

## まとめと次のステップ

Copilot CLI が命令ファイルを読み込む方法を確認し、新しい標準を機能に適用しました。具体的には、次の操作を行いました。

- リポジトリの命令とパスを指定した命令ファイルがエージェントに適用される方法を確認しました。
- コーディング標準が守られるように命令ファイルを更新しました。
- 命令ファイルがコードに与える影響を確認しました。

次は再利用可能な `quality-checks` スキルを[カスタマイズして実行し][next-lesson]、lint とテストを一貫して実施します。

## リソース

- [Copilot CLI のカスタム命令を追加する][instruction-files]
- [カスタム命令のサポート][custom-instructions-support]
- [カスタム命令のベスト プラクティス][instructions-best-practices]
- [Awesome Copilot — 命令ファイルなどのコレクション][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
