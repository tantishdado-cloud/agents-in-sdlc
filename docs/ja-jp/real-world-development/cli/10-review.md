---
title: "レッスン 10 - まとめと次のステップ"
description: "Copilot CLI のワークフロー、2 つのプルリクエスト、再利用可能なカスタマイズ、追加リソースを振り返ります。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

GitHub Copilot CLI を使い、Tailspin Toys の一連の開発ワークフローに取り組みました。具体的には、次のことを行いました。

- Codespace を準備し、Copilot CLI をインストールし、プロジェクトを確認して、絞り込み機能のイシューを見つけました。
- 星評価を追加し、Codespaces の転送ポートをブラウザーで開いて結果を確認して、最初のプルリクエスト (PR) を手動でマージしました。
- 絞り込み機能のイシューから始めて、Plan モードで進め方を定め、Autopilot モードで実装し、Interactive モードでレビューしました。
- カスタム指示でエージェントをガイドし、既存の `quality-checks` スキルをカスタマイズして、ユニットテスト、lint、型チェックを実行しました。
- Playwright Model Context Protocol (MCP) サーバーを追加し、実際のブラウザーから絞り込み機能を試しました。
- 品質保証 (QA) カスタムエージェントを作成して選択し、要件、カバレッジ、スキルの結果、ブラウザーでの検証結果を評価しました。
- 絞り込み機能の変更全体をレビューし、絞り込み機能の PR で Agent Merge を許可しました。
- コンテキスト、モデル、共有、任意のクラウド委任に使えるスラッシュコマンドを確認しました。

## リリースしたもの

このワークショップでは、更新済みの `main` からそれぞれ別のブランチを作り、2 つの PR を作成しました。

1. **星評価:** 既存の `starRating` と、評価がない場合の状態をゲームカードに表示します。
2. **絞り込み機能と品質ワークフロー:** 絞り込み機能を実装し、指示を更新して機能に適用し、`quality-checks` のレポートをカスタマイズし、QA プロファイルを作成して、関連するテストを追加します。

絞り込み機能の計画から PR の作成までは、同じ会話とブランチを使いました。ワークショップを簡潔にするため、この作業を 1 つの PR にまとめています。

## さまざまな検証方法

自動テスト、自分でのブラウザー確認、MCP を通じた Copilot のブラウザー操作でコードを確認しました。`quality-checks` スキルはユニットテスト、lint、型チェックを実行して、新しい形式で報告しました。QA はこれらの結果に要件とテストカバレッジのレビューを加え、PR を作成する前に全体を確認しました。

追加するテストは、実際に不足しているカバレッジを補うものであるべきです。QA を実行してもテストを追加する必要がない場合は、それが正しい結果であることもあります。マージを許可する前に、コードと検証の根拠を確認してください。変更があった場合は、影響を受ける検証結果を更新します。

## ベストプラクティス

Copilot に与えるコンテキストとツールが、作業内容を左右します。このワークショップでは、指示を更新し、スキルをカスタマイズし、QA プロファイルを作成し、MCP サーバーを設定しました。会話をまたいでこれらのカスタマイズを再利用し、チームのニーズに合わせて調整しましょう。指示は規約を定め、スキルは繰り返し実行するタスクを説明し、カスタムエージェントは専門的な役割を定義し、MCP サーバーは外部ツールに接続します。エージェントの要約だけでなく、実際の変更とツールの結果を確認してください。

タスクに合った**モードとモデル**を選びます。**Plan** は実装前に進め方を検討するとき、**Interactive** は範囲を絞った変更に自分も関わるとき、**Autopilot** は範囲が明確なタスクを任せるときに使います。定型的な編集には高速なモデルを、複雑な作業にはより高性能なモデルを選びましょう。

インフラストラクチャと同じくらい、コンテキストも重要です。何を、なぜ、どのように構築するのかを明確に説明すると、結果が大きく変わります。

## さらに試す

主要なワークフローは以上です。Copilot CLI の機能には、ほかにも次のようなものがあります。

- `/review` を使ってコードレビューエージェントに変更を分析してもらう。
- `/rubber-duck` を使って問題を整理し、別の視点を得る。
- `/fleet` を使って独立したサブタスクを並列に進める。
- `/worktree` を使って別のタスクを分離する。
- `/delegate` を使って Copilot クラウドエージェントにタスクを依頼する。

## 次のステップ

どんなツールも、使い続けることで活用方法が身につきます。本番コード、趣味のコード、何年も思い描きながら手をつけられなかった小さなアプリなどで試してみましょう。学んだことをチームで共有し、互いの経験から学んでください。そして、ドキュメントも引き続き確認しましょう。

ターミナルから Tailspin Toys をさらに拡張する場合は、任意の[Foundry Backer Concierge シリーズ][foundry]に進んでください。別の環境も試すには、[VS Code ワークショップ][vscode]、[GitHub Copilot アプリのワークショップ][app]、または[Copilot クラウドエージェントのワークショップ][cloud]を確認してください。

## リソース

- [GitHub Copilot CLI について][about-cli]
- [Copilot CLI コマンドリファレンス][cli-reference]
- [Copilot CLI をカスタマイズする][customize-cli]
- [Copilot CLI でプルリクエストを管理する][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
