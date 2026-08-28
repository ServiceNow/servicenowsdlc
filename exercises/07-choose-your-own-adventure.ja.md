---
title: "Choose your own adventure"
slide: "28 — Choose your own adventure（演習）"
---

# Choose your own adventure

[← デッキのスライド28に戻る](https://servicenow.github.io/servicenowsdlc/index.ja.html#28) ｜ [English version](https://github.com/ServiceNow/servicenowsdlc/blob/main/exercises/07-choose-your-own-adventure.md)

## Objective（目的）

スタックの中で自分のチームに最も関係の深い部分を、さらに一段深く掘り下げます — 1つのトラックを選び、残りの時間をそこに使いましょう。

## Estimated Time（想定時間）

30分

## Prerequisites（前提条件）

- メンテナンスアプリがビルド・テスト・ソース管理されていること（演習01〜05）

## Exercise Steps（演習の手順）

さらに探索を続けるためのアイデアをいくつか用意しました — 以下のどれを選んでも構いませんし、まったく自分たち独自の探索を考えても構いません。質問があればいつでもお手伝いします。

### トラック：Fluent 深掘り

- メンテナンスアプリでまだ使っていないメタデータ型を追加してみましょう：
  ```
  Add a scheduled job that runs nightly and auto-closes Maintenance
  Requests that have been in "Resolved" state for more than 7 days.
  ```
  ```
  Add a Scripted REST API that lets an external system create a
  Maintenance Request via POST.
  ```
- 既存の UI Builder ページを Fluent のソースに変換し、何が出力されるか見てみましょう：
  ```
  now-sdk transform --sys-id <ui_builder_page_sys_id>
  ```

### トラック：テスト深掘り

- 意図的にテストを壊して、Test Agent がどうトリアージするか見てみましょう：
  ```
  Comment out the routing/assignment step in the Maintenance Request
  flow, then run the ATF test and triage the failure.
  ```
- UI に関わる変更を加え、Test Agent にカバレッジを追加させてみましょう：
  ```
  Add a "Priority" choice field (Low/Medium/High) to the Maintenance
  Request form, visible only to Technicians.
  ```
  この変更に対して ATF テストを作成するか尋ねられたら「Yes, proceed」を選んでください。その後、生成されたテストを開き、新しいフィールドに対して追加された UI ステップを確認してみましょう。
- 要件を1つ削除し、Test Agent が自分で後始末をする様子を見てみましょう：
  ```
  Remove the "Priority" field and its form logic from the Maintenance
  Request app.
  ```
  Test Agent が、そのフィールドをカバーしていた ATF テストがもう不要だと認識して削除する様子を確認しましょう — カバレッジは、追加したときだけでなく両方向でアプリと同期し続けます。

### トラック：CI/CD API

- 演習06の ReleaseOps の UI フローの代わりに、CI/CD REST API（`app_repo/publish`、`app_repo/install`、`testsuite/run`、`app_repo/rollback`）を使って Git トリガーのパイプラインをスクリプト化してみましょう。
- 本番向けターゲットに対して `app_repo/install` を呼び出す前に、パイプラインが一時停止して手動承認を待つよう、承認ゲートを追加してみましょう。

### トラック：ReleaseOps

- 演習06の ReleaseOps ラボを最後まで進めてみましょう：Update Set を Deployment Request にプロモートし、「Ready to Assess」にマークして、アセスメントのプレイブック（Instance Scan → Move to Test → Run ATF）が実行される様子を見届けます。
- ATF が失敗した場合は、意図的に「Need Code Change」の経路をたどってみましょう — Deployment Request が Draft に戻る様子を確認し、修正したペイロードを添えて再度アセスメントします。
- 詳しい手順は [演習06](https://github.com/ServiceNow/servicenowsdlc/blob/main/exercises/06-releaseops.ja.md) を、アーキテクチャの詳細は [ReleaseOps Deep Dive](https://servicenow.github.io/servicenowsdlc/releaseops-deep-dive.html) を参照してください。

### トラック：MCP 連携

- 管理者に Connect Hub 経由で Figma または Miro のボードを接続してもらい、AI Control Tower で承認を得た上で、自分の Build Agent の設定 → MCP タブ → 「Enable MCP servers」で有効化してみましょう。
- 手動で説明する代わりに、接続された仕様から直接ビルドするよう Build Agent にプロンプトしてみましょう：
  ```
  Look at the connected Figma file and build the UI page it shows for
  the Maintenance Request list view.
  ```

### トラック：カスタムルール（プレースホルダー — 発表前に確認が必要）

- _このトラックは、Build Agent の出力の言葉づかい／用語を変更するカスタムルールを扱います。ライブで発表する前に、正確な仕組み、設定場所、実際に動作するサンプルプロンプトを確認してください — まだ検証されていません。_

### トラック：2つのトラックを組み合わせる

- CI/CD トラックの承認ゲートを、テストトラックの ATF テストが `testsuite/run` を通じて成功した場合にのみ `app_repo/install` を呼び出すように配線してみましょう。

### または：やり残したボーナス課題に戻る

これまでの各演習（01〜06）にはそれぞれ Bonus Challenge のセクションがあり、多くのチームはその場では時間が足りません。今がそれに戻って1つ取り組む良い機会です — 上記のどのトラックとも同じくらい価値があります。

## Success Criteria（達成基準）

- [ ] コア演習で扱った内容を超えて、少なくとも1つのトラックを実際に手を動かして体験した
- [ ] そのトラックから、具体的なもの（コマンド、設定、成功／失敗したテストなど）を1つ示せる

## Learning Points（学びのポイント）

- コア演習ではゴールデンパスをエンドツーエンドで扱いました。ここにある各トラックは、そのゴールデンパスを実践の中でさらに深めるためのものです。
