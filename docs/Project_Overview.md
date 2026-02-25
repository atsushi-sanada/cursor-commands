# Project Overview（cursor-commands）

## 概要

このリポジトリは、Cursor エディタの Commands（`/コマンド`）として使う Markdown ファイルを集約したものです。
バグ修正、テスト実行、Git 操作、ドキュメント整形などの頻出作業を、誰が実行しても同じ結果になる手順として標準化します。

## このリポジトリで管理しているもの

### コマンド本体（リポジトリ直下の `*.md`）

- `/コマンド名` として実行するための「手順テンプレート（プロンプト）」です。
- 例：`fix_bug.md`（バグ修正の標準プロセス）、`run_test.md`（テスト実行→失敗時の再実行導線）など。

### 運用ドキュメント（`docs/`）

- `docs/Project_Overview.md`：本ドキュメント。プロジェクト全体の把握用
- `docs/rule_list.md`：`.cursor/rules/*.mdc` の提案一覧（目的と適用範囲の整理）
- `docs/color_scheme_spec.md`：ワークスペース配色などの仕様

### AI 振る舞い・執筆ルール（`.cursor/rules/`）

- `ai-response-governance.mdc`：推測で断定しない、段階的に進める等の応答品質ルール
- `commands-authoring.mdc`：Commands を再現性の高い手順として書くための執筆ルール
- `markdown-writing.mdc`：Markdown 表記ルール（見出し・コードフェンス等）
- その他：コミットメッセージ、README、BAT、リポジトリ命名など補助ルール

### エディタ設定（`.vscode/settings.json`）

- Cursor/VSCode のワークスペース設定を管理します（必要に応じて更新）。

## 使い方（導入と実行）

### 1) 配置

コマンドファイル（`.md`）は、以下のいずれかに配置します。

- グローバルに使う（推奨）：`%USERPROFILE%\.cursor\commands`
- プロジェクト固有で使う：`<project>/.cursor/commands`

### 2) 実行

1. Cursor を再起動します
2. チャットで `/コマンド名` を入力します
3. 表示された指示に従って作業します

## 主要コマンド（用途別）

### プロジェクトセットアップ

- `start_workflow.md`：要件定義→設計→実装計画→Issue 作成→実行の 5 段階フロー
- `setup_project_rules.md`：`AGENTS.md` と `.cursor/rules/` を整備する 3 段階フロー
- `setup_gas_environment.md`：GAS 開発環境構築（clasp 設定、プロジェクト構造、BAT 自動生成）
- `create_vscode_settings.md`：プロジェクトルートに `.vscode` と `settings.json` を作成
- `set_workspace_color.md`：ワークスペース色設定を `.vscode/settings.json` に追加・更新
- `fetch_project.md`：現在のプロジェクトを把握し、`docs/Project_Overview.md` に保存
- `create-cursor-rules-templates.md`：Cursor Rules のテンプレート作成

### タスク管理

- `run_todo_tasks.md`：指示を 5〜30 分の実行単位に分解し、完了まで順次実行する
- `suggest_only.md`：コード変更はせず提案のみ行う
- `git_commit_push.md`：安全ルールに沿ってコミットとプッシュを行う
- `git_pullrequest_upstream.md`：PR 作成（upstream ベース想定）

### コーディング・修正

- `fix_bug.md`：バグ修正を標準プロセス（原因/方針→把握→設計→検証→修正→報告）で進める
- `analyze_bug.md`：原因分析と修正方法の提案（コード修正なし）
- `set_logs.md`：切り分けのためのログ出力を挿入する
- `run_test.md`：テストを実行し、成功まで修正を繰り返す
- `add_inline_comment.md`：コードにインラインコメントを自動挿入
- `add_documentation_comment.md`：関数にドキュメンテーションコメントを自動付与
- `read_unity_editor_log.md`：Unity Editor ログを読み取り、エラー・警告を分析
- `setup_unity_runtime_debug.md`：Unity ランタイムデバッグ環境のセットアップ
- `create_readme.md`：README 作成ルールに基づき `README.md` を自動生成

### ドキュメント生成・整形

- `fix_easy_write.md`：新卒・初心者向けに文章を調整
- `formate_markdown.md`：Markdown ドキュメントを整形
- `clean_ai_docs.md`：AI 生成文章を資料として使えるように整形

### その他

- `hr_evaluation_prompts.md`：人事評価を出力

## 運用ルール（迷わないための基準）

- `AGENTS.md`：このリポジトリ全体の思想・方針（再現性、最小変更、根拠重視、段階進行）
- `.cursor/rules/`：Markdown/Commands の書き方・AI の振る舞いを「常に」揃えるためのルール

## 次に見るべき入口

- まず全体像を掴む：`README.md`
- プロジェクト把握：`docs/Project_Overview.md`（本ドキュメント）
- 実作業を段階で回す：`start_workflow.md`
- 目の前の指示を実行に落とす：`run_todo_tasks.md`
