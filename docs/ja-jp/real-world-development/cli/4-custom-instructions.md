---
title: "レッスン 4 - カスタム指示で Copilot をガイドする"
description: "リポジトリの指示を確認し、ドキュメントの規約を追加して、絞り込み機能のコードに適用します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

生成 AI を使うときは、コンテキストが重要です。特定の方法でタスクを実行する必要があるなら、そのガイダンスを Copilot が利用できるようにしましょう。[指示ファイル][instruction-files]では、必要なコードだけでなく、その構成方法も説明できます。絞り込み機能を実装したので、Copilot が参照した指示を確認し、ドキュメントの規約を追加して、コードに適用します。

このレッスンでは、次のことを行います。

- リポジトリの指示とパスに限定した指示ファイルがエージェントにどう伝わるかを確認します。
- コーディング規約に従うよう、指示ファイルを更新します。
- 指示ファイルがコードに与える影響を確認します。

## シナリオ

Tailspin Toys では、開発プラクティスに関するガイドラインと要件を定めています。

- コメントでは、コードを言い換えるのではなく、意図や自明でない判断を説明します。
- `db/` と `src/lib/` のエクスポート関数には、目的、パラメーター、戻り値を記述する TSDoc/JSDoc を付けます。該当する場合は、差し替え可能な `db` 引数も含めます。
- 再利用可能な Astro コンポーネントには `Props` の契約を記述し、関連コードが変わったらコメントも最新に保ちます。
- 既存のフォーマットと lint のガイダンスは維持します。

指示ファイルを使って、Copilot がこれらのプラクティスに沿ってタスクを実行できるようにします。

## 指示ファイル

カスタム指示を使うと、コーディングスタイルや希望を Copilot に伝え、より適切な提案やコードスニペットを得られます。好みのコーディング規約、ライブラリ、コメントの書き方などを指定できます。リポジトリ全体に適用する指示も、特定のファイルやタスクに限った指示も作成できます。

指示ファイルには 2 種類あります。

- `.github/copilot-instructions.md` は、リポジトリへのすべてのリクエストで Copilot に送られる単一の指示ファイルです。多くのリクエストに関係するプロジェクトレベルの情報を記載します。
- `.github/instructions/*.instructions.md` は、特定の言語、ファイルの種類、タスクに関するガイドラインを提供します。

> [!NOTE]
> その他の指示形式への対応状況は、環境によって異なります。特定の形式を前提にする前に、[カスタム指示のサポートに関するリファレンス][custom-instructions-support]を確認してください。

## このプロジェクトのカスタム指示ファイルを確認する

作業を始めるための指示ファイルがスタータープロジェクトに含まれています。変更前に内容を確認して、影響を把握しましょう。

1. Codespace に戻ります。
2. Codespaces のエディターで (ターミナルではなく)、`.github/copilot-instructions.md` を開きます。
3. プロジェクトの簡単な説明とコーディングに関するガイダンスを確認します。この指示は、Copilot とのすべてのやり取りに適用されます。
4. `.github/instructions` フォルダーを開き、ファイルを確認します。Astro ファイル、Drizzle データレイヤー、テストなどに関する指示があります。
5. `.github/instructions/unit-tests.instructions.md` を開き、冒頭の `applyTo` フィールドを確認します。このフィールドでは、指示が適用されるファイルを指定する glob を設定します。
6. `.github/instructions/drizzle.instructions.md` を開き、他の指示ファイルや既存のプロジェクトファイルへのリンクを確認します。こうしたリンクを使って、大きな指示セットを再利用可能な小さなファイルに分け、Copilot に参考例を示せます。

## チームのガイダンスに合わせて指示ファイルを更新する

既存の指示はよい出発点ですが、まだ不足があります。新たに生成する TypeScript に TSDoc コメントを追加するよう、コアの `copilot-instructions.md` を変更します。

1. `.github/copilot-instructions.md` で、**Code formatting guidance** というセクションを探します。35 行目あたりにあります。
2. 最後の箇条書きとして、次を追加します。

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

変更は自動的に保存されます。

## 更新したガイダンスを使う

Copilot CLI は会話の開始時にリポジトリの指示を読み込みます。機能のコンテキストを保ったまま新しいガイダンスを反映するため、指示を保存した後、絞り込み機能の会話を終了して再開します。

1. 絞り込み機能の会話を続けている場合は、`/exit` を入力して終了します。Copilot CLI は会話を自動的に保存します。
2. リポジトリのルートにある `game-filters-cli` ブランチから、保存した会話を再開します。

   ```bash
   copilot --resume
   ```

   選択を求められた場合は、絞り込み機能の会話を選び、`game-filters-cli` ブランチがチェックアウトされていることを確認します。
3. 更新したガイダンスを適用するよう Copilot に依頼します。

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. `/diff` を入力し、変更された TypeScript ファイルを確認します。新たに生成された TSDoc コメントがコードを正確に説明していることを確認します。

## まとめと次のステップ

Copilot CLI が指示をどのように読み込むかを確認し、新しい規約を機能に適用しました。具体的には、次のことを行いました。

- リポジトリの指示とパスに限定した指示ファイルがエージェントにどう伝わるかを確認しました。
- コーディング規約に従うよう指示ファイルを更新しました。
- 指示ファイルがコードに与える影響を確認しました。

次は[再利用可能な quality-checks スキルをカスタマイズして実行し][next-lesson]、lint とテストが一貫して実行されるようにします。

## リソース

- [Copilot CLI にカスタム指示を追加する][instruction-files]
- [カスタム指示のサポート][custom-instructions-support]
- [効果的なカスタム指示を作成するためのベストプラクティス][instructions-best-practices]
- [Awesome Copilot — 指示ファイルなどのリソース集][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
