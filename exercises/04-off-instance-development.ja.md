---
title: "オフインスタンス開発：Fluent 向け AI Skills"
slide: "21 — オフインスタンス開発"
---

# オフインスタンス開発：Fluent 向け AI Skills

[← デッキのスライド21に戻る](https://servicenow.github.io/servicenowsdlc/index.ja.html#21) ｜ [English version](https://github.com/ServiceNow/servicenowsdlc/blob/main/exercises/04-off-instance-development.md)

## Objective（目的）

ServiceNow SDK の AI Skills プラグインをコーディングエージェント（Claude Code、Cursor、Windsurf など）と組み合わせて使い、プラットフォームの外でメンテナンスアプリを拡張し、Studio の外でも同じ `now-sdk build`／`now-sdk install` のループが機能することを確認します。

## Estimated Time（想定時間）

15〜20分

## Prerequisites（前提条件）

- ビルド＋テスト演習で使用したサンドボックスと Fluent プロジェクト
- ローカルにインストールされたコーディングエージェント（Claude Code、Cursor、Windsurf など）
- 使用するコーディングエージェント向けに ServiceNow SDK の AI Skills プラグインがインストールされていること — SDK の README にあるセットアップ手順に従ってください（[Claude Code のセクション](https://github.com/ServiceNow/sdk#claude-code) に書かれているプラグインのインストール手順は、Claude Code 限定ではなく、どのコーディングエージェントにも当てはまります。Cursor や Windsurf などを使う場合は、README 内のそれぞれのエージェント向けセクションも参照してください）

## Exercise Steps（演習の手順）

1. Fluent プロジェクトのルート（ビルド＋テスト演習で使用したものと同じ）でターミナルを開き、コーディングエージェントとのセッションを開始します。
2. アプリを拡張するようプロンプトを与えます：
   ```
   Create a new ServiceNow table called "Todo List" that can be used to
   group records from the existing Maintenance Request table. Also, add
   a "Due Date" date/time column to the existing Maintenance Request
   table.
   ```
3. エージェントがそのテーブル用の新しい `.now.ts` ファイル（おそらく `src/fluent/` 配下）を作成し、既存テーブルのコードにカラムを追加したことを確認します。
4. 動作を確認できるよう、デモデータを生成するよう依頼します：
   ```
   Add some demo Todo Lists and Todo records for them so I can see them
   in action.
   ```
5. データモデルだけでなく、ビジネスロジックの変更もプロンプトしてみます：
   ```
   Change default status to be draft on new maintenance request.
   ```
6. ビルドしてインストールします：
   ```
   now-sdk build && now-sdk install
   ```
7. サンドボックスを開き、新しいテーブル、カラム、デモデータ、そしてデフォルトステータスの変更が反映されていることを確認します。

## Success Criteria（達成基準）

- [ ] コーディングエージェント向けに AI Skills プラグインがインストールされている
- [ ] 新しい Todo List テーブルと Due Date カラムが Fluent のソースとして存在する
- [ ] デモデータが生成され、サンドボックス内で確認できる
- [ ] 新しい Maintenance Request のデフォルトステータスが「Draft」になっている
- [ ] `now-sdk build && now-sdk install` がエラーなく完了した

## Learning Points（学びのポイント）

- AI Skills プラグインは、インスタンス上で Build Agent が持っているのと同じ、根拠に基づいた最新の Fluent／SDK の知識をコーディングエージェントに与えます — プラットフォームの外に出てもワークフローは変わらず、変わるのはツールだけです。
- `now-sdk build`／`now-sdk install` は、Build Agent が生成したコードでもコーディングエージェントが生成したコードでも同じコマンドです — SDK は誰（または何）がソースを書いたかを気にしません。

## Bonus Challenge（ボーナス課題）

- このアプリにさらに追加すべきものがあるか、コーディングエージェントに尋ねてみましょう
- リストとその Todo を可視化するダッシュボードを追加するようプロンプトしてみましょう
