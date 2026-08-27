---
title: "nowSDK を使った Fluent アプリケーションの作成"
slide: "8 — ソース管理の準備"
---

# nowSDK を使った Fluent アプリケーションの作成

[← デッキのスライド8に戻る](https://servicenow.github.io/servicenowsdlc/index.ja.html#8) ｜ [English version](https://github.com/ServiceNow/servicenowsdlc/blob/main/exercises/01-creating-a-fluent-app.md)

## Objective（目的）

nowSDK を使って新しい Fluent アプリケーションを作成し、ソース管理に接続します — これは、本日のこれ以降すべてが前提とする2つの基盤ツールです。

## Estimated Time（想定時間）

20〜25分

## Prerequisites（前提条件）

- [ ] インスタンスの認証情報に登録し、サンドボックスが割り当てられていること（未実施の場合は、以下の Setup Instructions の手順1〜3で対応します）
- [ ] Node のバージョンが 20.18.0 以上であることを確認してください（`node -v`）
- [ ] Node が 20.18.0 未満の場合は、`nvm install 20.18.0` を実行してください
- [ ] ワークショップの org/repo にアクセスできる GitHub アカウント（「Get your source control ready」スライドを参照）

## Setup Instructions（セットアップ手順）

1. [インスタンス登録シート](https://servicenow-my.sharepoint.com/:x:/p/shelby_cohen/IQDPcQEo1cNpT7XNsBeEi9J_AYPIviiz8XC2P7KjYkhbBhg) でインスタンスの認証情報に登録してください。
2. その認証情報で、ベースインスタンス [https://empsdlcdev.service-now.com/](https://empsdlcdev.service-now.com/) にログインします。
3. 自分の名前を付けたサンドボックスを割り当てます：
    - [Sandbox Management Home](https://empsdlcdev.service-now.com/now/developer-sandbox/home) に直接アクセスするか、トップナビゲーションで `sandbox` を検索して **Sandbox Management Home** を開きます。

      ![Sandbox Management Home への移動](images/sandbox-setup/01-navigate-to-sandbox-management.png)
    - **Allocate sandbox** をクリックします。

      ![サンドボックスの割り当て](images/sandbox-setup/02-allocate-sandbox.png)
    - **Allocate Sandbox** ダイアログでは、**Sandbox template** は空のままにしてください。**Sandbox alias** には、この演習の他の箇所と同じ `<your_name>` の命名規則で名前を付けます（最大8文字、一意である必要があります）。これにより、他の参加者のものと衝突しなくなります。入力したら **Allocate** をクリックします。

      ![Sandbox template を空にした Allocate Sandbox ダイアログ](images/sandbox-setup/03-allocate-sandbox-details.png)
    - プロビジョニングが完了するまで待ちます — このサンドボックスの URL が、この演習の残りで使う `<instance url>` になります。
4. ターミナルウィンドウを開きます
    - IDE（WindSurf など）で既に開いているターミナルを使うか、新しいターミナルウィンドウを開いてください
    - 演習の手順は WindSurf のターミナルを想定していますが、他のターミナルでも同様に実行できます。
5. ServiceNow SDK をインストールします： `npm i -g @servicenow/sdk`
6. `now-sdk --version` を実行し、インストールされたバージョンが `4.6.0` 以上であることを確認してください
7. VSCode を使う場合は、[こちら](https://marketplace.visualstudio.com/items?itemName=ServiceNow.fluent-language-extension) から Fluent Language 拡張機能をインストールしてください。
8. Windsurf など他の VSCode フォークを使う場合は、[こちら](https://open-vsx.org/extension/ServiceNow/fluent-language-extension) からインストールしてください
9. `now auth --add <instance url> --type basic` を使って、サンドボックス用の認証プロファイルを設定します

> **命名規則：** 以降 `<your_name>` という表記が出てきますが、これは自分のファーストネーム（小文字、スペースなし）に置き換えてください（例：`shelby`）。これは共有インスタンスのため、一意な app/scope 名にすることが、他の参加者のアプリと衝突しないためのポイントです。

## Exercise Steps（演習の手順）

### Step 1: Fluent アプリケーションの初期化

1. 新しい空のディレクトリを作成し、`bootcamp-demo-<your_name>` という名前にします
2. そのディレクトリに移動します： `cd bootcamp-demo-<your_name>`
3. `now-sdk init` を実行して、新しい Fluent（ServiceNow SDK）アプリケーションを作成します
4. キーボードの矢印キーを使って、`-- TypeScript --` セクションの `now-sdk + basic` を選択します：
   ```txt
    ? Select a template:
     -- Basic --
      now-sdk boilerplate
     -- JavaScript --
      now-sdk + basic
      now-sdk + fullstack React
     -- TypeScript --
    ❯ now-sdk + basic
      now-sdk + fullstack React
      now-sdk + fullstack Vue
    A basic application using NowSDK and TypeScript
    ```
5. `Name of ServiceNow Application:` には `My First Fluent App - <Your Name>` と入力します（例：`My First Fluent App - Shelby`）
6. `NPM package name:` には `my-first-fluent-app-<your_name>` と入力します
7. `Create a Global/Scoped App?` では `Scoped` を選択します
8. `Scope name:` には `sn_my_first_fluent_<your_name>` と入力します
9. `npm install` を実行して、新しく作成した Fluent アプリの依存関係をインストールします（Fluent アプリは基本的に NPM パッケージなので、Node/NPM エコシステムの周辺ツールを利用できます）

この時点で、SDK は [JavaScript サーバーサイドモジュール](https://www.servicenow.com/docs/r/washingtondc/application-development/scripts/c_JS_modes.html) の言語として TypeScript を使ったサンプルの Fluent プロジェクトをスキャフォールドしています。デフォルトでは、JavaScript サーバーサイドモジュールは `src/server` 内に定義されます。

プロジェクトの構成は次のようになっているはずです：
```txt
├── now.config.json <-- スコープ名、スコープ ID、スコープの sys_id（GUID）が定義されている場所。この NPM パッケージを ServiceNow SDK（Fluent）プロジェクトにしているファイルです
├── package-lock.json <-- 標準的な NPM の package-lock.json ファイル
├── package.json <-- パッケージに関する属性や依存関係の一覧が定義された、標準的な NPM の package.json
└── src <---デフォルトのソースコードディレクトリ
    ├── fluent <-- Fluent ファイル用のサブディレクトリ
    │   └── example.now.ts <-- サンプルの Fluent ファイル。`.now.ts` 拡張子に注目してください
    ├── server <-- サーバーサイドモジュール用ディレクトリ
    │   ├── script.ts <-- サンプルの TS サーバーモジュール
    │   └── tsconfig.json <-- サーバーモジュール用の tsconfig.json
    ├── tsconfig.client.json <-- クライアントコード用の tsconfig.json。現時点ではこのプロジェクトに src/client はありません
    ├── tsconfig.json <-- ベースの tsconfig.json
    └── tsconfig.server.json <-- サーバーモジュール以外のサーバーサイドコード（Business Rule のスクリプトや Script Include のスクリプトなど）用の tsconfig.json
```
10. `fluent` ディレクトリから `example.now.ts` ファイルを削除します。

### Step 2: Git のセットアップ

Update Set ではなく Git が、このアプリの今後にわたる信頼できる唯一の情報源（source of truth）になります（「Git is the authoritative source of truth」スライドを参照）。メタデータを書く前に、まずこのプロジェクトを Git に接続しましょう：

1. プロジェクトのルートでリポジトリを初期化します：
   ```
   git init
   git add .
   git commit -m "Initial commit: bootcamp-demo-<your_name> scaffold"
   ```
2. ワークショップの GitHub org 配下に、**空の**リポジトリを新規作成し（README／license は不要です。すでにローカルにファイルがあるため）、ローカルリポジトリをそこに向けてプッシュします：
   ```
   git remote add origin <your new repo's URL>
   git branch -M main
   git push -u origin main
   ```
3. 「Get your source control ready」スライドで生成したパーソナルアクセストークンを使って、ServiceNow の dev インスタンスのソース管理設定を、この同じリポジトリに接続します。具体的なクリック手順は [Exercise 05 — Source control](https://github.com/ServiceNow/servicenowsdlc/blob/main/exercises/05-source-control.md) で詳しく扱います。ここでは、接続が成功したことを確認したら次に進んでください。

### Step 3: アプリケーションのビルドとインストール

1. `now-sdk build` - アプリケーションをビルドします
2. `now-sdk install` - アプリケーションを ServiceNow にデプロイします。
3. ヒント：`package.json` にカスタムコマンドを追加すると、これを簡単に実行できます。
`"build-deploy": "now-sdk build && now-sdk install"` と追加し、`npm run build-deploy` を実行します。
4. 今ビルドしたものをコミット＆プッシュします：
   ```
   git add .
   git commit -m "Initial build"
   git push
   ```

### Step 4: ServiceNow での確認

1. ServiceNow インスタンスに移動し、アプリが正しくインストールされたことを確認します（App Manager または Studio で、自分のスコープに、ビルドしたバージョンが表示されているはずです）。

## Success Criteria（達成基準）

- [ ] 一意な名前付きの app/scope で Fluent アプリケーションが初期化されている（他の参加者と衝突していない）
- [ ] ローカルリポジトリが初期化され、ワークショップの GitHub org にプッシュされ、dev インスタンスのソース管理設定に接続されている
- [ ] アプリケーションを ServiceNow にビルド＆インストールできている

## Learning Points（学びのポイント）

- SDK は、CLI ウィザードから実際の Fluent プロジェクトをスキャフォールドします — 型があり、診断可能なソースであり、中身を確認したり diff を取ったりできないクリックのブラックボックスではありません。
- このアプリをこの先も再現可能にしているのは、アプリそのものではなく Git です — ここで接続しておくことで、この後のすべての演習（ビルド、オフインスタンス開発、ソース管理、ReleaseOps）に、実際に積み上げていける土台ができます。
- 共有インスタンスにおいて、一意な app/scope 名は見た目だけの問題ではありません — それが「自分のアプリ」であるか、「最後にプッシュした人のアプリ」になってしまうかの分かれ目です。

## Bonus Challenge（ボーナス課題）

- 自分自身のテーブルとビジネスルールを追加してみましょう（本日のビルド演習と同じパターンの Fluent `Table()` と `BusinessRule()`）。追加したら再度ビルド＆インストールしてください
- 自分のリポジトリに対して、小さな変更を加えたプルリクエストを開いてみましょう。Exercise 05 で本番の流れを体験する前に、レビューの流れを一度見ておくためです
