# TODOアプリ 実装計画 (V1)

本ドキュメントは、要件定義（`01_requirements.md`）および設計書（`02_design.md`）に基づき、既存のプロジェクト構成（`index.ts`, `db.ts`, `package.json`, `tsconfig.json`）を活かしながら、既存のルートを一切壊さずに TODO 機能を安全に実装するための手順を定義します。

---

## 全体方針
- **ディレクトリ構成:** 既存構造への影響を最小限にするため、TODO関連ファイルは以下の専用フォルダに集約・追加する。
  - バックエンド/ルーティング: `src/apps/todo/`
  - 画面テンプレート: `views/todo/` （または既存の views 配下の todo フォルダ）
  - フロントエンドスクリプト/CSS: `public/todo/`
- **エントリーポイントの保護:** `index.ts` への変更は「`todoRouter` のルーティング登録（1行〜数行）」のみに留め、肥大化を防ぐ。

---

## 工程 1: SQLファイルのGit保存
- **対象ファイル:** `sql/001_create_todos.sql` （新規作成）
- **作業内容:**
  1. プロジェクト内に `sql` フォルダを作成する（存在しない場合）。
  2. 以下のテーブル作成用 SQL を記述したファイルを作成し、Gitにコミットする。
     ```sql
     CREATE TABLE IF NOT EXISTS todos (
         id SERIAL PRIMARY KEY,
         title VARCHAR(100) NOT NULL,
         completed BOOLEAN NOT NULL DEFAULT FALSE,
         created_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP
     );
     ```
- **確認コマンド:** `git status` および `git add sql/001_create_todos.sql`
- **期待結果:** `sql/001_create_todos.sql` が Git の管理下に追加されていること。
- **失敗時に最初に見る場所:** フォルダ作成漏れやファイルパスのタイポ。

## 工程 2: Neon SQL Editor でのテーブル作成（手動実行）
- **対象環境:** Neon PostgreSQL コンソール (Webブラウザ)
- **作業内容:**
  1. Neon のダッシュボードにログインし、対象プロジェクトの「SQL Editor」を開く。
  2. 工程 1 で作成した SQL の中身をエディタに貼り付けて実行（Run）する。
  3. ダッシュボードの Tables 画面などで `todos` テーブルが作成されたことを確認する。
- **確認コマンド:** （Neonコンソール上のSQLエディタで以下を実行）
  ```sql
  SELECT * FROM todos;