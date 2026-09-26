# Memo Flow

## あなたが行う初期設定

1. Googleスプレッドシートに次のシートを作り、1行目にカラム名をこの順で入力します。  
   - `メモ`: `id, user_id, channel, timestamp, content, tags, updated_at`（`user_id`は現在未使用の列です）  
   - `channels`: `name`
2. 「拡張機能 → Apps Script」を開き、`gas/Code.gs` を貼り付けます。
3. Apps Scriptの「プロジェクトの設定 → スクリプト プロパティ」で、`MEMO_APP_TOKEN` に好きな合言葉（パスワード）を設定します。
4. 「デプロイ → 新しいデプロイ → ウェブアプリ」で公開します。アクセス権はCloudflare Pagesから到達できる「全員」にし、発行されたURLを控えます。
5. `public/app.js` の `CONFIG.apiUrl` を、上記URLへ変更します。
6. Cloudflare Pagesで `public` を公開ディレクトリとしてデプロイします（ビルド不要）。

ローカル確認は PowerShell で `npx serve public -p 3000` を実行し、`http://localhost:3000` を開きます。

このアプリは個人利用専用で、ログイン画面の代わりに手順3で設定した合言葉をアクセスのたびに入力する仕組みです（ブラウザには保存しません）。GAS側は `doPost` の action（unlock/list/create/update/delete）だけで処理し、すべてのリクエストで合言葉の一致を確認します。フロントエンドはCORSの事前確認を避けるため、`Content-Type: text/plain` でJSONをPOSTします。
