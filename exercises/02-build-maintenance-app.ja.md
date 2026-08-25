---
title: "メンテナンスアプリのビルド＋テスト"
slide: "15 — ビルド"
---

# メンテナンスアプリのビルド＋テスト

[← デッキのスライド15に戻る](https://servicenow.github.io/servicenowsdlc/index.ja.html#15) ｜ [English version](https://github.com/ServiceNow/servicenowsdlc/blob/main/exercises/02-build-maintenance-app.md)

## Objective（目的）

チームが先ほど作成した build-ready な要件を使って、Build Agent（と ATF）で自然言語のプロンプトから動作するメンテナンス依頼アプリを構築します。

## Estimated Time（想定時間）

60分

## Prerequisites（前提条件）

- サンドボックスが割り当てられ、フィーチャーブランチが作成済みであること
- 前のアクティビティで作成した build-ready な要件（エンティティ、ロール／アクセス、Given-When-Then）
- Build Agent の設定（General タブ）で「Sync ATF tests with app」が有効になっていること — これにより、ビルドしながら ATF テストが生成され、アプリの変更に合わせて同期され続けます

![「Sync ATF tests with app」がオンになっている Build Agent の設定画面](images/ba-settings-atf.png)

## Exercise Steps（演習の手順）

1. Studio 内で Build Agent を開きます。オフインスタンスでの利用は後ほど扱います。
2. build-ready な要件を使って Build Agent にプロンプトを与えます — エンティティ、ロール／アクセス、Given-When-Then シナリオをプロンプトの中に直接名前で記述してください。要件定義のアクティビティで参考シナリオを使ったチームは、そのままこの内容を貼り付けてください。そうでない場合は、自分たちのエンティティ／ロール／Given-When-Then に置き換えてください：
   ```
   Build a Maintenance Request app with:

   Entities: Maintenance Request, Equipment

   Roles:
   - Requester: create and view their own Maintenance Requests
   - Technician: view assigned Maintenance Requests, update their status

   Given a Requester submits a Maintenance Request for active Equipment,
   when they submit it,
   then a Maintenance Request record is created in "New" state and
   assigned according to a routing rule.
   ```
3. Build Agent が作成したものを確認しましょう。裏側で何が起きているか気になる場合は、Fluent（`.now.ts`）コードを表示するよう依頼してみてください — これは任意で、アーキテクト向けの内容です：
   ```
   Show me the Fluent (.now.ts) code you just generated for this app.
   ```
   Maintenance Request テーブルの Fluent コードは、次のようなものになるはずです：
   ```typescript
   export const MaintenanceRequest = Table({
     label: 'Maintenance Request',
     fields: { equipment: Reference('equipment') }
   });
   ```
4. 変更に対して ATF テストを作成するか尋ねられたら、「Yes, proceed」を選択してください — 変更内容にテストカバレッジが追加されます。
5. アプリをサンドボックスにインストールします：
   ```
   now-sdk install --sandbox
   ```
6. サンドボックス内でゴールデンパスを手動で一通り確認し、Given-When-Then シナリオと一致することを確かめます。

## Success Criteria（達成基準）

- [ ] 要件で定義したエンティティが、サンドボックス内でテーブルとして存在している
- [ ] ロール／アクセスがチームで定義した内容と一致している
- [ ] ゴールデンパスがエンドツーエンドで動作し、Given-When-Then シナリオと一致している
- [ ] 少なくとも1つの ATF テストが成功している

## Learning Points（学びのポイント）

- SDK と Fluent は、自然言語のプロンプトを型付きで診断可能なソースに変換します — 中身を確認したり diff を取ったりできないクリックのブラックボックスではありません。
- 機能をビルドするのと同じセッション内で ATF テストを書くことで、テストは誰も戻ってこないフォローアップ作業ではなく、開発ループの中にとどまります。
- Test Agent は標準でおよそ13種類のステップタイプ（フォーム、サーバー、REST など）をカバーします。サポートされていないステップタイプが必要なエッジケースがある場合は、標準の UI テストエディタ（All > Automated Testing Framework > Tests）でそのステップを手動で作成してください。

## Bonus Challenge（ボーナス課題）

- Technician への割り当て／ルーティングロジックを追加してみましょう：
  ```
  Add routing logic so new Maintenance Requests are automatically
  assigned to the Technician with the fewest open assignments.
  ```
- エッジケースや拒否パスをカバーする ATF テストを追加してみましょう：
  ```
  Generate an ATF test for the case where a Technician tries to update
  a Maintenance Request that isn't assigned to them. Confirm the update
  is rejected.
  ```
- ロールベースのアクセス制限を追加し、権限のないユーザーとして動作を確認してみましょう：
  ```
  Add access control so only the assigned Technician (or an admin) can
  update a Maintenance Request's status, and Requesters can only view
  their own requests.
  ```
