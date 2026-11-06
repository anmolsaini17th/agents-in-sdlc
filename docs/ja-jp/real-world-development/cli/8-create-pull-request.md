---
title: "レッスン 8 - 機能 PR の作成とマージ"
description: "絞り込み機能、命令、スキル、QA プロファイル、テストを一緒に確認して PR を作成し、Agent Merge を使用します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

絞り込み機能の実装、命令の更新、スキルの更新、品質保証 (QA) プロファイル、テストが、1 つのブランチに保存されています。すべてをまとめてレビューし、プル リクエストを作成します。星評価のプル リクエスト (PR) は自分でマージしましたが、今回は **Agent Merge** にプロセスを任せます。

> [!NOTE]
> 通常は機能、命令の更新、スキルの更新、QA エージェントを複数の PR に分けます。このワークショップでは流れを簡略化するため、絞り込み機能と品質チェックの作業を同じ会話とブランチで進め、1 つの PR にまとめました。

このレッスンでは、次の操作を行います。

- Agent Merge と、マージ ライフサイクルを自動化する方法を学びます。
- 機能全体の変更と検証の証拠を確認します。
- 絞り込み機能の PR を作成します。
- レビュー後にのみ Agent Merge を有効にし、PR がマージされたことを確認します。

## シナリオ

絞り込み機能の開発で、Copilot を使って計画、実装、検証を行いました。Tailspin Toys では、開発者がマージを許可するタイミングを管理しながら、PR の残りの作業を自動化したいと考えています。

## Agent Merge について

**Agent Merge** は、プル リクエストをマージするために必要な残りの作業を自動化します。有効にすると、Copilot は PR をブロックしている問題 (失敗した継続的インテグレーション (CI) チェック、レビュー コメント、必要なリベースなど) に対処し、リポジトリで許可されていれば GitHub の自動マージを有効にします。

これまでは、自分で **Merge pull request** を選択していました。Agent Merge にその作業を任せることもできます。Agent Merge を有効にする前に、作業をレビューし、準備ができていることを確認してください。

## Agent Merge で PR を管理する

コードの作成が終わったら、全体をレビューして PR を作成し、その後の作業を Agent Merge に任せましょう。

1. Codespace に戻ります。
2. `/agent` を入力してエージェント ダイアログを開きます。
3. 一覧から **Default** を選択し、<kbd>Enter</kbd> を押します。
4. `/pr create` コマンドで新しい PR を作成します。
5. `/pr agentmerge` を使って Agent Merge を有効にします。
6. Copilot は PR の継続的インテグレーションを確認します。すべて成功すると、マージを実行します。
7. Copilot から「PR #14 was squash-merged successfully.」のようなメッセージが届いたことを確認します。

> [!IMPORTANT]
> Agent Merge は、必須の承認、ブランチ保護、マージ キュー、リポジトリ設定、必要な権限を回避しません。処理がブロックされた場合は報告された理由を確認し、リポジトリで許可されていれば、レビュー済みの変更を手動でマージします。

## まとめと次のステップ

コードの生成、テストと検証、プル リクエストの処理など、いくつかの開発プロセスを自動化しました。このレッスンでは、次の操作を行いました。

- Agent Merge と、マージ ライフサイクルを自動化する方法を学びました。
- 機能全体の変更と検証の証拠を確認しました。
- 絞り込み機能の PR を作成しました。
- レビュー後に Agent Merge を有効にし、PR がマージされたことを確認しました。

次は、コンテキスト、モデル、共有、任意のクラウド委任に役立つ Copilot CLI の[スラッシュ コマンドを確認します][next-lesson]。

## リソース

- [Copilot CLI でプル リクエストを管理する][manage-prs]
- [Copilot CLI コマンド リファレンス][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
