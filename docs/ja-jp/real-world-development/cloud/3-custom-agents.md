---
title: "演習 3 - カスタム エージェント"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## カスタム エージェントとは

GitHub Copilot の[カスタム エージェント][custom-agents-concept]を使うと、開発ワークフローにある特定のタスクや分野に合わせて、専用の AI アシスタントを作成できます。リポジトリの `.github/agents` フォルダーに Markdown ファイルを置き、対象タスクに役立つ命令、ベスト プラクティス、コーディング パターン、分野固有の知識を Copilot に提供できます。チームの知識を再利用可能なエージェントにまとめることもできます。たとえば、[WCAG][wcag] 準拠を確認するアクセシビリティ エージェント、安全なコーディング プラクティスに従うセキュリティ エージェント、一貫したテスト パターンを守るテスト エージェントがあります。

カスタム エージェントは、プロジェクトの `.github/agents` フォルダー、またはグローバルな `~/.copilot/agents` にある Markdown ファイルで定義します。各ファイルには、少なくとも `name` と `description` を含む YAML フロントマターと、エージェントの動作、専門知識、命令を定義する Markdown プロンプトを記述します。

### カスタム エージェントとエージェント スキルの違い

カスタム エージェントと[エージェント スキル][agent-skills-concept]には、役割が一部重なるところがあります。どちらも主に Markdown ファイルで定義し、AI が作業を行う方法を説明します。簡単に言うと、**カスタム エージェントは作業者、スキルはツール**です。

カスタム エージェントは独自のコンテキスト ウィンドウを持ち、作業の一部としてスキル (さらにほかのエージェント) を調整します。このラボでは、アクセシビリティ エージェントがサイトをアクセシビリティ ガイドラインに照らして確認し、更新します。その作業の一環として、プル リクエストのワークフローを支援するスキルや、テストを実行して管理するスキルを呼び出すこともできます。

> [!NOTE]
> カスタム エージェントの作成に唯一の正解はありません。AI に関するほかの作業と同様に、環境やシナリオに合わせてテストし、改善してください。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

カスタム エージェントを使って、次の操作を行います。

- カスタム エージェントの定義方法を確認します。
- タスクをカスタム エージェントに割り当てます。

## シナリオ

Tailspin Toys は、視覚的な能力や好みにかかわらず、すべてのユーザーがクラウドファンディング プラットフォームを利用できるようにしたいと考えています。最近のユーザー フィードバックでは、現在のダーク テーマについて、テキストと背景色のコントラストが不十分で読みにくいと感じるユーザーがいることが分かりました。このアクセシビリティの問題を解決するため、デザイン チームはユーザーが切り替えられるハイ コントラスト モードの実装を依頼しました。

アクセシビリティは重要です。機能をできるだけ早く実装するために、カスタム エージェントを使います。

## アクセシビリティ カスタム エージェントを確認する

アクセシビリティ用のカスタム エージェントは、すでに作成されています。Copilot をどのようにガイドするかを理解するため、内容を確認しましょう。

Codespace に戻り、アクセシビリティ カスタム エージェントのファイルを確認します。

1. `.github/agents/accessibility.md` を開きます。
2. `name` と `description` の YAML フロントマターを確認します。

> [!CAUTION]
> カスタム エージェントには、`name` と `description` のフロントマターが必要です。

3. 続くセクションを読み、次の内容を確認します。
   - アクセシブルな Web サイト用コードを生成するときの主な役割。
   - アクセシビリティのベスト プラクティス。
   - HTML、CSS、JavaScript のコード例。
   - よくある問題と避けるべきミス。

## Issue を作成して割り当てる

Mission control は、環境内のエージェントを操作するための中心的な場所です。Copilot cloud agent にタスクを割り当て、進捗を監視し、作業の方向を変更して追加の指示を与えられます。まず、ハイ コントラスト モードを作成するタスクを Copilot に割り当てます。

1. リポジトリに移動します。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい Issue を開きます。
4. **Blank issue** を選択して Issue を作成します。
5. **Title** に `Add high contrast mode to website` と入力します。
6. **Description** に次の内容を入力します。

   ```plaintext
   We need a high contrast mode for the site. There should be a toggle for high contrast which the user can set. It should store the setting in local storage on the browser.
   ```

7. **Create** を選択します。
8. 右側の **Assign to Copilot** を選択して、割り当てダイアログを開きます。
9. 一覧から **Accessibility agent** を選択します。

   ![カスタム エージェントを選択する画面で、cloud agent の割り当てと Accessibility を強調表示したスクリーンショット](../../../_images/ex5-select-custom-agent.png)

10. **Assign** を選択します。
11. Copilot はバックグラウンドで作業を開始します。

## まとめと次のステップ

[カスタム エージェント][custom-agents]を確認しました。カスタム エージェントは、特定のタスクや分野に合わせた GitHub Copilot の専門アシスタントです。チームの知識や標準を再利用可能なエージェントにまとめ、Copilot が作業を効率よく行えるようにします。

このレッスンでは、次の操作を行いました。

- カスタム エージェントの定義方法を確認しました。
- タスクをカスタム エージェントに割り当てました。

Copilot がハイ コントラスト モードを実装している間に、次は [mission control からエージェントのセッションを監視して方向付けます][next-lesson]。

## リソース

- [カスタム エージェントについて][custom-agents]
- [Organization でカスタム エージェントを使うための準備][org-custom-agents]
- [Enterprise でカスタム エージェントを使うための準備][enterprise-custom-agents]

---

| [← 前のレッスン: GitHub Copilot cloud agent][previous-lesson] | [次のレッスン: エージェントの監視と管理 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
