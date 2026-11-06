---
title: "レッスン 5 - quality-checks スキルのカスタマイズと使用"
description: "既存の quality-checks スキルを確認し、レポート形式をカスタマイズして、絞り込み機能の検証に使用します。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

コードを書く作業には、単にコードを書く以上のことが含まれます。手動でコードの動作を確認し、命令ファイルを使って標準に従うことも確認しました。テスト、lint、継続的インテグレーション (CI) のほかの作業はどうでしょうか。

このようなタスクには、**エージェント スキル**が適しています。スキルを使うと、こうした処理を適切に実行する方法を Copilot に伝えられます。

このレッスンでは、次の操作を行います。

- 既存の `quality-checks` スキルを確認します。
- 結果の報告形式をカスタマイズします。
- スキルを再読み込みして実行します。

## シナリオ

Tailspin Toys は、単体テスト、lint、型チェックに `quality-checks` スキルを使用します。チームでは、結果を読みやすくするためにレポートを改善したいと考えています。

## 命令、スクリプト、リソース

エージェント スキルは、エージェントが必要に応じて読み込む、再利用可能なタスクの命令、実行可能なスクリプト、補助リソースをまとめたものです。基本的には、スキル名のフォルダーと、その中にある `SKILL.md` という名前の Markdown ファイルで構成されます。Markdown には、スキル名と説明を定義するフロントマター、スキルの概要、呼び出すタイミングのガイダンスを記載します。フォルダーには、呼び出されたときに使用するスクリプトやその他のリソースを含めることもできます。

> [!NOTE]
> スキルに追加のフォルダーやファイルは必須ではありません。Tailspin Toys の `quality-checks` スキルは、プロジェクトにあるコマンドを使用するため、`SKILL.md` だけで構成されています。

スキルをプロジェクトの `.github/skills` フォルダーに置くと、チームで共有して再利用できるリポジトリのアセットになります。また、通常は `~/.copilot/skills` にある、ユーザーのスキル フォルダーに置くこともできます。

## スキルを確認する

Tailspin Toys のチームが単体テスト、lint、型チェックの実行用に作成した `quality-checks` スキルを確認しましょう。

1. Codespace に戻ります。Codespaces エディターで `.github/skills/quality-checks/SKILL.md` を開きます。
2. 先頭にある `name` と `description` を読みます。説明は Copilot がスキルを呼び出すタイミングを判断するのに役立ちます。
3. 命令を読み、Copilot にテストと lint の手順をどのように案内しているかを確認します。
4. スキルには **Results output formatting** セクションがまだないことを確認します。

## 変更前にスキルを実行する

スキルは Copilot CLI から直接呼び出すか、自然言語を使って呼び出せます。Copilot に 3 種類のチェックを実行してもらいましょう。

1. 絞り込み機能の会話に戻り、Interactive モードであることを確認します。
2. 次のプロンプトを使います。

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. 最後に出力されるレポートを確認します。

## レポートをカスタマイズする

実行した内容、成功したかどうか、ツールが実際に報告した内容を伝える、よりよいレポートが必要です。スキルを更新して、このレポートを作成できるようにしましょう。

1. `.github/skills/quality-checks/SKILL.md` に戻ります。
2. まだ開いていない場合は、ファイルを開きます。
3. ファイルの末尾に次のセクションを追加します。

   ```markdown
   ## Results output formatting

   Upon completion, report each command that ran and whether it passed, failed, or was blocked. Include test counts, durations, errors, warnings, and other metrics only when the tool reports them. Identify the next action for any failure or blocker, and never describe a skipped or incomplete check as passed.
   ```

4. ファイルは自動的に保存されます。

## 更新したスキルを実行する

変更したスキルを使ってみましょう。Copilot CLI は、会話を再起動せずに編集済みのスキルを再読み込みできます。

1. 次のコマンドを入力します。

   ```plaintext
   /skills reload
   ```

2. 前回とまったく同じプロンプトを使います。

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. 最後に出力されるレポートを確認し、最初のレポートと比較します。

## まとめと次のステップ

既存のエージェント スキルをカスタマイズして使用しました。このレッスンでは、次の操作を行いました。

- 単体テスト、lint、型チェックを実行する `quality-checks` スキルを確認しました。
- 結果の報告形式をカスタマイズしました。
- スキルを再読み込みして実行しました。

この変更は、絞り込み機能の PR に含めます。次は[Playwright MCP サーバーを通じて][next-lesson]、Copilot がサイトを直接操作できるようにします。

## ほかのスキルの例

コミュニティによる次の例は参考資料であり、追加のタスクではありません。

- [Agent Skills 仕様][skill-spec]
- [コントリビューションのワークフロー: `make-repo-contribution`][contribution-example]
- [要件文書: `prd`][prd-example]
- [図と、同梱のエクスポート スクリプト: `drawio`][drawio-example]
- [ブラウザー テスト: `webapp-testing`][browser-example]

上流のコントリビューション用スキルの名前は `make-repo-contribution` です。以前の Tailspin テンプレートでは、異なる名前の `make-contribution` が使用されていました。このワークショップでは、どちらのコントリビューション スキルも必要ありません。

[previous-lesson]: ../4-custom-instructions/
[next-lesson]: ../6-mcp-playwright/
[skill-spec]: https://agentskills.io/specification
[contribution-example]: https://github.com/github/awesome-copilot/tree/main/skills/make-repo-contribution
[prd-example]: https://github.com/github/awesome-copilot/tree/main/skills/prd
[drawio-example]: https://github.com/github/awesome-copilot/tree/main/skills/drawio
[browser-example]: https://github.com/github/awesome-copilot/tree/main/skills/webapp-testing
