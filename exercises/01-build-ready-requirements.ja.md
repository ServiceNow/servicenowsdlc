---
title: "Build-ready な要件定義：曖昧なストーリーを書き直す"
slide: "10 — アクティビティ"
---

# Build-ready な要件定義：曖昧なストーリーを書き直す

[← デッキのスライド10に戻る](https://servicenow.github.io/servicenowsdlc/index.ja.html#10) ｜ [English version](https://github.com/ServiceNow/servicenowsdlc/blob/main/exercises/01-build-ready-requirements.md)

## Objective（目的）

コードを書き始める前に、曖昧なユーザーストーリーを — エンティティ、ロール／アクセス、Given-When-Then の受け入れ条件を備えた — build-ready なストーリーに書き直す練習をします。

## Estimated Time（想定時間）

5分

## Prerequisites（前提条件）

- 特にありません。ペンと紙、または共有ドキュメントがあれば十分です。

## Exercise Steps（演習の手順）

1. チームで、次の曖昧なストーリーから始めます：

   > 「設備担当ユーザーとして、修理してもらうために設備のメンテナンスを依頼したい。」

2. 5分間で、以下を明確にして build-ready なストーリーに書き直します：
   - **エンティティ（Entities）** — どのテーブル／レコードが関係するか？
   - **ロールとアクセス（Roles & access）** — 誰が各エンティティを作成・参照・操作できるか？
   - **Given–When–Then** — 具体的な受け入れシナリオを少なくとも1つ。
3. 書き直した build-ready 版を書き留めます。
4. グループへの発表に備えてください — 次に進む前に、いくつかのチームの回答を比較します。

## Success Criteria（達成基準）

- [ ] 少なくとも2つのエンティティが特定されている（例：Maintenance Request、Equipment）
- [ ] 少なくとも2つのロールが定義され、それぞれ作成／参照／更新できる内容が明記されている
- [ ] 主要な（正常系の）パスに対する、完全な Given-When-Then シナリオが1つある

## Learning Points（学びのポイント）

- 曖昧なストーリーは、曖昧さを後工程に押し流します。その結果、計画時に意図的に解消されるべきことが、ビルド中に場当たり的に解消されてしまいます。
- エンティティ、ロール／アクセス、Given–When–Then は「build-ready」と呼べる最低限の基準です — これらは、ビルドする人（人間であれ AI であれ）が推測に頼らず行動するために必要な情報です。

## Bonus Challenge（ボーナス課題）

- 拒否またはエッジケース（例：すでにメンテナンス中の設備）に対する2つ目のシナリオを追加してみましょう
- 依頼が Technician にどのように割り当てられるか、ルーティングルールを具体的に定義してみましょう
