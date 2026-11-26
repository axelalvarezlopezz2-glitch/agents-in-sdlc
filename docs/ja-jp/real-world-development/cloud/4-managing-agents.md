---
title: "演習 4 - エージェントの監視と管理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

前のいくつかの演習では、ユーザー体験の改善と機能追加のため、Copilot クラウドエージェントに 3 つのタスクを依頼しました。クラウドエージェントは非同期かつ自律的に作業しますが、タスクの進行状況を監視することも大切です。

クラウドエージェントに割り当てたタスクは、GitHub.com の[エージェントページ][agents-page]など、さまざまなツールで管理できます。ここでは、オープンな pull request (PR) があるすべてのエージェントタスクを確認できます。実行内容を調べたり、進行中のセッションに指示を追加したりできます。

このレッスンでは、次の操作を行います。

- エージェントページでクラウドエージェントのタスクを監視します。
- 進行中のセッションに指示を追加し、機能をリクエストします。

## シナリオ

エージェントにハイコントラストモードの作成を依頼した後、チームはライトモードも追加することにしました。サイトのスタイルを更新して切り替え機能を追加する作業を進めているため、同時に実装するのが合理的です。ハイコントラストに加えてライトモードも追加するよう、エージェントに指示します。

## Copilot クラウドエージェントのタスクを確認する

Copilot クラウドエージェントに割り当てたタスクの現在の状況を確認します。

1. [Copilot エージェントページ](https://github.com/copilot/agents)を開きます。
2. メインペインと左ペインの両方に表示されるタスクの一覧を確認します。次のようなタスクが表示されます。
   - コードベースのドキュメントを更新する。
   - ゲームを変更する API を生成する。
   - Web サイトにハイコントラストモードを追加する。
3. 実行中のタスクを選択し、Copilot が実行した作業を確認します。次のような手順が含まれることがあります。
   - リポジトリからコードをチェックアウトする。
   - Copilot の作業環境を作成する。
   - MCP サーバーをセットアップする。
   - 割り当てられたタスクを完了するための作業を進める。

> [!NOTE]
> 表示される手順は、Copilot の作業状況や進め方によって異なります。

4. 右側に表示される pull request (PR) ペインも確認してください。PR と変更されたファイルを確認し、作業をさらに監視できます。

## クラウドエージェントに指示を追加する

現在実行中のタスクを確認できました。次に、ハイコントラストモードの作業中にライトモードの切り替えも追加するよう依頼します。

1. ハイコントラストモードの追加に関するセッションを選択します。表示されるタイトルは、Copilot が選んだ名前や作業状況によって異なります。

    ![ミッションコントロールのアクセシビリティセッション](../../../_images/ex6-accessibility-session.png)

2. セットアップが完了して作業が始まったことを示すメッセージが表示されるまで、数分間セッションを確認します。
3. **Steer active session while Copilot is working** ダイアログに次のプロンプトを入力します。

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![Copilot エージェントページで、Steer active session while Copilot is working ダイアログを示すスクリーンショット](../../../_images/ex6-steer-cloud-agent-task.png)

4. <kbd>Enter</kbd> を押してプロンプトを送信します。
5. Copilot がプロンプトを受け取り、作業計画に反映することを確認します。

## Copilot の作業を待つ

前回と同様に、Copilot は更新されたタスクに取り組みます。メッセージを送信した時点で実行中だったステップを完了した後、新しいリクエストを作業に取り込みます。

完了までには数分かかります。その間に休憩して、ここまでに学んだことを振り返りましょう。

## まとめと次のステップ

Copilot クラウドエージェントのタスクを監視して指示するための中心的な場所である、Copilot エージェントページについて学びました。アクティブなタスクと完了したタスクを追跡し、実行中の作業をレビューし、作業中のタスクに指示を追加できます。

次の概念を確認しました。

- ミッションコントロールとエージェントページで、クラウドエージェントのタスクを監視する。
- 進行中のセッションに追加の機能をリクエストする。

Copilot がアクセシビリティ機能を実装している間に、Copilot が作成した pull request を[改善する方法][next-lesson]を確認しましょう。

## リソース

- [Copilot エージェントページ][agents-page]
- [カスタムエージェントについて][custom-agents]

---

| [← 前のレッスン: カスタムエージェント][previous-lesson] | [次のレッスン: Copilot の作業を反復して改善する →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
