---
title: "nowSDK を使った Fluent アプリケーションの作成"
slide: "10 — ソース管理の準備"
---

# nowSDK を使った Fluent アプリケーションの作成

[← デッキのスライド10に戻る](https://servicenow.github.io/servicenowsdlc/index.ja.html#10) ｜ [English version](https://github.com/ServiceNow/servicenowsdlc/blob/main/exercises/01-creating-a-fluent-app.md)

## Objective（目的）

nowSDK を使って新しい Fluent アプリケーションを作成し、ソース管理に接続します — これは、本日のこれ以降すべてが前提とする2つの基盤ツールです。これは今日一日をかけて作り込んでいく、まさにそのアプリです — Exercise 03 で Build Agent がこれを拡張し、以降の演習でプッシュ・リリース・デプロイされるのもこのアプリです。

## Estimated Time（想定時間）

20〜25分

## Prerequisites（前提条件）

- [ ] インスタンスの認証情報に登録し、サンドボックスが割り当てられていること（未実施の場合は、以下の Setup Instructions の手順1〜3で対応します）
- [ ] Node のバージョンが 20.18.0 以上であることを確認してください（`node -v`）
- [ ] Node が 20.18.0 未満の場合は、`nvm install 20.18.0` を実行してください
- [ ] 個人の GitHub.com アカウント

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

> **命名規則：** 以降 `<your_name>` という表記が出てきますが、これは自分のファーストネーム（小文字、スペースなし）に置き換えてください（例：`shelby`）。これは共有インスタンスのため、一意な app/scope 名にすることが、他の参加者のアプリと衝突しないためのポイントです。ファーストネームが他の参加者と重複する場合や、長すぎて下記のスコープ名の18文字制限を超えてしまう場合は、代わりに ServiceNow の userid を使ってください。

## Exercise Steps（演習の手順）

### Step 1: Fluent アプリケーションの初期化

1. ターミナルウィンドウを開きます
    - IDE（WindSurf など）で既に開いているターミナルを使うか、新しいターミナルウィンドウを開いてください
    - 演習の手順は WindSurf のターミナルを想定していますが、他のターミナルでも同様に実行できます。
2. ServiceNow SDK をインストールします： `npm i -g @servicenow/sdk`
3. `now-sdk --version` を実行し、インストールされたバージョンが `4.6.0` 以上であることを確認してください
4. VSCode を使う場合は、[こちら](https://marketplace.visualstudio.com/items?itemName=ServiceNow.fluent-language-extension) から Fluent Language 拡張機能をインストールしてください。
5. Windsurf など他の VSCode フォークを使う場合は、[こちら](https://open-vsx.org/extension/ServiceNow/fluent-language-extension) からインストールしてください
6. サンドボックス用の認証プロファイルを設定します： `now-sdk auth --add <instance url> --type basic`
    - `<instance url>` は、上記手順3のサンドボックス URL です（例：`https://<sandboxalias>.empsdlcdev.service-now.com`）— ベースインスタンスの URL ではありません。
    - インスタンスへのログインに使ったのと同じユーザー名／パスワードを使用してください。
    - 続行する前に、認証が成功したことを示すメッセージを確認してください。失敗する場合は、ベースインスタンスではなくサンドボックスの URL を指定しているか再確認してください。
7. 新しい空のディレクトリを作成し、`sdlc-workshop-<your_name>` という名前にします
8. そのディレクトリに移動します： `cd sdlc-workshop-<your_name>`
9. `now-sdk init` を実行して、新しい Fluent（ServiceNow SDK）アプリケーションを作成します
10. キーボードの矢印キーを使って、`-- TypeScript --` セクションの `now-sdk + basic` を選択します：
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
11. `Name of ServiceNow Application:` は自分で入力する必要があります — デフォルトはありません。`Maintenance_<YourName>`（例：`Maintenance_Shelby`）と入力してください。これは Exercise 03 で Build Agent が拡張する、同じアプリ名です。ここで入力した通りに、そのまま使ってください。
12. `NPM package name:` は提案されたデフォルト（アプリ名から自動生成されたもの）をそのまま使用してください。
13. `Create a Global/Scoped App?` では `Scoped` を選択します
14. `Scope name:` は提案されたデフォルトをそのまま使用してください。自分で入力するよう求められた場合は `x_snc_<your_name>` を使用してください — ServiceNow のスコープ名には18文字の制限があるため、`<your_name>` は短くしてください。
15. `npm install` を実行して、新しく作成した Fluent アプリの依存関係をインストールします（Fluent アプリは基本的に NPM パッケージなので、Node/NPM エコシステムの周辺ツールを利用できます）

この時点で、SDK は [JavaScript サーバーサイドモジュール](https://www.servicenow.com/docs/r/washingtondc/application-development/scripts/c_JS_modes.html) の言語として TypeScript を使ったサンプルの Fluent プロジェクトをスキャフォールドしています。デフォルトでは、JavaScript サーバーサイドモジュールは `src/server` 内に定義されます。

プロジェクトの構成は次のようになっているはずです：
```txt
├── now.config.json <-- スコープ名、スコープ ID、スコープの sys_id（GUID）が定義されている場所。この NPM パッケージを ServiceNow SDK（Fluent）プロジェクトにしているファイルです
├── package-lock.json <-- 標準的な NPM の package-lock.json ファイル
├── package.json <-- パッケージに関する属性や依存関係の一覧が定義された、標準的な NPM の package.json
└── src <---デフォルトのソースコードディレクトリ
    ├── fluent <-- Fluent ファイル用のサブディレクトリ
    ├── server <-- サーバーサイドモジュール用ディレクトリ
    │   ├── script.ts <-- サンプルの TS サーバーモジュール
    │   └── tsconfig.json <-- サーバーモジュール用の tsconfig.json
    ├── tsconfig.client.json <-- クライアントコード用の tsconfig.json。現時点ではこのプロジェクトに src/client はありません
    ├── tsconfig.json <-- ベースの tsconfig.json
    └── tsconfig.server.json <-- サーバーモジュール以外のサーバーサイドコード（Business Rule のスクリプトや Script Include のスクリプトなど）用の tsconfig.json
```

### Step 2: Git のセットアップ

Update Set ではなく Git が、このアプリの今後にわたる信頼できる唯一の情報源（source of truth）になります（「Git is the authoritative source of truth」スライドを参照）。メタデータを書く前に、まずこのプロジェクトを Git に接続しましょう：

1. ローカルの Git ID を、自分の GitHub.com の個人アカウントに設定します（すでにマシンのグローバルデフォルトになっている場合はスキップしてください）：
   ```
   git config user.name "<your GitHub username>"
   git config user.email "<your GitHub-registered email>"
   ```
2. プロジェクトのルートでリポジトリを初期化します：
   ```
   git init
   git add .
   git commit -m "Initial commit: sdlc-workshop-<your_name> scaffold"
   ```
3. **空の**リポジトリを新規作成します（README、`.gitignore`、license は不要です。すでにローカルにファイルがあるため）：
    - [github.com/new](https://github.com/new) にアクセスします。
    - Owner とリポジトリ名を設定し、visibility・README・`.gitignore`・license は下の画像の通りにしたまま、**Create repository** をクリックします。

      ![github.com/new でリポジトリを作成。Add README はオフ、.gitignore もライセンスもなし](images/github-setup/create_new_repo.png)
    - ローカルリポジトリを、作成した空のリポジトリに向けてプッシュします：
      ```
      git remote add origin <your new repo's URL>
      git branch -M main
      git push -u origin main
      ```
4. パーソナルアクセストークンを生成します — これが、ServiceNow がリポジトリに対して認証するために使うものです：
    - [github.com/settings/tokens](https://github.com/settings/tokens)（Settings → Developer settings → Personal access tokens → Tokens (classic)）にアクセスします。
    - **Generate new token** → **Generate new token (classic)** をクリックします。
    - 名前を付け、**Select scopes** で最上位の **repo** チェックボックスをオンにします — これで配下のすべてのサブスコープも自動的にオンになります。

      ![Generate new token (classic) ページ。Select scopes セクションと repo チェックボックス](images/github-setup/classic_token_scopes.png)

      ![repo チェックボックスがオンになり、配下のサブスコープもすべて自動的にオンになった状態](images/github-setup/repo_scopes_checked.png)
    - 下にスクロールして **Generate token** をクリックします。

      ![Generate token ボタン](images/github-setup/generate_token.png)
    - **今すぐトークンをコピーしてください。** 次の手順で必要になりますが、このページを離れると GitHub は二度とトークンを表示してくれません — 失くした場合は新しく生成し直す必要があります。
5. サンドボックスをこのリポジトリに接続します：
    - サンドボックスインスタンス（Setup Instructions の手順3で取得した `<instance url>`）で ServiceNow IDE を開きます。まだ何も開いていない場合、Explorer パネルに **Create an app**・**Open Apps** と並んで **Clone Git repository** が表示されるので、それをクリックします。

      ![ServiceNow IDE の Explorer パネル。Clone Git repository のオプションが表示されている](images/github-setup/clone_git_repo.png)
    - 手順3で作成したリポジトリの URL を入力し、Enter を押します。

      ![クローンするリポジトリの URL を入力](images/github-setup/enter_url.png)
    - 画面右下に、Git の認証情報を設定するよう求めるメッセージが表示されます。**Configure** をクリックし、GitHub のユーザー名と、パスワード欄には手順4のパーソナルアクセストークンを入力してください — GitHub はもはやここでアカウントパスワードを受け付けません。
    - クローンが正常に完了したことを確認してください。認証エラーで失敗する場合は、トークンを再生成し、`repo` スコープが付与されていることを確認してください。
6. インスタンス側の接続を Sync します：Studio で **Sync** を実行してください（`now-sdk install` ではありません — Sync はインスタンスからローカルソースへ取り込む方向で、install とは逆方向です）。これは、IDE を使わずインスタンスの UI で直接変更を加えた場合にも使う操作です。
    - Sync は手順5で設定した認証情報に依存しています。フェッチ／ダウンロードエラーで失敗する場合、多くは Git の問題ではなくトークンやスコープの権限の問題です。

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
- [ ] ローカルリポジトリが初期化され、新しく作成した空の GitHub リポジトリにプッシュされ、dev インスタンスの IDE にクローンされている
- [ ] アプリケーションを ServiceNow にビルド＆インストールできている

## Learning Points（学びのポイント）

- SDK は、CLI ウィザードから実際の Fluent プロジェクトをスキャフォールドします — 型があり、診断可能なソースであり、中身を確認したり diff を取ったりできないクリックのブラックボックスではありません。
- このアプリをこの先も再現可能にしているのは、アプリそのものではなく Git です — ここで接続しておくことで、この後のすべての演習（ビルド、オフインスタンス開発、ソース管理、ReleaseOps）に、実際に積み上げていける土台ができます。
- 共有インスタンスにおいて、一意な app/scope 名は見た目だけの問題ではありません — それが「自分のアプリ」であるか、「最後にプッシュした人のアプリ」になってしまうかの分かれ目です。

## Bonus Challenge（ボーナス課題）

- 自分自身のテーブルとビジネスルールを追加してみましょう（本日のビルド演習と同じパターンの Fluent `Table()` と `BusinessRule()`）。追加したら再度ビルド＆インストールしてください
- 自分のリポジトリに対して、小さな変更を加えたプルリクエストを開いてみましょう。Exercise 05 で本番の流れを体験する前に、レビューの流れを一度見ておくためです
