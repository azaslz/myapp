# Memo Flow

## あなたが行う初期設定

1. Googleスプレッドシートに次の3シートを作り、各1行目にカラム名をこの順で入力します。  
   - `メモ`: `id, user_id, channel, timestamp, content, tags, updated_at`  
   - `users`: `id, email, password_hash, password_salt, verified_at, created_at`  
   - `sessions`: `id, user_id, kind, token_hash, expires_at, created_at`
2. 「拡張機能 → Apps Script」を開き、`gas/Code.gs` を貼り付けます。
3. Apps Scriptの「プロジェクトの設定 → スクリプト プロパティ」で、`MEMO_APP_TOKEN` に長い共有シークレットを設定します。
4. 「デプロイ → 新しいデプロイ → ウェブアプリ」で公開します。アクセス権はCloudflare Pagesから到達できる「全員」にし、発行されたURLを控えます。
5. `public/app.js` の `CONFIG.apiUrl` と `CONFIG.token` を、上記URLと同じシークレットへ変更します。
6. Cloudflare Pagesで `public` を公開ディレクトリとしてデプロイします（ビルド不要）。

ローカル確認は PowerShell で `npx serve public -p 3000` を実行し、`http://localhost:3000` を開きます。

GAS側は `doPost` の action（list/create/update/delete）だけで処理しています。フロントエンドはCORSの事前確認を避けるため、`Content-Type: text/plain` でJSONをPOSTします。
