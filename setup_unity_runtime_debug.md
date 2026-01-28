# Unityランタイムデバッグ：セットアップ（エクステンション + `launch.json`）

## ゴール

Unity 実行中（Play中）に、Cursor から Unity Editor へアタッチしてブレークポイントで停止できる状態にする。

## 前提条件

- Cursor が起動できる
- 対象の Unity プロジェクト（フォルダ）を Cursor で開いている

## 成果物（作られる/変更されるもの）

- Cursor に以下のエクステンションがインストールされている
  - C# Extension（ノート記載：Anysphere / Cursor 開発元）
  - .NET Install Tool（`ms-dotnettools.vscode-dotnet-runtime`）
  - DotRush（`nromanov.dotrush`）
- DotRush の初期設定（`.sln` / `.csproj` の選択）が完了している
- `.vscode/launch.json` が存在し、既存内容を保持したまま以下の設定が追加されている（重複は作らない）

```json
{
  "name": "Unity Debugger",
  "type": "unity",
  "request": "attach"
}
```

---

## 実行ルール

- ユーザー確認は不要。**本コマンド内のタスクが完了するまで中断せずに実行**する。
- 推測で別のエクステンションに置き換えない（見つからない場合は停止して報告する）。
- 既存ファイルは無断で上書きしない（追記・追加のみ）。
- `launch.json` が JSON として壊れている場合は、勝手に修復せず停止して報告する。
- 実行不能（例：必要なエクステンションが見つからない、JSONが破損している等）の場合のみ停止し、理由と次の打ち手を短く報告する。

---

## 実行手順

## フェーズ1：エクステンションの現状確認（未インストールか確認）

1. Cursor の拡張機能パネルを開く（Extensions）
2. 以下の3つをそれぞれ検索し、インストール済みか記録する
   - **C# Extension（ノート記載：Anysphere / Cursor）**
   - **.NET Install Tool**（ID: `ms-dotnettools.vscode-dotnet-runtime`）
   - **DotRush**（ID: `nromanov.dotrush`）

この時点で、未インストールのものと、既に入っているものを短く一覧化してユーザーに提示し、そのままフェーズ2へ進む。

## フェーズ2：エクステンションのインストール

### 2-1) .NET Install Tool

- 未インストールならインストールする（ID: `ms-dotnettools.vscode-dotnet-runtime`）

### 2-2) DotRush（Unityアタッチデバッグ用）

- 未インストールならインストールする（ID: `nromanov.dotrush`）

### 2-3) C# Extension（Cursor開発元）

- 未インストールなら、ノート記載の **Anysphere (Cursor) の C# Extension** をインストールする
- **見つからない場合**
  - 代替案を勝手に選ばず、検索結果（候補の表示名）と現状を報告して停止する（次の指示待ち）

インストール後に「再起動 / Reload Window」を求められた場合は、それに従う。

完了したら、インストール結果（3つの状態）を短く報告し、そのままフェーズ3へ進む。

## フェーズ3：DotRushの初期設定（プロジェクト/ソリューション選択）

1. コマンドパレットを開く
2. `DotRush: Pick Project or Solution files` を実行する
3. 対象の `.sln` または `.csproj` を選択する

完了したら、選択したファイル名（`.sln` / `.csproj`）を報告し、そのままフェーズ4へ進む。

## フェーズ4：`.vscode/launch.json` の現状確認

1. Unity プロジェクトルート配下に `.vscode/` があるか確認する
2. `.vscode/launch.json` があるか確認する
3. `.vscode/launch.json` が存在する場合
   - JSON として正しくパースできるか確認する
   - ルートがオブジェクトであることを確認する
   - `configurations` が配列かどうか確認する（無ければ「無い」として扱う）
4. `configurations` 内に、以下と同等の設定が既にあるか確認する
   - `name` が `Unity Debugger` で `type` が `unity` かつ `request` が `attach`

確認結果（フォルダ/ファイル有無、既存設定の有無、壊れていないか）を短く報告し、そのままフェーズ5へ進む。

## フェーズ5：追加方針の確定

以下の方針で進める。

- `.vscode/` が無い：作成する
- `launch.json` が無い：新規作成する
- `launch.json` がある：既存内容を保持したまま `configurations` に追加する
- `Unity Debugger` が既にある（同等設定）：何もしない
- `Unity Debugger` という名前が既にあるが内容が違う：既存は変更せず、**別名**で追加する（例：`Unity Debugger (attach)`）

## フェーズ6：作成/追記

### 6-1) `.vscode/` の作成（必要な場合のみ）

- `.vscode/` が無ければ作成する

### 6-2) `launch.json` の作成（ファイルが無い場合）

- `.vscode/launch.json` を新規作成し、以下をそのまま記述する

```json
{
  "version": "0.2.0",
  "configurations": [
    {
      "name": "Unity Debugger",
      "type": "unity",
      "request": "attach"
    }
  ]
}
```

### 6-3) `launch.json` の追記（ファイルがある場合）

- 既存のトップレベルキーは保持する
- `configurations` が無い場合は新規に配列を作る
- `configurations` が配列の場合は、同等設定が無ければ以下を追加する
  - 基本は `name: "Unity Debugger"`
  - ただし、同名が存在して内容が異なる場合は `name: "Unity Debugger (attach)"` で追加する

追記後、ファイル全体が JSON として正しくパースできることを確認し、そのままフェーズ7へ進む。

## フェーズ7：完了通知

- ユーザーに以下を通知する

```
デバッグ開始方法

1. F1キーを押し、DotRush: Pick Project or Solution files と入力
2. <プロジェクト名>.sln を選択
3. F5: Unityエディタにアタッチしてデバッグ開始。
4. ブレークポイントを設定してUnityをPlayモードにすると対象箇所で停止することを確認
5. Shift + F5: デバッグセッションを終了


