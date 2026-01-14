# ワークスペース色設定コマンド

プロジェクトの`.vscode\settings.json`にワークスペース全体の色設定を追加または更新します。色相をランダムに決定し、彩度と明るさは`docs/color-scheme-spec.md`で定義された 5 段階のレベルに従って設定することで、プロジェクト毎に視覚的に区別できるカラースキームを実現します。

## 前提条件

- `window.titleBarStyle: "custom"`の設定状態を確認し、未設定の場合は処理完了後にユーザーに設定を促す通知を行う
- 色設定の詳細仕様は`docs/color-scheme-spec.md`を参照

## 色生成ルール

### 色相の決定（重要）

**色相は 1 回だけランダムに決定し、すべてのレベル（1 ～ 5）で同じ色相を使用する**

1. **メイン色相**：0 ～ 360 度の範囲でランダムに 1 つ選択
2. **全レベルで統一**：レベル 1 ～ 5 のすべての色は、この同じ色相を使用
3. **アクセント色相**：アクティビティバー用に、メイン色相とは異なる色相を別途 1 つ選択

### 各レベルの明るさと彩度（`docs/color-scheme-spec.md`準拠）

| レベル          | 明度 | 彩度                | 主な用途                     |
| --------------- | ---- | ------------------- | ---------------------------- |
| 1（最も明るい） | 50   | 中程度（20 ～ 30%） | タイトルバー、ステータスバー |
| 2               | 22   | 低め（18%）         | 入力欄、パネル境界線         |
| 3               | 16   | 低め（20%）         | サイドバー、非アクティブタブ |
| 4（最も暗い）   | 20   | 極小（1 ～ 3%）     | エディタ、ターミナル         |
| 5（超暗）       | 18   | 中程度（15 ～ 20%） | セクションヘッダー、境界線   |

**非アクティブ色**：レベル 1 より明るく（明度 60 前後、彩度はレベル 1 と同程度）

### 参照色（紫がかった茶色系の例）

- レベル 1: `#523948`
- レベル 2: `#3d422e`
- レベル 3: `#2d3121`
- レベル 4: `#1e1c1d`（ほぼグレー）
- レベル 5: `#1c1419`
- 非アクティブ: `#725968`

## 実行手順

1. グローバル設定の確認

   - `%APPDATA%\Cursor\User\settings.json`を確認
   - `window.titleBarStyle`が`"custom"`でなければ未設定フラグを立てる

2. `.vscode`フォルダと`settings.json`の準備

   - `.vscode`フォルダがなければ作成
   - `settings.json`がなければ`{}`を作成、あれば読み込む

3. 色相を決定

   - **メイン色相を 0 ～ 360 度からランダムに 1 つ選択**
   - **アクセント色相を別途 1 つ選択**

4. 各レベルの色を生成

   - **すべてのレベル（1 ～ 5）でメイン色相を使用**
   - 明度と彩度のみを調整してレベル別の色を生成
   - アクティビティバーのみアクセント色相を使用

