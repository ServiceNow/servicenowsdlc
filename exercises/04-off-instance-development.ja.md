---
title: "オフインスタンス開発：Fluent 向け AI Skills"
slide: "21 — オフインスタンス開発"
---

# オフインスタンス開発：Fluent 向け AI Skills

[← デッキのスライド21に戻る](https://servicenow.github.io/servicenowsdlc/index.ja.html#21) ｜ [English version](https://github.com/ServiceNow/servicenowsdlc/blob/main/exercises/04-off-instance-development.md)

## Objective（目的）

ServiceNow SDK の AI Skills プラグインをコーディングエージェント（Claude Code、Cursor、Windsurf など）と組み合わせて使い、プラットフォームの外でメンテナンスアプリの既存の動作を変更します — 何か新しいものを追加するのではなく。Studio の外でも同じ `now-sdk build`／`now-sdk install` のループと、テストが壊れた際の自動修復の挙動が機能することを確認します。

## Estimated Time（想定時間）

15〜20分

## Prerequisites（前提条件）

- ビルド＋テスト演習で使用したサンドボックスと Fluent プロジェクト
- ローカルにインストールされたコーディングエージェント（Claude Code、Cursor、Windsurf など）
- 使用するコーディングエージェント向けに ServiceNow SDK の AI Skills プラグインがインストールされていること — SDK の README にあるセットアップ手順に従ってください（[Claude Code のセクション](https://github.com/ServiceNow/sdk#claude-code) に書かれているプラグインのインストール手順は、Claude Code 限定ではなく、どのコーディングエージェントにも当てはまります。Cursor や Windsurf などを使う場合は、README 内のそれぞれのエージェント向けセクションも参照してください）

## Exercise Steps（演習の手順）

1. Fluent プロジェクトのルート（ビルド＋テスト演習で使用したものと同じ）でターミナルを開き、コーディングエージェントとのセッションを開始します。
2. 何か新しいものを追加するのではなく、既存の動作を変更するようプロンプトを与えます：
   ```
   Change default status to be draft on new maintenance request.
   ```
3. エージェントが Maintenance Request のビジネスルールの現在のデフォルト値（「New」）を見つけ、それを「Draft」に更新し、ルールのコメントもそれに合わせて更新することを確認します。
4. これは意図的な破壊的変更です：デフォルトステータスが「New」であることを前提とする既存の ATF テスト（Exercise 03 で作られた positive-case テストや、状態を確認する ACL／制約系のテストなど）が失敗するようになります。この変更によって失敗するようになった ATF テストがあるかどうかをコーディングエージェントに確認させ、修正させてみましょう：
   ```
   Does this change break any existing ATF tests? If so, fix them.
   ```
   失敗したテストのアサーションとコメントが、「New」ではなく「Draft」を期待する内容に更新されることを確認してください — これは、Test Agent がインスタンス上で行っているのと同じ自動修復の挙動が、プラットフォームの外でも機能するということです。
5. ビルドしてインストールします：
   ```
   now-sdk build && now-sdk install
   ```
6. サンドボックスを開き、新しい Maintenance Request のデフォルトが「Draft」になっていることを確認し、Studio 上で該当する ATF テストを再実行して、再び成功することを確認します。

## Success Criteria（達成基準）

- [ ] コーディングエージェント向けに AI Skills プラグインがインストールされている
- [ ] 新しい Maintenance Request のデフォルトステータスが「Draft」になっている
- [ ] デフォルトが「New」であることを前提としていた ATF テストが更新され、再び成功している
- [ ] `now-sdk build && now-sdk install` がエラーなく完了した

## Learning Points（学びのポイント）

- AI Skills プラグインは、インスタンス上で Build Agent が持っているのと同じ、根拠に基づいた最新の Fluent／SDK の知識をコーディングエージェントに与えます — プラットフォームの外に出てもワークフローは変わらず、変わるのはツールだけです。
- `now-sdk build`／`now-sdk install` は、Build Agent が生成したコードでもコーディングエージェントが生成したコードでも同じコマンドです — SDK は誰（または何）がソースを書いたかを気にしません。
- 自動修復ループは Build Agent／Test Agent だけの特殊な仕組みではありません — 同じ根拠に基づいた知識を持つコーディングエージェントであれば、自分が壊したテストを検知して修正することが、プラットフォームの外でも同じように行えます。

## Bonus Challenge（ボーナス課題）

- スキーマも拡張してみましょう — 新しいテーブルと、既存テーブルへのカラム追加：
  ```
  Create a new ServiceNow table called "Todo List" that can be used to
  group records from the existing Maintenance Request table. Also, add
  a "Due Date" date/time column to the existing Maintenance Request
  table.
  ```
  その後、動作を確認できるよう、デモデータを生成するよう依頼します：
  ```
  Add some demo Todo Lists and Todo records for them so I can see them
  in action.
  ```
- このアプリにさらに追加すべきものがあるか、コーディングエージェントに尋ねてみましょう
- リストとその Todo を可視化するダッシュボードを追加するようプロンプトしてみましょう
