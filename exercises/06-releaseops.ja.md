---
title: "ReleaseOps：アセスとリリース"
slide: "22 — ReleaseOps"
---

# ReleaseOps：アセスとリリース

[← デッキのスライド22に戻る](https://servicenow.github.io/servicenowsdlc/index.ja.html#22) ｜ [English version](https://github.com/ServiceNow/servicenowsdlc/blob/main/exercises/06-releaseops.md)

## Objective（目的）

演習05で公開したアプリを、ReleaseOps の品質管理パイプラインに通します — Deployment Request を作成し、アセスメントの実行を確認し、Ready for Deploy まで到達させます。**この演習では本番環境へのプッシュは行いません** — その部分はこの後のライブデモで扱う内容であり、皆さん自身で行う作業ではありません。

## Estimated Time（想定時間）

30分

## Prerequisites（前提条件）

- Application Repository に公開されたプレリリース（演習05）
- 開発用インスタンスとは別のテスト用インスタンスへのアクセス

## Exercise Steps（演習の手順）

1. 開発用インスタンス上で、Update Set を新しい Deployment Request にプロモートします。
2. Deployment Request を「Ready to Assess」にマークします。
3. アセスメントの手順が自動的に実行される様子を確認します：Instance Scan（開発用インスタンス上）→ Move to Test → Run ATF。
4. ATF が失敗した場合は、3つの対応オプションのうち1つを選び、それぞれが実際に何をするのか理解しましょう：
   - **Retest** — そのままアセスメントを再実行する。
   - **Need Code Change** — アセスメントを無効化する。Deployment Request は Draft に戻り、再アセスメントの前に新しいペイロードが必要になる。
   - **Sign Off** — 失敗があっても手動承認で先に進める。
5. Deployment Request が「Ready for Deploy」に到達したら、**そこで止めてください。** それがこの演習のゴールです — Release レコードを作成したり、本番環境へ何かをプッシュしたりする必要はありません。
6. 実際に本番環境へリリースする作業（On Demand か Scheduled か）は、この直後にライブデモとして扱います — 次のスライドを参照してください。見学するだけで、自分のインスタンスで再現しないでください。

## Success Criteria（達成基準）

- [ ] Deployment Request が作成され、Instance Scan、Move to Test、Run ATF を経て進んだ
- [ ] Retest、Need Code Change、Sign Off の違いを説明できる
- [ ] Deployment Request が「Ready for Deploy」に到達した — ここで演習は終了で、リリース／本番へのプッシュは不要

## Learning Points（学びのポイント）

- Deployment Request はアセスメントの対象となるペイロードのコンテナであり、Release はクリアされた Deployment Request を実際に本番環境へ移す別のレコードです。
- 「Need Code Change」は軽い警告ではありません — アセスメント全体を無効化し、Draft に戻してしまいます。実際に遭遇する前に理解しておく価値があります。
- ロールバックは、アプリが Application Repository からインストールされていた場合にのみ、かつ決められた期間内でのみ機能します — これも、手動インストールではなく App Repo が本番環境へのサポートされた経路である理由の一つです。

## Bonus Challenge（ボーナス課題）

- アセスメント前に意図的に ATF テストを失敗させ、「Need Code Change」の経路をたどってみましょう — ペイロードを再構築して再アセスメントします
- On Demand と Scheduled のリリースを比較し、チームのペースにどちらが合うか議論してみましょう
