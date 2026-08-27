---
title: "ソース管理：Git でリリースする"
slide: "20 — ソース管理演習"
---

# ソース管理：Git でリリースする

[← デッキのスライド20に戻る](https://servicenow.github.io/servicenowsdlc/index.ja.html#20) ｜ [English version](https://github.com/ServiceNow/servicenowsdlc/blob/main/exercises/05-source-control.md)

## Objective（目的）

push、PR、レビュー、マージ、公開、pull という Git ベースのコラボレーションフローを一通り体験し、Update Set がその代わりになれない理由を実際に確認します。

## Estimated Time（想定時間）

30分

## Prerequisites（前提条件）

- 演習03でコミットした作業が入ったフィーチャーブランチ

## Exercise Steps（演習の手順）

1. **オフインスタンス（GitHub）：** フィーチャーブランチをコミットして push します。
   ```
   git add .
   git commit -m "Add maintenance request app"
   git push -u origin feature/maintenance-app
   ```
   自分で入力したくない場合は、Claude Code（または利用中のコーディングエージェント）にブランチのコミットと push を依頼してください。
2. プルリクエストを開きます。別のチームとペアになり、レビューしてもらいましょう — 少なくとも1つコメントを残してもらってください。
3. フィードバックに対応し、`main` にマージします。
4. ベースの開発用インスタンスから、アプリのプレリリースを Application Repository に公開します。
5. **オンインスタンス：** 2つ目のインスタンスで Studio/IDE（SNS）を開き、git を使ってマージ済みの変更を pull します。
6. pull したアプリに、マージした変更が反映されていることを確認します。

## Success Criteria（達成基準）

- [ ] レビュー履歴が確認できる形でマージされた PR
- [ ] Application Repository に公開されたプレリリース
- [ ] 2つ目のインスタンスで pull され、確認できる変更

## Learning Points（学びのポイント）

- レビューとコンフリクト解決は、Git がネイティブに提供するコラボレーションの基本要素です — Update Set にはこれに相当するものがなく、last-write-wins しかありません。
- 2つ目のインスタンスへのインストールを再現可能かつ追跡可能にしているのは、Update Set ではなく App Repo です。
- 先ほど公開したプレリリースは、まさに次に ReleaseOps が受け取るものです — 演習06は、その同じ Application Repository のアーティファクトから作られた Update Set を Deployment Request にプロモートするところから始まります。

## Bonus Challenge（ボーナス課題）

- レビューのパートナーと意図的にマージコンフリクトを発生させ（同じフィールドを両方で編集）、行単位で解決してみましょう
- 議論してみましょう：同じコンフリクトが2つの Update Set で発生した場合、どうなっていたでしょうか？
