# TODOアプリ V1 設計書

## 1. 作るものとファイルの役割
本設計書は、Express + TypeScript + EJS環境におけるTODOアプリV1の全体設計を定義するものです。
既存の `/` や `/api/db-check` の挙動を壊さないことを前提とします。

### ファイル構成（授業テキストに合わせた配置）
- **エントリーポイント:** **[要確認: index.ts または app.ts のどちらを使用しているか要確認]**
- **DB接続設定:** **[要確認: db.ts 等のファイルの有無・配置場所要確認]**
- **ルーティング・コントローラー:** **[要確認: routes/ または controllers/ などの具体的な配置場所要確認]**
- **画面テンプレート:** `src/views/todo.ejs` （または指定のビューディレクトリ）
- **フロントエンド用スクリプト:** `public/js/todoApi.js` （または TypeScriptファイル）

---

## 2. リクエストはどこを通るか
ユーザーが画面やAPIを操作した際のリクエストの流れは以下の通りです。

1. **画面表示 (`/todo`):**
   - ブラウザからのリクエストがエントリーポイントで受け取られ、対応するルーティングを通って `todo.ejs` がレンダリングされる。
2. **API操作 (`/api/todo/v1/todos`):**
   - ブラウザのフロントエンド（`TodoApi` クラス経由の Fetch API）からリクエストが送信される。
   - エントリーポイントを経由し、最小限のルーター（例: `src/routes/todo.ts` **[要確認]**) にルーティングされる。
   - コントローラー・サービス層にてNeon PostgreSQL（`DATABASE_URL`）にアクセスし、結果をJSON形式で返却する。

---

## 3. DBの設計
Neon PostgreSQLを使用し、以下の `todos` テーブルを対象とする。

### テーブル名: `todos`
- `id`: `SERIAL PRIMARY KEY` （1以上の整数、自動採番）
- `title`: `VARCHAR(100) NOT NULL` （前後の空白トリム済み、1〜100文字）
- `completed`: `BOOLEAN NOT NULL DEFAULT FALSE` （完了フラグ）
- `created_at`: `TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP`

---

## 4. APIの共通ルール
- **ベースパス:** `/api/todo/v1/todos`
- **成功レスポンス共通フォーマット:**
  ```json
  {
    "success": true,
    "data": ...
  }