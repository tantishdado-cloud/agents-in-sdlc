---
title: "レッスン 8 - 機能の PR を作成してマージする"
description: "絞り込み機能、指示、スキルの更新、QA プロファイル、テストをまとめてレビューし、PR を作成して Agent Merge を使います。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

絞り込み機能の実装、指示の更新、スキルの更新、品質保証 (QA) プロファイル、テストは、すべて 1 つのブランチに保存されています。まとめてレビューし、プルリクエストを開きましょう。星評価のプルリクエスト (PR) は自分でマージしましたが、今回は **Agent Merge** にプロセスを任せます。

> [!NOTE]
> 通常は、機能、指示の更新、スキルの更新、QA エージェントを複数の PR に分けます。このワークショップでは手順を簡潔にするため、絞り込み機能と品質ワークフロー全体を 1 つの会話とブランチで進め、1 つの PR にまとめています。

このレッスンでは、次のことを行います。

- Agent Merge とは何か、マージまでのライフサイクルをどう自動化するかを学びます。
- 機能全体の変更と検証の根拠を確認します。
- 絞り込み機能の PR を作成します。
- レビュー後に Agent Merge を有効にし、PR がマージされたことを確認します。

## シナリオ

絞り込み機能の作業では、Copilot を使って機能を計画、実装、検証してきました。Tailspin Toys では、マージの許可を開発者の管理下に置いたまま、残りの PR 作業を自動化したいと考えています。

## Agent Merge とは

**Agent Merge** は、プルリクエストをマージするために必要な残りの作業を自動化します。有効にすると、Copilot は PR を妨げている問題 (失敗した継続的インテグレーション (CI) チェックの修正、レビューコメントへの対応、必要に応じたリベースなど) に対処します。その後、リポジトリで許可されていれば、GitHub の自動マージを有効にします。

ここまでは、自分で **Merge pull request** を選択してきました。Agent Merge にその作業を任せることもできます。有効にする前に、作業をレビューして問題がないことを確認してください。

## Agent Merge で PR を管理する

コードの作成が終わったので、まとめてレビューして PR を作成し、残りのプロセスを Agent Merge に任せましょう。

1. Codespace に戻ります。
2. `/agent` を入力してエージェントのダイアログを開きます。
3. 選択肢から **Default** を選び、<kbd>Enter</kbd> を押します。
4. `/pr create` コマンドを使って新しい PR を作成します。
5. `/pr agentmerge` を使って Agent Merge を有効にします。
6. Copilot が PR の継続的インテグレーションを確認します。すべて成功すると、マージが行われます。
7. 「PR #14 was squash-merged successfully.」のようなメッセージが Copilot から表示されることを確認します。

> [!IMPORTANT]
> Agent Merge は、必須の承認、ブランチ保護、マージキュー、リポジトリの設定、不足している権限を回避しません。処理がブロックされた場合は、報告された理由を確認し、リポジトリで許可されている方法で、レビュー済みの変更を手動でマージしてください。

## まとめと次のステップ

コードの生成、テストと検証、プルリクエストのプロセスなど、開発プロセスの一部を自動化しました。具体的には、次のことを行いました。

- Agent Merge とは何か、マージまでのライフサイクルをどう自動化するかを学びました。
- 機能全体の変更と検証の根拠を確認しました。
- 絞り込み機能の PR を作成しました。
- レビュー後に Agent Merge を有効にし、PR がマージされたことを確認しました。

次は[コンテキスト、モデル、共有、任意のクラウド委任に使える Copilot CLI のスラッシュコマンド][next-lesson]を確認します。

## リソース

- [Copilot CLI でプルリクエストを管理する][manage-prs]
- [Copilot CLI コマンドリファレンス][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