5. UI 要素に色を適用

   **タイトルバー・ステータスバー**（レベル 1）:

   - タイトルバー/ステータスバー背景: レベル 1
   - 文字色: `#ffffff`

   **アクティビティバー**（アクセント色相）:

   - 背景: アクセント色相（明度・彩度はレベル 3 ～ 4 の中間）
   - 文字色: `#ffffff` / `#cccccc`

   **サイドバー**（レベル 3、レベル 5）:

   - サイドバー背景: レベル 3
   - セクションヘッダー背景: レベル 5
   - 文字色: `#f8f8f2`

   **エディタ**（レベル 4、レベル 3）:

   - エディタ背景: レベル 4（彩度極小）
   - ガター背景: レベル 4
   - 行ハイライト: レベル 3
   - 文字色: `#f8f8f2`
   - 行番号: `#90908a` / `#c2c2bf`
   - カーソル: `#f8f8f0`
   - 空白文字: `#464741`
   - インデントガイド: `#464741` / `#767771`
   - 選択範囲: `#878b9180` / `#575b6180` / `#4a4a7680` / `#6a6a9680`

   **タブ**（レベル 4、レベル 3、レベル 5）:

   - アクティブタブ: レベル 4
   - 非アクティブタブ: レベル 3
   - タブ境界線: レベル 5
   - エディタグループヘッダー: レベル 3
   - 文字色: `#f8f8f2` / `#ccccc7`

   **パンくずリスト**（レベル 4）:

   - 背景: レベル 4
   - 文字色: `#ccccc7` / `#f8f8f2`

   **パネル・ターミナル**（レベル 4、レベル 2）:

   - パネル背景: レベル 4
   - パネル境界線: レベル 2
   - ターミナル背景: レベル 4
   - ターミナル文字色: `#f8f8f2`
   - ターミナル ANSI カラー: Monokai テーマの配色（固定）

   **入力欄・ウィジェット**（レベル 2、レベル 3）:

   - 入力欄/クイック入力: レベル 2
   - エディタウィジェット: レベル 3
   - 文字色: `#f8f8f2`

   **リスト・選択**（レベル 1、レベル 2、レベル 3）:

   - アクティブ選択: レベル 1
   - 非アクティブ選択: レベル 2
   - ホバー: レベル 3

   **テキストブロック**（レベル 3、レベル 4）:

   - テキストブロック引用: レベル 3
   - テキストコードブロック: レベル 4

6. 設定を保存

   - 既存の`workbench.colorCustomizations`に色設定を追加または更新
   - 既存の他の設定は保持
   - JSON を整形してインデント 2 スペースで保存

7. 未設定通知
   - `window.titleBarStyle: "custom"`が未設定の場合、設定を促す通知を表示

## ターミナル ANSI カラー（Monokai テーマ・固定値）

以下の ANSI カラーは色相に関わらず固定値を使用：

```json
"terminal.ansiBlack": "#333333",
"terminal.ansiRed": "#C4265E",
"terminal.ansiGreen": "#86B42B",
"terminal.ansiYellow": "#B3B42B",
"terminal.ansiBlue": "#6A7EC8",
"terminal.ansiMagenta": "#8C6BC8",
"terminal.ansiCyan": "#56ADBC",
"terminal.ansiWhite": "#e3e3dd",
"terminal.ansiBrightBlack": "#666666",
"terminal.ansiBrightRed": "#f92672",
"terminal.ansiBrightGreen": "#A6E22E",
"terminal.ansiBrightYellow": "#e2e22e",
"terminal.ansiBrightBlue": "#819aff",
"terminal.ansiBrightMagenta": "#AE81FF",
"terminal.ansiBrightCyan": "#66D9EF",
"terminal.ansiBrightWhite": "#f8f8f2"
```

## 設定例

以下は紫がかった茶色系（メイン色相: 約 330 度、アクセント色相: 約 90 度）の例：

```json
{
  "workbench.colorCustomizations": {
    "titleBar.activeBackground": "#523948",
    "titleBar.activeForeground": "#ffffff",
    "titleBar.inactiveBackground": "#725968",
    "titleBar.inactiveForeground": "#cccccc",
    "statusBar.background": "#523948",
    "statusBar.foreground": "#ffffff",
    "activityBar.background": "#3d4a28",
    "activityBar.foreground": "#ffffff",
    "sideBar.background": "#2d3121",
    "sideBar.foreground": "#f8f8f2",
    "editor.background": "#1e1c1d",
    "editor.foreground": "#f8f8f2",
    "terminal.background": "#1e1c1d",
    "input.background": "#3d422e",
    "panel.background": "#1e1c1d"
  }
}
```

## 注意事項

- グローバル設定の`window.titleBarStyle: "custom"`が必須
- **色相は 1 回だけランダムに選択し、全レベルで統一すること**（各レベルで異なる色相を使用しない）
- 既存の設定は上書きしない
- プロジェクト毎に色相が変わることで視覚的な区別が可能

## 参照

- 詳細仕様: `docs/color-scheme-spec.md`
- 色生成ガイドライン: `docs/color-scheme-spec.md`の「色生成ガイドライン」セクション
