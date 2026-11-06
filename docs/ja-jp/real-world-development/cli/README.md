---
slug: ja-jp/real-world-development/cli
title: "GitHub Copilot CLI"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

**[GitHub Copilot CLI](https://docs.github.com/copilot/concepts/agents/about-copilot-cli)** は、ターミナルで GitHub Copilot をエージェント型のコーディング アシスタントとして利用できるようにします。コードベースを探索し、コードを生成し、コマンドを実行し、外部ツールに接続できます。すべてコマンド ラインから行えるため、グラフィカル エディターに切り替えずに作業の流れを保てます。

このワークショップでは、Tailspin Toys の一連のワークフローに沿って進めます。

1. GitHub Codespaces でプロジェクトを準備し、Copilot CLI をインストールして使い方を確認します。
2. 星評価の変更を行い、ブラウザーでレビューして、最初のプル リクエスト (PR) を手動でマージします。
3. 絞り込み機能の Issue から始め、プラン モードで方針を決め、オートパイロット モードで実装し、インタラクティブ モードでレビューします。
4. リポジトリの命令を更新し、絞り込み機能の作業に適用します。
5. 既存の `quality-checks` スキルをカスタマイズし、プロジェクトのチェックを実行します。
6. Playwright Model Context Protocol (MCP) サーバーを追加し、ブラウザーで絞り込み機能を確認します。
7. QA カスタム エージェントを作成し、要件、テスト範囲、検証の証拠を確認します。
8. 絞り込み機能の変更全体をレビューし、Agent Merge を使って PR をマージします。
9. コンテキスト、モデル、共有、任意のクラウド委任に役立つスラッシュ コマンドを確認します。

このワークショップでは、PR を 2 つ作成します。星評価の PR に続いて、命令の更新、スキルの更新、QA プロファイル、テストを含む絞り込み機能の PR を作成します。絞り込み機能と品質チェックの作業では同じ会話とブランチを使うため、各ツールを学びながら作業を積み重ねられます。

## レッスン

| レッスン | トピック | 説明 |
| ------ | ----------- | ----------- |
| [0. 前提条件][ex0] | セットアップ | リポジトリと Codespace を作成します |
| [1. Copilot CLI のインストール][ex1] | インストール | Copilot CLI をインストールして認証し、使い方を確認します |
| [2. 星評価の追加: すぐにできる改善][ex2] | 最初の変更 | 既存の評価と `null` の場合の代替表示を行い、最初の PR をマージします |
| [3. エージェント モード: プランとオートパイロット][ex3] | エージェント モード | Issue から機能を計画し、オートパイロット モードで実装して、インタラクティブ モードでレビューします |
| [4. カスタム命令で Copilot をガイドする][ex4] | コンテキスト | 命令を確認して更新し、絞り込み機能の作業に適用します |
| [5. quality-checks スキルのカスタマイズと使用][ex5] | 繰り返し可能なチェック | 既存スキルを確認し、レポート形式を変更して実行します |
| [6. Playwright MCP で機能を検証する][ex6] | ブラウザーでの確認 | CLI で MCP を構成し、絞り込みの動作を確認します |
| [7. QA エージェントを作成して使用する][ex7] | 要件とテスト範囲 | 専門プロファイルを作成して選択し、最終的な検証の証拠を集めます |
| [8. 機能 PR の作成とマージ][ex8] | レビューとマージ | 変更全体をレビューして PR を作成し、Agent Merge を使用します |
| [9. GitHub Copilot CLI のスラッシュ コマンド][ex9] | CLI の機能 | コンテキスト、モデル、共有、クラウド エージェントへの任意の委任を確認します |
| [10. 振り返りと次のステップ][ex10] | まとめ | ワークフロー、再利用可能なカスタマイズ、追加リソースを確認します |
| [オプション: Foundry を組み込む][foundry] | ホスト型エージェント | モデルを準備し、カタログの情報に基づくエージェントをデプロイして、Web サイトに接続します |

## 前提条件

このワークショップに参加する前に、次を準備してください。

- [ ] **Copilot Student、Pro、Pro+、Business、Enterprise** のいずれかの有効なプランがある GitHub アカウント
- [ ] リポジトリと Codespace を作成する権限
- [ ] ターミナルまたはコマンド ライン操作の基本的な知識

> [!TIP]
> 有料プランをお持ちでない場合、認証済みの学生は [GitHub Education][student-plan] を通じて GitHub Copilot を無料で利用できます。**Copilot Student** プランには、このワークショップで使用するエージェント、MCP、コードレビュー、Copilot CLI の機能が含まれています。

[student-plan]: https://github.com/education/students

> [!NOTE]
> Copilot Business または Copilot Enterprise を使用している場合は、管理者が Copilot CLI の使用を有効にしていることを確認してください。

## はじめる

**[前提条件から始める →][ex0]**

[ex0]: 0-prerequisites/
[ex1]: 1-install-copilot-cli/
[ex2]: 2-add-star-rating/
[ex3]: 3-agent-modes/
[ex4]: 4-custom-instructions/
[ex5]: 5-agent-skills/
[ex6]: 6-mcp-playwright/
[ex7]: 7-qa-agent/
[ex8]: 8-create-pull-request/
[ex9]: 9-cli-power-tools/
[ex10]: 10-review/
[foundry]: 8-foundry-agent/
