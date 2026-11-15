---
title: "演習 1 - カスタム指示 (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: エージェントモード →][next-lesson]

生成 AI を使うときは、コンテキストが重要です。タスクを特定の方法で実行する必要がある場合や、Copilot に知っておいてほしい背景情報がある場合は、そのコンテキストを参照できるようにしましょう。Copilot に特定のコンテキストを伝える方法はいくつかあります。中でも重要なのが、コード生成のガイダンスを提供する[指示ファイル][instruction-files]です。

この演習では、次のことを行います。

- リポジトリのカスタム指示とパスに限定した指示ファイルを通じて、プロジェクト固有のコンテキスト、コーディングガイドライン、ドキュメントの規約が Copilot にどう伝わるかを確認します。
- 現在の指示のまま、絞り込み機能の最初のデータ部分 (販売元ヘルパー) を生成します。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する新しい規約を追加します。
- 同じプロンプトをもう一度実行して、生成コードが新しい規約を採用する様子を確認します。
- 指示の変更と絞り込み機能の基盤を `main` にコミットし、Copilot が次の演習で新しいガイダンスを使えるようにします。

> [!CAUTION]
> 生成コードが、設定した規約の一部に従わない場合があります。Copilot の動作は確率的です。この演習の目的は、指示の変更後に動作がどう**変化する傾向にあるか**を確認することであり、出力を一字一句一致させることではありません。

## 指示ファイル

### シナリオ

Tailspin Toys では、開発プラクティスに関するガイドラインと要件を定めています。

- データレイヤーには、必ずユニットテストを用意します。
- UI はダークモードにし、現代的な印象にします。
- コードには TSDoc のドキュメントコメントを追加します。
- 各ファイルの先頭に、ファイルの役割を説明するコメントを追加します。

指示ファイルを使って、Copilot がこれらのプラクティスに沿ってタスクを実行できるようにします。

### カスタム指示

カスタム指示を使うと、コーディングスタイルや希望を Copilot に伝え、より適切な提案やコードスニペットを得られます。好みのコーディング規約、ライブラリ、コメントの書き方などを指定できます。リポジトリ全体に適用する指示も、特定のファイルやタスクに限った指示も作成できます。

指示ファイルには 2 種類あります。

- `.github/copilot-instructions.md` は、リポジトリへのすべてのリクエストで Copilot に送られる単一の指示ファイルです。多くのチャットや CLI のリクエストに関係する、プロジェクトの技術スタック、開発対象の概要、ベストプラクティスなどを記載します。
- `.github/instructions/*.instructions.md` は、特定のタスクやファイルの種類に作成できます。TypeScript や Astro などの言語別ガイドラインや、UI コンポーネントの作成、ユニットテストの追加などのタスクに関する指示を記載できます。

> [!NOTE]
> IDE では、指示ファイルは Copilot Chat のコード生成にのみ使われます。コード補完や次の編集の提案には使われません。
>
> Copilot Chat、Copilot CLI、Copilot クラウドエージェントは、コード生成時にリポジトリレベルの指示と `applyTo` フロントマター付きの `*.instructions.md` ファイルの両方を使います。
>
> Copilot は [AGENTS.md や CLAUDE.md][custom-instructions-support]など、他の規格に基づく指示ファイルにも対応しています。

### 指示ファイルを管理するためのベストプラクティス

指示ファイルの作成方法を詳しく説明することは、このワークショップの範囲外です。ただし、サンプルプロジェクトには代表的な構成例があります。概要は次のとおりです。

- `copilot-instructions.md` には、開発対象やプロジェクト構成の説明、全体に適用するコーディング規約など、プロジェクトレベルのガイダンスを記載します。
- `*.instructions.md` には、ファイルの種類 (ユニットテスト、Astro コンポーネント、データレイヤーなど) や特定のタスクに関する指示を記載します。
- 自然な言葉を使い、明確なガイダンスと、コードのよい例・悪い例を示します。

指示ファイルの作り方に決まった形がないのと同じように、AI の使い方にも決まった形はありません。プロジェクトやシナリオに合った方法を、試しながら見つけましょう。

> [!TIP]
> GitHub Copilot を使うプロジェクトでは、充実した指示ファイルを用意しましょう。このプロジェクトの指示を確認すると、[UI の更新][ui-instructions]や [Astro][astro-instructions]など、さまざまなタスク用のファイルが見つかります。
>
> Copilot に指示ファイルを作成してもらうこともできます。画面によって操作は異なります (VS Code の **Configure Chat → Generate Agent Instructions**、Copilot CLI の `/init` など)。関連するレッスンで操作を説明します。
>
> テンプレートや出発点を探す場合は、指示ファイル、カスタムエージェント、その他のリソースをまとめた [awesome-copilot][awesome-copilot] を確認してください。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## このプロジェクトのカスタム指示ファイルを確認する

