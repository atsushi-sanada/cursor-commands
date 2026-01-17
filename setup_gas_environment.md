# GAS 開発環境構築プロンプト

Google Apps Script（GAS）を JavaScript で IDE 開発するための作業環境を自動構築します。clasp のセットアップ、プロジェクト構造の作成、BAT ファイルの準備まで自動化します。

## 実行ルール

### ファイル操作

- 既存ファイルが存在する場合は上書き前に確認を求める
- 既存フォルダ構造は保持し、不足分のみ追加する
- 中断時は完了したステップまでを保存する

### 前提条件

- Node.js と npm がインストールされていること
- Google アカウントにアクセスできること
- Google Apps Script API が有効化されていること（手順を案内）

### プロジェクトパス

- このプロンプトを実行したフォルダ（カレントディレクトリ）に環境を構築する

## 1. 環境確認フェーズ

- Node.js/npm のバージョンを確認する
  - `node --version`と`npm --version`を実行してバージョンを確認
  - 未インストールの場合はエラーメッセージを表示し、[Node.js 公式サイト](https://nodejs.org/)からインストールするよう案内
- clasp のインストール状況を確認する
  - `clasp --version`を実行してインストール状況を確認
  - 未インストールの場合はグローバルインストール: `npm install -g @google/clasp`
- Google Apps Script API の有効化確認
  - 未有効化の場合は、[Google Apps Script API 設定ページ](https://script.google.com/home/usersettings)で API を有効化するよう案内する
  - 有効化手順を説明する

## 2. プロジェクト初期化フェーズ

- `package.json`の作成（存在しない場合）
  - `npm init -y`を実行して初期化
  - `@google/clasp`を devDependencies に追加: `npm install --save-dev @google/clasp`
  - 既に存在する場合は内容を確認し、`@google/clasp`が含まれていない場合のみ追加
- `.gitignore`の作成（存在しない場合）
  - 以下の内容で作成：
    ```
    node_modules/
    .clasp.json
    ```
  - 既に存在する場合は内容を確認し、不足している項目のみ追加

## 3. clasp 設定フェーズ

- clasp ログイン確認
  - `clasp login --status`を実行してログイン状況を確認
  - 未ログインの場合は`clasp login`を実行する
  - ログイン後、ブラウザが開くので認証を完了するよう案内
- GAS プロジェクトの作成または既存プロジェクトへの接続
  - `.clasp.json`が存在しない場合：
    - カレントディレクトリのフォルダ名を取得してプロジェクト名として使用
    - 新規プロジェクトを作成: `clasp create --title "プロジェクト名" --type standalone --rootDir src`
    - 作成後、`.clasp.json`が生成されることを確認
  - `.clasp.json`が存在する場合：
    - 既存の`.clasp.json`を読み込んで内容を確認
- `.clasp.json`の設定確認と更新
  - 以下の設定が含まれていることを確認：
    ```json
    {
      "scriptId": "GASのスクリプトID",
      "rootDir": "src",
      "scriptExtensions": [".gs"],
      "htmlExtensions": [".html"],
      "jsonExtensions": [".json"],
      "filePushOrder": [],
      "skipSubdirectories": false,
      "runtimeVersion": "V8"
    }
    ```
  - **重要**: `scriptExtensions`には`.gs`のみを含める（`.js`は含めない）
    - `.clasp.json`の`scriptExtensions`に`.js`が含まれている場合は削除し、`.gs`のみにする
    - `.js`ファイルが GAS にアップロードされると不具合が発生するため
  - `rootDir`が`src`に設定されているか確認し、異なる場合は更新
  - `runtimeVersion`が未設定の場合は`"V8"`を追加する
  - その他の設定（htmlExtensions、jsonExtensions、filePushOrder、skipSubdirectories）が適切か確認
  - **注意**: scriptId の設定は「7. スクリプト ID 反映フェーズ」で行うため、ここでは確認のみ

## 4. プロジェクト構造作成フェーズ

- `src/`フォルダの作成（存在しない場合）
  - `mkdir src`を実行（Windows の場合は`if not exist src mkdir src`）
  - 既に存在する場合はそのまま使用
- `src/appsscript.json`の作成（存在しない場合）
  - 以下の内容で作成：
    ```json
    {
      "timeZone": "Asia/Tokyo",
      "dependencies": {},
      "exceptionLogging": "STACKDRIVER",
      "runtimeVersion": "V8"
    }
    ```
  - 既に存在する場合は内容を確認し、不足している設定のみ追加
- `.claspignore`の作成（存在しない場合）
  - 以下の内容で作成：

    ```
    # node_modulesを除外
    node_modules/

    # ルートディレクトリの不要なファイルを除外
    *.bat
    *.md
    package*.json

    # .git関連を除外
    .git/
    .gitignore
    ```

  - **重要**: `**/**`で全てを除外する方式は使用しない
    - `clasp`が`src`配下のファイルを正しく認識できない場合があるため
    - 除外したいファイル/ディレクトリを明示的に指定する方式を推奨
  - 既に存在する場合は内容を確認し、不足している項目のみ追加
- 基本的なサンプルコードの作成（オプション、存在しない場合）
  - `src/main.gs`が存在しない場合のみ作成
  - 以下の最小限のサンプルコードを作成：
    ```javascript
    /**
     * メイン関数
     */
    function main() {
      Logger.log("Hello, Google Apps Script!");
    }
    ```

## 5. BAT ファイル作成フェーズ

- `open.bat`の作成（存在しない場合、または上書き確認）
  - 既に存在する場合は内容を確認し、上書き前に確認を求める
  - PowerShell を使って`.clasp.json`から scriptId を読み取り、GAS エディタを開く
  - UTF-8（chcp 65001）で保存（既存の open.bat に合わせる）
  - 内容：
    ```batch
    @echo off
    chcp 65001 > nul
    :: .clasp.jsonからscriptIdを読み取る
    for /f "usebackq delims=" %%i in (`powershell -NoProfile -Command "(Get-Content .clasp.json -Raw | ConvertFrom-Json).scriptId"`) do set scriptId=%%i
    :: 前後の空白と引用符を削除
    set scriptId=%scriptId:"=%
    set scriptId=%scriptId: =%
    clasp open-script %scriptId%
    ```
- `pull.bat`の作成（存在しない場合、または上書き確認）
  - 既に存在する場合は内容を確認し、上書き前に確認を求める
  - GAS からコードを取得する
  - UTF-8（chcp 65001）で保存
  - 内容：
    ```batch
    @echo off
    chcp 65001 > nul
    clasp pull
    ```
- `push.bat`の作成（存在しない場合、または上書き確認）
  - 既に存在する場合は内容を確認し、上書き前に確認を求める
  - GAS にコードをアップロードする
  - UTF-8（chcp 65001）で保存
  - 内容：
    ```batch
    @echo off
    chcp 65001 > nul
    clasp push --force
    ```

## 6. VSCode 設定フェーズ

- `.vscode/`フォルダの作成（存在しない場合）
  - `mkdir .vscode`を実行（Windows の場合は`if not exist .vscode mkdir .vscode`）
  - 既に存在する場合はそのまま使用
- `.vscode/settings.json`の作成（存在しない場合、または上書き確認）
  - 既に存在する場合は内容を確認し、上書き前に確認を求める
  - `.gs`ファイルを JavaScript として認識する設定を追加
  - 内容：
    ```json
    {
      "files.associations": {
        "*.gs": "javascript"
      }
    }
    ```
  - 既に存在する場合は、`files.associations`セクションを確認し、`"*.gs": "javascript"`が含まれていない場合のみ追加

## 7. スクリプト ID 反映フェーズ

- ユーザーにスクリプト ID を求める
  - `.clasp.json`の`scriptId`を確認する
  - `scriptId`が未設定または空の場合、ユーザーに入力してもらう
  - `scriptId`が既に設定されている場合でも、ユーザーに確認を求める（変更するか、そのまま使用するか）
- `.clasp.json`への反映
  - ユーザーが入力または確認したスクリプト ID を`.clasp.json`の`scriptId`フィールドに反映する
  - `.clasp.json`が存在しない場合は、必要な設定を含めて作成する（フェーズ 3 で作成されているはずだが、念のため）
  - 既存の`.clasp.json`がある場合は、`scriptId`フィールドのみを更新する（他の設定は保持）
- 反映後の確認
  - `.clasp.json`の内容を確認し、`scriptId`が正しく設定されていることを確認する
  - `open.bat`が正しく動作することを案内する（`.clasp.json`から自動読み取りされるため）

## 8. README 作成フェーズ

- `README.md`の作成（存在しない場合、または上書き確認）
  - 既に存在する場合は内容を確認し、上書き前に確認を求める
  - 以下の内容を含める：
    - プロジェクトの概要
    - セットアップ手順（clasp のインストール、ログイン、プロジェクト作成）
    - BAT ファイルの使い方
      - `open.bat`: GAS エディタを開く（scriptId は`.clasp.json`から自動読み取り）
      - `pull.bat`: GAS からコードを取得
      - `push.bat`: GAS にコードをアップロード
    - scriptId の設定方法（`.clasp.json`の`scriptId`フィールドのみ編集すれば OK、`open.bat`は自動で読み取る）
    - 開発フロー（ローカルで編集 → `push.bat`でアップロード → GAS で実行）

## エラーハンドリング

- **既存ファイル**: 上書き前に確認を求める
- **既存フォルダ**: 既存構造を保持し、不足分のみ追加
- **clasp エラー**:
  - `clasp login`エラー: 認証プロセスの再実行を案内
  - `clasp create`エラー: プロジェクト名の重複や権限の問題を確認
  - `clasp push/pull`エラー: scriptId の確認、ネットワーク接続の確認を案内
- **`.claspignore`設定の問題**:
  - `clasp status`で`src`配下が`Untracked files`として表示される場合、`.claspignore`の設定を確認
  - `**/**`で全てを除外する方式が原因の可能性がある
  - 明示的に除外するファイルだけを指定する方式に変更することを推奨
  - 修正後は`clasp status`で`src`配下のファイルが`Tracked files`に表示されることを確認
- **API 未有効化**: Google Apps Script API の有効化手順を詳細に案内
- **Node.js/npm 未インストール**: インストール手順を案内
- **clasp 未インストール**: インストールコマンドを実行
- **scriptId 未設定**: `.clasp.json`の`scriptId`フィールドに入力するよう案内
- **中断時**: 完了したステップまでを保存し、再開時に前回の状態を確認
