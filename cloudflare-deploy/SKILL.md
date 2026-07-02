---
name: cloudflare-deploy
disable-model-invocation: true
description: |
  HTMLファイルをCloudflare Pagesに公開してURLを発行するスキル。
  以下のいずれかで起動する：
  - 「デプロイして」「公開して」「URLを発行して」
  - 「クライアントに見せられるURLにして」「ブラウザで開けるようにして」
  - HTMLファイルが完成して共有・確認が必要なとき
  Keywords: deploy, publish, hosting, Cloudflare Pages, share URL, public link
---

# Cloudflare Pages デプロイスキル

## アカウント情報

- メール：garlond@works-ws.com
- Account ID：d800609f7811d4418ce4f9ba755516a5
- 認証：CLOUDFLARE_API_TOKEN（~/.zshrc に設定済み）

## デプロイ手順

### 1. 対象ファイルの確認

- 単一HTMLファイルか複数ファイル構成かを確認する
- ファイル名・保存先パスを確認する

### 2. プロジェクト名の決定

命名規則：`garlond-[案件略称]-[種別]`

例：
- `garlond-h-company-report`
- `garlond-kt-proposal`
- `garlond-sdt-summary`

### 3. デプロイ実行

単一HTMLファイルの場合（一時ディレクトリを使う）：

```bash
mkdir -p /tmp/deploy_site
cp [対象HTMLファイル] /tmp/deploy_site/index.html

# 初回のみ：プロジェクト作成
CLOUDFLARE_API_TOKEN=$(grep CLOUDFLARE_API_TOKEN ~/.zshrc | cut -d'"' -f2) \
  npx wrangler pages project create [プロジェクト名] --production-branch main 2>&1

# デプロイ（初回・更新共通）
CLOUDFLARE_API_TOKEN=$(grep CLOUDFLARE_API_TOKEN ~/.zshrc | cut -d'"' -f2) \
  npx wrangler pages deploy /tmp/deploy_site \
  --project-name [プロジェクト名] \
  --commit-dirty=true 2>&1
```

### 4. URL確認

デプロイ成功時に表示される URL：
`https://[プロジェクト名].pages.dev`

### 5. 動作確認

WebFetchでURLにアクセスしてレスポンスを確認する。

## 既存プロジェクト一覧の確認

```bash
CLOUDFLARE_API_TOKEN=$(grep CLOUDFLARE_API_TOKEN ~/.zshrc | cut -d'"' -f2) \
  npx wrangler pages project list 2>&1
```

---

## ⚠️ Gotchas（やりがちな失敗）

**同じプロジェクト名を使い回す**
同じプロジェクト名で再デプロイすると上書き更新になる。別案件・別バージョンは必ず別のプロジェクト名にする。

**トークンの取得方法を間違える**
`~/.zshrc` の `CLOUDFLARE_API_TOKEN` は `grep` + `cut -d'"' -f2` で取得する。`source ~/.zshrc` では環境変数として展開されない場合がある。

**デプロイ後にURLが即座に有効にならない**
Cloudflare のプロパゲーションに数秒〜数十秒かかる場合がある。WebFetchで確認してエラーが出ても少し待ってから再確認する。

**無料プランの上限**
月500デプロイまで。テスト目的の頻繁な再デプロイに注意。

---

## 注意事項

- 新しいプロジェクト名は自動作成される
- 無料プランで500デプロイ/月まで
