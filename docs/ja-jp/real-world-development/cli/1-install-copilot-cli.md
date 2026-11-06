---
title: "レッスン 1 - GitHub Copilot CLI のインストール"
description: "Codespace に Copilot CLI をインストールして認証し、使い方を確認して、用意された絞り込み機能の Issue を見つけます。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

[GitHub Copilot CLI][about-copilot-cli] は、ターミナルで動作するエージェント型コーディング アシスタントです。コマンド ラインからコードベースを探索し、コードを生成し、コマンドを実行して、外部ツールを使用できます。タスクを任せたり、変更を依頼したりできるため、作業に集中できます。最初のステップとしてツールをインストールしましょう。使い慣れたツールを使って簡単にインストールできます。

このレッスンでは、次の操作を行います。

- npm を使って GitHub Copilot CLI をインストールします。
- GitHub アカウントで認証します。
- ワークショップのリポジトリを信頼し、簡単な会話を試します。
- 組み込みの GitHub MCP サーバーを使って絞り込み機能の Issue を探します。

## シナリオ

チームは、増え続けるバックログに取り組むため、AI エージェントを使い始めています。Copilot CLI は、多くの開発者が日常的に作業するターミナルにその機能を提供します。このレッスンでインストールと認証を完了し、ワークショップの残りで利用できるようにします。

## Copilot CLI をインストールする

Copilot CLI は [npm][install-cli]、WinGet、Homebrew でインストールできます。GitHub Codespaces には Node.js があらかじめインストールされているため、ここでは npm を使います。

1. Codespace に戻り、ターミナルを開きます。
2. Node.js がインストールされ、必要なバージョンを満たしていることを確認します。

   ```bash
   node --version
   ```

   バージョン 24 以上が表示されます。

3. Copilot CLI をグローバルにインストールします。

   ```bash
   npm install -g @github/copilot
   ```

4. インストールを確認します。

   ```bash
   copilot --version
   ```

   バージョン番号が表示されます。

## GitHub で認証する

Copilot CLI を初めて起動すると、GitHub アカウントで認証するように求められます。

1. Copilot CLI を起動します。

   ```bash
   copilot
   ```

2. 求められた場合は、デバイス コードの手順に従って認証し、Copilot CLI を承認します。
3. Copilot CLI に次のプロンプトが表示されます。

   ```plaintext
   Copilot can read files in this folder and, with your permission, edit them or run code and shell commands. It will remember your permissions for the rest of this session.

   Do you trust the files in this folder?
   ```

4. 表示されたパスが Tailspin Toys リポジトリであることを確認します。**Yes, and remember this folder for future sessions** を選択して、信頼することを確定します。

> [!NOTE]
> Codespace では、GitHub セッションを通じてすでに認証されている場合があります。認証を求められずに Copilot CLI が起動した場合は、そのまま続行できます。

## 使い方を確認する

通常のシェル プロンプトに入力したコマンドは、Codespace で直接実行されます。Copilot CLI の起動後は、自然言語をエージェントに送信し、スラッシュ コマンドで会話を操作します。

1. `/model` を入力します。矢印キーで **Auto** を選択し、<kbd>Enter</kbd> を押して、もう一度 <kbd>Enter</kbd> を押して確定します。
2. `/help` を入力して、インストールされているバージョンで使用できるコマンドを確認し、<kbd>Esc</kbd> を押してヘルプ画面を閉じます。
3. Copilot に簡単な質問をして、正しく動作していることを確認します。

   ```plaintext
   What are the key files in this project?
   ```

4. 回答を読み、Copilot が回答前にリポジトリを調べる様子を確認します。
5. `/mcp list` を入力し、組み込みの GitHub MCP サーバーが利用可能であることを確認します。
6. Copilot に絞り込み機能の Issue を探してもらいます。

   ```plaintext
   Using GitHub MCP, find the issue in this repository titled "Allow users to filter games by category and publisher." Give me its URL and a short summary. Don't change anything.
   ```

7. URL を開いて Issue を読みます。最初の小さな変更を完了した後に、この Issue を使います。

> [!TIP]
> 通常の Copilot CLI セッションは、ターミナルで現在チェックアウトしているブランチを使用し、自動的に worktree を作成しません。変更を始める前に機能ブランチを作成します。

## ワークショップ用のショートカットを使う

Copilot CLI は通常、許可されている範囲外のツールを使う前に確認を求めます。このワークショップでは `--yolo` を使って再起動します。Codespace 内で承認プロンプトを省略し、演習に集中するためのショートカットです。

> [!CAUTION]
> `--yolo` は、すべてのツール、パス、URL を許可する完全な自動権限 (`--allow-all-tools`、`--allow-all-paths`、`--allow-all-urls`) を有効にします。Codespace や仮想マシン (VM) などの分離された環境でのみ使い、日常の開発で既定の設定やエイリアスとして使用しないでください。詳細については[ツールの使用を許可または禁止する方法][allow-all-warning]を参照してください。

また、`--enable-all-github-mcp-tools` を指定すると、後のレッスンで Issue やプル リクエストを操作するときに使う、読み取りと書き込みの GitHub MCP ツールが有効になります。Codespace によってローカル コンピューターへのアクセスは制限されますが、認証された GitHub リソースには引き続きアクセスできます。公開やマージの前に変更をレビューしてください。

1. `/exit` で Copilot CLI を終了します。
2. リポジトリのルートから再起動します。

   ```bash
   copilot --yolo --enable-all-github-mcp-tools
   ```

3. Copilot にプロジェクトについて別の簡単な質問をして、会話が動作することを確認し、`/exit` で終了します。

Copilot は会話を自動的に保存します。後で命令を変更したりエージェントを追加したりしたら、`copilot --resume` を実行して同じ機能の会話とブランチを再開します。

## まとめと次のステップ

おめでとうございます。このレッスンでは、次の操作を行いました。

- npm を使って GitHub Copilot CLI をインストールしました。
- GitHub アカウントで認証しました。
- ワークショップのリポジトリを信頼して、簡単な会話を試しました。
- 組み込みの GitHub MCP サーバーで絞り込み機能の Issue を見つけました。

次は[最初の変更に取り組み][next-lesson]、Copilot CLI を使ってゲーム カードに星評価を表示します。

## リソース

- [GitHub Copilot CLI のインストール][install-cli]
- [GitHub Copilot CLI について][about-copilot-cli]
- [Copilot CLI コマンド リファレンス][cli-reference]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-add-star-rating/
[install-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/set-up-copilot-cli/install-copilot-cli
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[allow-all-warning]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli#allowing-and-denying-tool-use