このリポジトリに用意されている指示ファイルを読んでみましょう。コアの `copilot-instructions.md` が 1 つと、さまざまなタスク向けの `*.instructions.md` ファイルがあります。エディターまたは GitHub の Web UI で開きます。

1. `.github/copilot-instructions.md` を開きます。
2. プロジェクトの簡単な説明と、**Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。**Code standards** には **GitHub Actions Workflows** のガイダンスもあります。これらの指示は、Copilot とのあらゆるやり取りに適用されます。
3. `.github/instructions` フォルダーを開いて内容を確認します。Astro ファイル、Drizzle データレイヤー、テストなどに関する指示があります。
4. `.github/instructions/unit-tests.instructions.md` を開き、先頭の `applyTo` フィールドを確認します。このフィールドでは、指示の適用対象をリポジトリのルートからの相対 glob で指定します。ここでは、`**/*.test.ts` に一致するファイルなど、TypeScript のテストファイルが対象です。
5. このプロジェクトでユニットテストを作成する際の指示を確認します。
6. `.github/instructions/drizzle.instructions.md` を開いて末尾までスクロールし、`unit-tests.instructions.md` などの他の指示ファイルや既存のプロジェクトファイルへのリンクを確認します。こうしたリンクを使って、大きな指示セットを再利用可能な小さなファイルに分け、コード生成時に Copilot へ参考例を示せます。(ここでのパスはリポジトリのルートではなく、指示ファイルからの相対パスです。)

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションにはプロジェクトのコーディング規約が記載されていますが、コード内のドキュメントに関する要件はまだありません。次の手順で、TSDoc のドキュメントコメントとファイルヘッダーの規則を追加します。

## 変更用ブランチを作成する

ベストプラクティスに従い、変更用のブランチを作りましょう。

