以下の内容に基づき、プロジェクト用の高品質な `README.md` を生成してください。

# README自動生成プロンプト

> **目的**：以下の指示に従い、**誰でも迷わず実行できる**高品質な `README.md` を生成する。
> **出力要件**：**日本語**／**中学生でも理解できる語り口**／**見出し最大4階層**／**H1は1回のみ**

---

## 出力要件（必須）

* 文体：短文・平易・箇条書き中心（1文40～60文字目安）
* 冒頭に\*\*「何ができるか」\*\*を1～2行で断定
* 進行順は **クイックスタート → 詳細**
* **.bat / .sh / GUI** の簡易実行を**先に**提示、後で詳細コマンド
* コマンドはコードフェンスで記述（`bash` / `powershell`）
* 不要な章は出力しない（空見出し禁止）
* 依存ツール／SDKには**公式URL**を併記
* 画像や図は代替テキストを付与

---

## 章構成テンプレート

### 1. タイトル & 概要（必須）

* **プロジェクト名**
* **このプロジェクトでできること（1～2行）**
  例：画像を一括で最適化します。初心者でも3分で実行できます。

### 2. 主な機能（任意・最大5項目）

* 箇条書き
  `[実装済] / [予定]` ラベルで区別

### 3. 前提条件 / 必要な環境（必須）

* 対応OS：Windows / macOS / Linux
* 依存：例）Node.js 18+ / Python 3.10+ / .NET 8 など
* **動作確認コマンド（例）**

  * Node.js:

    ```bash
    node -v
    ```
  * Python:

    ```bash
    python --version
    ```
  * .NET:

    ```bash
    dotnet --info
    ```
* 各依存の**公式URL**を列挙

### 4. クイックスタート（最短・先に読む／3手順以内・必須）

* **番号付き手順**で3つ以内
* **.bat / .sh / GUI** がある場合は**ここで最優先**で案内

**例（Windows／バッチあり）**

1. 取得：

   ```bash
   git clone https://example.com/your/repo.git && cd repo
   ```
2. 実行（ダブルクリック可）：

   ```powershell
   scripts\run.bat
   ```
3. 成功確認：

   ```powershell
   scripts\verify.ps1
   ```

（想定出力を2～3行で併記）

**例（macOS/Linux）**

```bash
chmod +x scripts/run.sh
./scripts/run.sh
```

### 5. インストール方法（詳細・必須）

**A. ツール（CLI/GUI）のインストール**

* OS別に **事前チェック → 入手 → 配置 → PATH設定 → 検証** の順で記載

**例（CLI・Windows）**

1. 事前チェック：

   ```powershell
   where.exe yourtool
   ```
2. 入手：公式リリース（URL記載）から `.zip` を取得
3. 展開：`C:\Tools\yourtool\` に配置
4. PATH登録（GUI と PowerShell 手順を各1つ）
5. 検証：

   ```powershell
   yourtool --version
   ```

**B. ライブラリのインストール（言語別）**

* Node.js:

  ```bash
  npm i your-lib
  ```
* Python:

  ```bash
  pip install your-lib
  ```
* \*\*最小サンプル（10～15行）\*\*を併記

> 依存（GPU / ffmpeg / Java 等）がある場合は、**公式URL**と**検証コマンド**を必ず記載。

### 6. 使い方（Usage）— **簡易 → 詳細**（必須）

**① かんたん実行（推奨・先頭）**

* `.bat` / `.sh` / GUI の具体的操作を明記
* Windowsバッチの前提：**UTF-8**、**CRLF**、先頭に `chcp 65001 >nul`
* 実行例（Windows）：

  ```powershell
  scripts\run.bat
  ```
* 想定出力例（2～5行）

**② 詳細な使い方（コマンド／オプション）**

* シンタックス：

  ```bash
  yourtool <input> -o <out> --quality 80 --dry-run
  ```
* 主要オプション（5～10項目、既定値を数値で明記）
  例：`--quality`（0–100、既定: 80）、`--threads`（既定: 4）
* 入出力サンプル（最小3例：単一／複数／ワイルドカード）

**③ よく使うレシピ（任意・3例まで）**

* 例：「JPEGだけ処理」「フォルダ再帰」「ログ保存」

### 7. ディレクトリ構成（任意）

* `tree -L 2` 形式＋主要フォルダの一言説明
* 例：`docs/`, `tests/`, `scripts/`, `config/` を分離

### 8. 設定（任意）

* `.env` 例と**各キーの意味／型／既定値**（表形式）
* 秘密情報は記載禁止、管理方法を明記

### 9. 開発（任意）

* `lint` / `format` / `test` のコマンド、ローカル起動方法

### 10. デプロイ（任意）

* Docker / GitHub Actions 等を使う場合のみ最小手順で

### 11. トラブルシューティング（必須・5項目以上）

* 形式：**症状 → 原因 → 解決手順（コマンド付き）**

**例1：** `command not found` → PATH 未設定 → 環境変数に追記
**例2：** PowerShell 実行制限 → 管理者権限で設定

```powershell
Set-ExecutionPolicy RemoteSigned -Scope CurrentUser
```

**例3：** macOS Gatekeeper ブロック → 右クリックで開く、または解除

```bash
xattr -dr com.apple.quarantine yourapp.app
```

**例4：** 権限不足（Linux）

```bash
chmod +x scripts/run.sh
```

**例5：** プロキシ環境で失敗 → `HTTP_PROXY` / `HTTPS_PROXY` を設定

### 12. ライセンス / クレジット / 参考リンク（必要時）

* ライセンス表記（MIT / Apache-2.0 等）
* 依存の**公式URL**（Node / Python / .NET / ffmpeg 等）

---

## 生成対象の種類ごとの指示（自動適用）

* **ライブラリ**：`インストール` と `使い方（API最小例）` を最優先。サンプルは10～15行。
* **Webアプリ**：`クイックスタート（起動まで）`、`画面URL/認証`、`主要スクリプト` を重視。
* **スクリプト集**：`依存関係`、`代表スクリプト3本`、`まとめ実行用 .bat/.sh` を先頭で案内。
* **CLI/GUIツール**：`.bat/.sh/インストーラ` を先頭、コマンド詳細は次段、オプション表は10項目以内。

---

## 追補：.bat / .sh 作成規定（READMEに明記）

* **.bat（Windows）**：UTF-8、CRLF、先頭行に

  ```bat
  chcp 65001 >nul
  ```
* **.sh（Unix系）**：LF、実行権付与

  ```bash
  chmod +x scripts/run.sh
  ```
* ログは最小限。失敗時は非0の終了コードを返す。

---

## 品質チェック（生成前に自己検証）

* [ ] 冒頭で「何ができるか」を**2行以内**で断定
* [ ] **クイックスタートが3手順以内**かつ\*\*.bat/.sh/GUIを最優先\*\*
* [ ] **ツールのインストール**が **事前チェック → 入手 → 配置 → PATH → 検証** で完走
* [ ] 依存ごとに**公式URL**と**検証コマンド**がある
* [ ] **トラブルシューティング5項目以上**（コマンド付き）
* [ ] コードフェンス（`bash` / `powershell`）と改行が正しい
* [ ] 不要な見出しは出力していない（H1は1回）
