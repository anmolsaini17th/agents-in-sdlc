---
title: "演習 0: 前提条件"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

VS Code の演習を始める前に、必要な準備を整えます。Tailspin Toys リポジトリの自分用コピーを作成し、作業用の [Codespace][codespaces] を起動して、エディターで GitHub Copilot Chat が使えることを確認します。

## ラボ用リポジトリを設定する

作成するコード用に、[テンプレート リポジトリ][template-repository]から Tailspin Toys の自分用コピーを作成します。新しいリポジトリには、ラボに必要なファイルがすべて含まれています。演習中はこのリポジトリを使用します。

1. 新しいブラウザー ウィンドウで、[Tailspin Toys テンプレート][tailspin-template]を開きます。
2. ラボ用リポジトリのページで **Use this template** ボタンを選択し、次に **Create a new repository** を選択します。

    ![Use this template ボタン](../../../_images/ex0-use-template.png)

3. GitHub または Microsoft が主催するイベントの一環としてワークショップに参加している場合は、メンターの指示に従います。それ以外の場合は、GitHub Copilot を利用できる Organization に新しいリポジトリを作成します。

    ![リポジトリ テンプレートの設定を入力する画面](../../../_images/ex0-repository-settings.png)

4. ラボの後半で使用するため、作成したリポジトリのパス (**organization-or-user-name/repository-name**) を控えておきます。

> [!NOTE]
> **バックログの準備は完了しています**
>
> テンプレートからリポジトリを作成すると、GitHub Issue のバックログが自動的に作成されます。ワークショップを通じてこれらの Issue に取り組むため、自分で起票する必要はありません。

## Codespace を作成する

次は、Codespace を使ってラボの演習に取り組みます。

[GitHub Codespaces][codespaces] は、ブラウザーから直接コードを作成、実行、デバッグできるクラウドベースの開発環境です。複数のプログラミング言語、拡張機能、ツールに対応した高機能な IDE を利用できます。

1. 新しく作成したリポジトリに移動します。
2. 緑色の **Code** ボタンを選択します。

    ![Code ボタンを選択する](../../../_images/ex0-code-button.png)

3. **Codespaces** タブを選択し、**+** ボタンを選択して Codespace を作成します。

    ![新しい Codespace を作成する](../../../_images/ex0-create-codespace.png)

Codespace の作成には数分かかりますが、すべてのサービスを手動でインストールするよりもはるかに短時間で完了します。その間に、次に紹介する GitHub Copilot の機能を確認してもよいでしょう。

> [!CAUTION]
> 後の演習で Codespace に戻ります。今はブラウザーのタブで開いたままにしてください。

> [!NOTE]
> このワークショップは、Codespace またはローカルの [dev container][dev-containers] 内で実行します。どちらの環境にも、スムーズに進めるために必要な前提条件がインストールされています。ローカルで実行する場合は、クローンしたリポジトリを VS Code で開き、求められたら **Reopen in Container** を選択します。Codespace と同じ dev container が構築されます。

[codespaces]: https://github.com/features/codespaces
[dev-containers]: https://code.visualstudio.com/docs/devcontainers/containers

## GitHub Copilot Chat とエージェント モードを使う

GitHub Copilot Chat のエージェント モードを使うには、IDE に GitHub Copilot Chat 拡張機能をインストールする必要があります。GitHub Codespace を使っている場合は、すでにインストールされています。

> [!TIP]
> GitHub Copilot Chat 拡張機能がインストールされていない場合は、[Visual Studio Code Marketplace からインストール][copilot-chat-extension]できます。または、Visual Studio Code の Extensions ビューを開き、**GitHub Copilot Chat** を検索して **Install** を選択します。

拡張機能をインストールしたら、GitHub アカウントで認証して有効にする必要があります。

1. Codespace に戻ります。
2. エディターの右側に Copilot Chat が表示されていない場合は、Codespace の上部にある **Copilot Chat** アイコンを選択します。
3. Copilot Chat ウィンドウに「Hello world」のようなメッセージを入力して Enter キーを押します。Copilot Chat が起動します。
4. 認証されていない場合は、GitHub アカウントへのサインインを求められます。指示に従って認証します。

    ![Copilot Chat の認証プロンプトの例](../../../_images/ex1-copilot-authentication.png)

5. 認証後、Copilot Chat ウィンドウが表示されます。

## まとめ

おめでとうございます。ラボ用リポジトリのコピーを作成し、コードを書き始めるときに使う Codespace の作成を始めました。

## 次のステップ

Copilot を使い始めましょう。[演習 1 - カスタム命令][next-lesson]に進み、Copilot にプロジェクトの規則を伝えます。

## リソース

- [GitHub Codespaces の概要][codespaces]
- [テンプレートからリポジトリを作成する][template-repository]
- [Codespaces の使用を開始する][codespaces-quickstart]

[tailspin-template]: https://github.com/github-samples/tailspin-toys
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[codespaces-quickstart]: https://docs.github.com/codespaces/getting-started/quickstart
[copilot-chat-extension]: https://marketplace.visualstudio.com/items?itemName=GitHub.copilot-chat
[next-lesson]: ../1-custom-instructions/