1. 前の演習で使った Codespace に戻ります。
2. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して、新しいターミナルを開きます。
3. ブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions
   ```

## 指示を更新する**前**に Copilot Chat を使う

> [!TIP]
> **Copilot Chat を開く**
>
> 次の演習を始める前に、Codespace に戻り、Copilot Chat パネルを開き、**New Chat** を選択して新しい会話を始めます。モードとモデルの選択は演習によって異なります。必要な場合は各手順で説明します。

カスタム指示の影響を確認するため、まず現在の指示ファイルのままでプロンプトを送ります。後で指示を更新して、同じプロンプトを再送信し、違いを見ます。

1. Copilot が必要なコンテキストだけを使えるよう、前の演習で開いたエディターのタブを閉じます。
2. `src/lib/publishers.ts` を開き、ヘルパーを配置する場所を Copilot に示します。
3. Chat ビューのエージェントのドロップダウンから **Agent** を選び、ファイルの変更を適用できるようにします。

   ![Chat ビューのエージェント選択を示すスクリーンショット](../../../_images/shared-chat-mode-selector.png)

4. 次のプロンプトを送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot がプロジェクトを調べてコードを更新します。ヘルパーファイルとテストなど、複数のファイルが変更されることがあります。
6. 提案されたヘルパーは型付き関数で、最初の引数として `db` クライアントを受け取り、型付き販売元配列を返します。これは `src/lib/*.ts` に適用される `.github/instructions/drizzle.instructions.md` のデータレイヤー規約に基づいています。
7. 提案されたコードには、TSDoc のドキュメントコメントとファイルレベルのコメントヘッダーが**ない**ことを確認します。

> [!CAUTION]
> Copilot は確率的に動作するため、指示がなくてもドキュメントコメントを追加する可能性があります。その場合も問題ありません。重要なのは指示を更新した後の一貫性の改善です。

## リポジトリの新しい規約を追加する

前の説明のとおり、`.github/copilot-instructions.md` は Copilot にプロジェクトレベルの情報を提供します。コードの提案を改善するために、リポジトリのコーディング規約を記載しましょう。

1. `.github/copilot-instructions.md` をもう一度開きます。
2. 27 行目あたりにある **Code formatting requirements** セクションを探します。ここにはプロジェクトの規約が記載されていますが、コード内ドキュメントのルールがありません。そのため、生成されたヘルパーにドキュメントコメントがありませんでした。
3. 既存の規約のすぐ下に次の Markdown を追加し、ファイルヘッダーと TSDoc のドキュメントコメントを追加するよう Copilot に指示します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> 前のレッスンで確認したように、指示ファイルはリポジトリレベル (`.github/copilot-instructions.md`) に作成して全体のガイダンスに使うことも、特定の言語、ファイルの種類、タスクに向けた `*.instructions.md` ファイルとして作成することもできます。ここで追加したドキュメントコメントの規則のような、プロジェクト全体の規約にはリポジトリレベルのファイルが適しています。

## プロンプトを再実行して変更を確認する

1. Copilot Chat に戻り、**New Chat** を選択してバッファーをクリアします。
2. `src/lib/publishers.ts` を再度選択し、Copilot が適切なファイルに注目するようにします。
3. 先ほどと**同じプロンプト**を送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 提案されたファイルの先頭に、次のようなコメントブロックが追加されていることを確認します。

   ```typescript
   /**
    * Tailspin Toys クラウドファンディングプラットフォーム用の販売元データアクセスヘルパー。
    * データベースから販売元情報を取得する関数を提供します。
    */
   ```

5. 提案された関数に、次のような TSDoc コメントが含まれていることを確認します。

   ```typescript
   /**
    * すべての販売元について、ID と名前の一覧を返します。
    *
    * @param db - Drizzle データベースクライアント。
    * @returns 販売元オブジェクトの配列に解決される Promise。
    */
   ```

これで新しいプロジェクト規約に従うよう Copilot をガイドし、次の演習で使うコードを作成できました。

## 指示とブランチをコミットする

指示ファイルもリポジトリ内の他のファイルと同じく、ソース管理で管理します。変更をコミットしてブランチをリポジトリにプッシュしましょう。

1. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して、新しいターミナルを開きます。
2. 次のコマンドを実行し、指示の更新とヘルパーの変更があることを確認します。

   ```bash
   git status
   ```

3. ターミナルで指示ファイルと Copilot がヘルパーの基盤として変更したファイルをステージし、コミットします。

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # git status に追加の関連変更 (テストなど) が表示された場合は、それらのファイルも追加します。
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. ブランチをリポジトリにプッシュします。

   ```bash
   git push -u origin custom-instructions
   ```

## プルリクエストを作成してマージする

ブランチをプッシュできたので、プルリクエストを作成し、バックログにあるドキュメント規約のイシューと関連付けます。手動で行う代わりに、すでにプロジェクトに接続されている GitHub ツールを使って Copilot に依頼できます。該当するイシューを探し、そのイシューをクローズする PR を作成して、マージを依頼しましょう。

> [!NOTE]
> この演習では初めて Copilot に GitHub 上の操作を依頼します。プロジェクトのテンプレートでは、GitHub ツールがワークスペースに接続済みです。セットアップは不要ですが、Copilot が初めてツールを使うときに VS Code から GitHub へのサインインを求められることがあります。画面の案内に従ってください。後の演習では、この接続の仕組み (Model Context Protocol) について説明します。

1. Codespace で Copilot Chat を開きます。
2. <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) を押して Copilot Chat ビューを開き、エージェント選択で **Agent** が選ばれていることを確認します。
3. 指示ファイルの更新に関するイシューを探し、現在のブランチから PR を作成して、指示の更新と販売元ヘルパーの基盤の両方を説明するよう Copilot に依頼します。

   ```plaintext
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot がイシューを探し、PR を作成します。
5. Copilot が GitHub 操作の実行許可を求めたら、コマンドを確認して、適切な場合に **Allow** を選択します。
6. PR が作成されたら、次のプロンプトでマージしてローカルのブランチを `main` に戻すよう依頼します。

   ```plaintext
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. Copilot が GitHub 操作とシェルコマンドの実行許可を求めたら、コマンドを確認して、適切な場合に **Allow** を選択します。

GitHub Copilot のサポートを使って、プルリクエストを作成してマージできました。

## まとめと次のステップ

このプロジェクトの指示ファイルから Copilot がコンテキストを読み取る方法を確認し、VS Code の Copilot Chat を使って次のことを行いました。

- 既存の指示のままコード生成プロンプトを送信し、Copilot の出力を確認しました。
- `.github/copilot-instructions.md` にリポジトリ全体の新しい規約を追加しました。
- 同じプロンプトを再実行し、提案されたコードが新しい規約を採用する様子を確認しました。
- 指示の変更とヘルパーの基盤を `main` にコミットし、次の演習で使えるようにしました。

次は[エージェントモード][next-lesson]で指示を使い、Copilot にコードベース全体で新機能を追加してもらいましょう。

## リソース

- [GitHub Copilot のカスタマイズに使う指示ファイル][instruction-files]
- [効果的なカスタム指示を作成するためのベストプラクティス][instructions-best-practices]
- [Copilot のカスタム指示を改善する 5 つのヒント][copilot-instructions-five-tips]
- [GitHub Copilot の個人用カスタム指示][personal-instructions]
- [Awesome Copilot — 指示ファイルなどのリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
