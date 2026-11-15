---
title: "演習 1 - カスタム指示 (クラウドエージェント)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: Copilot クラウドエージェント →][next-lesson]

生成 AI を使うときは、コンテキストが重要です。タスクを特定の方法で実行する必要がある場合や、Copilot に知っておいてほしい背景情報がある場合は、そのコンテキストを参照できるようにしましょう。[指示ファイル][instruction-files]を使うと、Copilot に実行内容だけでなく方法も伝えられます。

この演習では、次のことを行います。

- リポジトリのカスタム指示とパスに限定した指示ファイルを通じて、プロジェクト固有のコンテキスト、コーディングガイドライン、ドキュメントの規約が Copilot にどう伝わるかを確認します。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する新しい規約を追加します。

> [!NOTE]
> VS Code や CLI の実習環境とは異なり、この演習では変更の前後でプロンプトを実行しません。Copilot クラウドエージェントは GitHub イシューに対して非同期で作業するため、影響をリアルタイムに並べて示すのが難しいためです。以降の演習でクラウドエージェントが作成したプルリクエストをレビューするときに、指示ファイルの効果を確認します。

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

## リポジトリの新しい規約を追加する

ここで行う編集は 1 つです。コード内のドキュメントには TSDoc のドキュメントコメントとファイルレベルのコメントヘッダーを使うという、プロジェクト全体の規則を追加します。後の演習でクラウドエージェントにイシューを割り当てると、この指示が使われます。

編集する前に、作業用のブランチを作成します。クラウドエージェントは対象のイシューが指定するブランチから指示を読み込みますが、変更を整理してコミットしておきます。

1. Codespace のターミナルで、新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions-cloud
   ```

前の説明のとおり、`.github/copilot-instructions.md` は Copilot にプロジェクトレベルの情報を提供します。コードの提案を改善するために、リポジトリのコーディング規約を記載しましょう。

1. `.github/copilot-instructions.md` をもう一度開きます。
2. 27 行目あたりにある **Code formatting requirements** セクションを探します。ここにはプロジェクトのコーディング規約が記載されていますが、コード内ドキュメントの規則はありません。そのため、生成されたヘルパーにドキュメントコメントがありませんでした。
3. 既存の規約のすぐ下に次の Markdown を追加し、ファイルヘッダーと TSDoc のドキュメントコメントを追加するよう Copilot に指示します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> 前のレッスンで確認したように、指示ファイルはリポジトリレベル (`.github/copilot-instructions.md`) に作成して全体のガイダンスに使うことも、特定の言語、ファイルの種類、タスクに向けた `*.instructions.md` ファイルとして作成することもできます。ここで追加したドキュメントコメントの規則のような、プロジェクト全体の規約にはリポジトリレベルのファイルが適しています。

## 指示の変更をコミット、プッシュ、マージする

クラウドエージェントは、対象のイシューが指定するブランチから指示ファイルを読み込みます。次の演習で Copilot にイシューを割り当てると、`main` からブランチが作成されます。したがって、クラウドエージェントが指示を使えるように、変更を `main` にマージする必要があります。

1. ステージしてコミットします。

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. ブランチをプッシュします。

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. github.com で `custom-instructions-cloud` から `main` へのプルリクエストを開き、マージします。次の演習で割り当てたイシューに取り組むとき、クラウドエージェントはこの指示を読み込みます。

> [!TIP]
> このワークショップで `main` に直接作業したい場合は、ブランチを作成せずに `main` へコミットしてもかまいません。実際のプロジェクトでの手順に近づけるため、ここではブランチを使っています。

## まとめと次のステップ

このプロジェクトの指示ファイルから Copilot がコンテキストを読み取る方法を確認し、`.github/copilot-instructions.md` にリポジトリ全体の新しい規約を追加しました。残りの実習環境でクラウドエージェントが作成するプルリクエストを確認するときに、この規約の効果を確かめられます。

次は[最初のイシューを Copilot クラウドエージェントに割り当てます][next-lesson]。

## リソース

- [GitHub Copilot のカスタマイズに使う指示ファイル][instruction-files]
- [効果的なカスタム指示を作成するためのベストプラクティス][instructions-best-practices]
- [Copilot のカスタム指示を改善する 5 つのヒント][copilot-instructions-five-tips]
- [Awesome Copilot — 指示ファイルなどのリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
