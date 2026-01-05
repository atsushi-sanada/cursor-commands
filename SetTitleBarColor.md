# タイトルバー色設定コマンド

プロジェクトの`.vscode\settings.json`にタイトルバーの色設定を追加または更新します。視認性を確保するため、背景色と文字色のコントラストを十分に確保します。

## 前提条件

- `window.titleBarStyle: "custom"`の設定状態を確認し、未設定の場合は処理完了後にユーザーに設定を促す通知を行う

## 実行ルール

### ファイル操作

- `.vscode`フォルダが存在しない場合は作成する
- `.vscode\settings.json`が存在しない場合は空の JSON オブジェクト`{}`を作成してから設定を追加する
- `.vscode\settings.json`が既に存在する場合：
  - タイトルバー関連の設定（`titleBar.activeBackground`, `titleBar.activeForeground`, `titleBar.inactiveBackground`, `titleBar.inactiveForeground`）があれば削除して再設定する
  - タイトルバー関連の設定がなければ挿入する
  - **既存の設定は必ずそのまま保持する**（上書きや削除は禁止）

### 色設定の要件

- **視認性の確保**：文字色は背景色に対して十分なコントラスト比（WCAG AA 基準以上、推奨 4.5:1 以上）を確保する
- **色の選択**：
  - 背景色はランダムに生成するが、暗すぎる（`#000000`～`#252525`）または明るすぎる（`#f5f5f5`～`#ffffff`）色は避ける
  - アクティブ背景：`#282828`～`#5a5a5a`の範囲でランダムに選択
  - 非アクティブ背景：アクティブ背景より明るめ（`#484848`～`#7a7a7a`）でランダムに選択
  - アクティブ文字色：背景に対して十分なコントラストを確保（通常は`#ffffff`または`#f0f0f0`）
  - 非アクティブ文字色：背景に対して十分なコントラストを確保（通常は`#cccccc`または`#b0b0b0`）

### JSON 構造の維持

- `workbench.colorCustomizations`が既に存在する場合は、その中にタイトルバー設定を追加または更新する
- `workbench.colorCustomizations`が存在しない場合は新規作成する
- JSON のフォーマットは読みやすく整形する（インデント 2 スペース）

## 実行手順

1. グローバル設定の確認

   - グローバルの User Settings.json を確認（以下のパスを順に確認）
     - Windows (VSCode): `%APPDATA%\Code\User\settings.json`
     - Windows (Cursor): `%APPDATA%\Cursor\User\settings.json`
     - macOS/Linux (VSCode): `~/.config/Code/User/settings.json`
     - macOS/Linux (Cursor): `~/.config/Cursor/User/settings.json`
   - `window.titleBarStyle`の設定値を確認
   - 設定値が`"custom"`でない場合は、未設定フラグを立てる

2. `.vscode`フォルダの確認と作成

   - `.vscode`フォルダが存在するか確認
   - 存在しない場合のみ作成する

3. `settings.json`の確認と読み込み

   - `.vscode\settings.json`が存在するか確認
   - 存在する場合は内容を読み込む
   - 存在しない場合は空の JSON オブジェクト`{}`を作成する

4. 既存設定の確認

   - `workbench.colorCustomizations`が存在するか確認
   - タイトルバー関連の設定（`titleBar.activeBackground`, `titleBar.activeForeground`, `titleBar.inactiveBackground`, `titleBar.inactiveForeground`）が存在するか確認

5. 色の生成

   - アクティブ背景色を`#282828`～`#5a5a5a`の範囲でランダムに生成
     - RGB 各成分（R, G, B）をそれぞれ`0x28`～`0x5a`の範囲で独立してランダムに生成する（彩度のある色を生成するため）
   - 非アクティブ背景色を`#484848`～`#7a7a7a`の範囲でランダムに生成（アクティブ背景より明るめ）
     - RGB 各成分（R, G, B）をそれぞれ`0x48`～`0x7a`の範囲で独立してランダムに生成する（彩度のある色を生成するため）
   - アクティブ文字色を背景に対して十分なコントラストを確保する色（通常は`#ffffff`）に設定
   - 非アクティブ文字色を背景に対して十分なコントラストを確保する色（通常は`#cccccc`）に設定

6. 設定の追加または更新

   - `workbench.colorCustomizations`が存在しない場合は新規作成
   - タイトルバー関連の設定が存在する場合は削除してから再設定
   - タイトルバー関連の設定が存在しない場合は追加
   - **既存の他の設定は必ずそのまま保持する**

7. JSON の保存

   - 整形された JSON を`.vscode\settings.json`に保存する
   - インデントは 2 スペースを使用

8. 設定確認と通知
   - 未設定フラグが立っている場合、処理完了後にユーザーに以下の内容で通知を行う：
     - タイトルバーの色設定を反映するには、グローバル設定で`window.titleBarStyle: "custom"`を設定する必要があること
     - 設定方法：Cursor の設定（`Ctrl+,`または`Cmd+,`）を開き、「titleBarStyle」で検索して「custom」を選択する
     - または、User Settings.json に直接`"window.titleBarStyle": "custom"`を追加する
     - 設定後は Cursor の再起動が必要な場合があること

## 設定例

```json
{
  "workbench.colorCustomizations": {
    "titleBar.activeBackground": "#4a3d2f",
    "titleBar.activeForeground": "#ffffff",
    "titleBar.inactiveBackground": "#6f5a52",
    "titleBar.inactiveForeground": "#cccccc"
  }
}
```

## 注意事項

- グローバル設定の`window.titleBarStyle: "custom"`が設定されていない場合、タイトルバーの色設定は反映されません
- 未設定の場合は処理完了後にユーザーに設定を促す通知を表示します
- 色のコントラスト比が不十分な場合は、文字色を自動調整して視認性を確保します
- 既存の設定を上書きしないよう、必ずマージ処理を実施します
