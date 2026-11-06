---
title: "レッスン 7 - QA エージェントを作成して使用する"
description: "Issue の要件、quality-checks スキル、Playwright MCP を組み合わせた QA カスタム エージェントを作成します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

`quality-checks` スキルを使って自動チェックを実行し、Playwright MCP を使ってブラウザーで絞り込み機能を確認しました。次は、QA プロセスを明確に定義したカスタム エージェントに、これらの機能をまとめます。

このレッスンでは、次の操作を行います。

- カスタム エージェントが命令、スキル、MCP ツールと連携する方法を確認します。
- 再利用可能な品質保証プロファイルを作成して確認します。
- QA エージェントを選択し、絞り込み機能の Issue に照らしてその結果をレビューします。

## シナリオ

Tailspin Toys では、プル リクエスト (PR) を作成する前に、要件、コード品質、自動チェック、テスト範囲、ブラウザーでの動作を一貫して確認したいと考えています。カスタム エージェントを使うと、QA プロセスをまとめて実行し、再利用可能なレポートを作成できます。

## カスタム エージェントとは

カスタム エージェントは、Markdown プロファイルで定義する Copilot の専門エージェントです。プロファイルにはエージェントの目的、命令、使用可能なツールを記述します。このワークショップでは、`.github/agents/qa.agent.md` に QA の役割を定義し、Copilot CLI で選択します。

これまでに使用したカスタマイズには、それぞれ異なる役割があります。リポジトリの命令はチームの標準を記述します。`quality-checks` スキルは繰り返し実行できるチェックをまとめます。Playwright MCP はブラウザー ツールを提供します。QA プロファイルは、要件を評価して結果を報告するために、Copilot がこれらの機能をどのように使うかを示します。QA プロファイルは、これらを置き換えるものではなく、別の会話も必要ありません。

## QA プロファイルを作成する

機能の PR を開く前に、再利用可能な QA プロファイルを作成するよう Copilot に依頼します。プロファイルには、QA が実施するチェックと、その作業範囲を定義します。

1. Codespace に戻り、絞り込み機能の会話が Interactive モードであることを確認します。
2. 既定のエージェントに、新しいカスタム エージェントを作成するよう依頼します。

   ```plaintext
   Create a custom agent named QA in .github/agents/qa.agent.md. It should check features against their issues and agreed requirements, follow the repository instructions, run the quality-checks skill, use Playwright MCP to verify behavior, and add tests when coverage is missing.

   Have it report each requirement as pass, fail, or blocked with supporting evidence. It must ask before changing implementation code, and it must not commit changes or open pull requests.

   Just create the profile for now so I can review it.
   ```

## プロファイルを確認する

新しいエージェントを使用する前に、そのプロファイルをレビューし、Copilot が意図した QA ワークフローと作業範囲を認識していることを確認します。検証だけを求めたときに、範囲の不明確な変更を行うことを防げます。

1. `/diff` を入力し、`.github/agents/qa.agent.md` を開きます。
2. フロントマターを確認します。`description` は必須です。`name` は省略できますが、設定するとエージェントの表示名が分かりやすくなります。
3. プロファイルの命令を読み、QA が要件を確認し、リポジトリの命令に従い、`quality-checks` スキルを実行して、Playwright MCP を使用することを確認します。
4. QA が根拠を示して結果を報告し、実装コードを変更する前に確認を求め、変更をコミットしたりプル リクエストを開いたりしないことを確認します。
5. これらの役割や制約が不足している場合は、続ける前に既定のエージェントにプロファイルの修正を依頼します。

## Issue に対して QA を実行する

Copilot CLI は会話の開始時にプロジェクトのエージェントを読み込みます。プロファイルを作成したら同じ絞り込み機能の会話を再開し、QA を選択して、Issue と計画の内容を使えるようにします。

1. 次のプロンプトを使ってエージェントを有効にします。

   ```plaintext
   /agent QA
   ```

> [!NOTE]
> 作成したばかりのエージェントは、使用可能なエージェントの一覧にまだ表示されない場合があります。エージェントは存在するため、上記のコマンドで有効にできます。

2. 次のプロンプトで機能のレビューを依頼します。

   ```plaintext
   Review the filtering feature against the issue and the decisions in our plan. Is it ready for a PR?
   ```

3. QA エージェントが作業を開始します。
4. 完了したら、レポートを確認します。

## まとめと次のステップ

再利用可能な専門エージェントをワークフローに加え、その結果を確認しました。このレッスンでは、次の操作を行いました。

- カスタム エージェントが命令、スキル、MCP ツールと連携する方法を確認しました。
- 再利用可能な品質保証プロファイルを作成して確認しました。
- QA エージェントを選択し、絞り込み機能の Issue に照らしてその結果をレビューしました。

これで、実装、スキルの更新、QA プロファイル、テスト、検証レポートが揃いました。次は機能 PR を[まとめて作成し、Agent Merge を使用します][next-lesson]。

## リソース

- [Copilot CLI のカスタム エージェントを作成する][create-agents]
- [カスタム エージェントの構成][agent-config]

[previous-lesson]: ../6-mcp-playwright/
[next-lesson]: ../8-create-pull-request/
[create-agents]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli
[agent-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
