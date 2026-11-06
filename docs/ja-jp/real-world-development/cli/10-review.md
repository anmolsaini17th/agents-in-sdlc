---
title: "レッスン 10 - 振り返りと次のステップ"
description: "Copilot CLI のワークフロー、2 つのプル リクエスト、再利用可能なカスタマイズ、追加リソースを確認します。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

GitHub Copilot CLI を使って、Tailspin Toys の一連のワークフローを進めました。次の操作を行いました。

- Codespace を準備し、Copilot CLI をインストールしてプロジェクトを確認し、用意されている絞り込み機能の Issue を見つけました。
- 星評価を追加し、転送されたブラウザーで結果を確認して、最初のプル リクエスト (PR) を手動でマージしました。
- 絞り込み機能の Issue から始め、Plan モードで方針を決め、Autopilot モードで実装し、Interactive モードでレビューしました。
- カスタム命令でエージェントをガイドし、既存の `quality-checks` スキルをカスタマイズして、単体テスト、lint、型チェックを実行しました。
- Playwright Model Context Protocol (MCP) サーバーを追加し、実際のブラウザーで絞り込み機能を確認しました。
- QA カスタム エージェントを作成して選択し、要件、テスト範囲、スキルの結果、ブラウザーでの証拠を評価しました。
- 絞り込み機能全体をレビューし、Agent Merge を承認して絞り込み機能の PR をマージしました。
- コンテキスト、モデル、共有、任意のクラウド委任に使えるスラッシュ コマンドを確認しました。

## 作成したもの

このワークショップでは、更新済みの `main` からブランチを作成し、次の 2 つの PR を作成しました。

1. **星評価**: ゲーム カードに既存の `starRating` と、評価がない場合の状態を明示して表示します。
2. **絞り込みと品質ワークフロー**: 絞り込み機能を実装し、命令を更新してその命令を機能に適用し、`quality-checks` のレポートをカスタマイズして、QA プロファイルと関連テストを追加します。

絞り込み機能の計画から PR の作成までは、同じ会話とブランチを使いました。ワークショップを簡略化するため、これらの変更を 1 つの PR にまとめました。

## さまざまな検証方法

自動テスト、自分で行うブラウザー確認、MCP を通じた Copilot のブラウザー確認など、複数の方法でコードを検証しました。`quality-checks` スキルは単体テスト、lint、型チェックを実行し、新しい形式で結果を報告しました。QA はこれらの結果に要件とテスト範囲のレビューを組み合わせました。

追加したテストで実際の不足を解消できる必要があります。QA の実行時に新しいテストが不要と判断される場合もあります。マージを承認する前に、コードと検証の証拠を確認してください。変更を加えたら、影響を受ける検証の結果を更新します。

## ベスト プラクティス

Copilot に与えるコンテキストとツールは、作業結果に影響します。このワークショップでは、命令を更新し、スキルをカスタマイズし、QA プロファイルを作成し、MCP サーバーを構成しました。これらのカスタマイズを会話間で再利用し、チームのニーズに合わせて調整できます。命令は標準を設定し、スキルは繰り返し実行するタスクを説明し、カスタム エージェントは専門的な役割を定義し、MCP サーバーは外部ツールに接続します。エージェントの要約だけでなく、実際の変更とツールの結果を確認してください。

タスクに合わせて**モードとモデル**を選びます。**Plan** は実装前に方針を決めるとき、**Interactive** は変更を細かく確認しながら進めるとき、**Autopilot** は範囲の明確なタスクを任せるときに使用します。定型的な編集には高速なモデルを、複雑な作業にはより高性能なモデルを選択できます。

インフラストラクチャと同じように、コンテキストも重要です。何を、なぜ、どのように作りたいかを明確に説明すると、結果が大きく変わります。

## さらに確認する

主要なワークフローは完了しました。Copilot CLI には、ほかにも次のような機能があります。

- `/review` でコード レビュー エージェントに変更を分析してもらいます。
- `/rubber-duck` で問題を整理し、別の視点を得ます。
- `/fleet` で独立したサブタスクを並列に実行します。
- `/worktree` で別のタスクを分離します。
- `/delegate` で Copilot クラウド エージェントにタスクを送信します。

## 次のステップ

ツールの使い方を上達させるには、継続して使うことが大切です。本番コード、趣味のコード、何年も作りたいと思っていた小さなアプリなどで試してみましょう。チームと学んだ内容を共有し、互いの経験から学んでください。そして、ドキュメントも引き続き確認しましょう。

ターミナルから Tailspin Toys を拡張する場合は、オプションの[Foundry Backer Concierge シリーズ][foundry]に進んでください。別の環境での作業については、[VS Code ワークショップ][vscode]、[GitHub Copilot app ワークショップ][app]、または[Copilot クラウド エージェント ワークショップ][cloud]を確認してください。

## リソース

- [GitHub Copilot CLI について][about-cli]
- [Copilot CLI コマンド リファレンス][cli-reference]
- [Copilot CLI のカスタマイズ][customize-cli]
- [Copilot CLI でプル リクエストを管理する][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
