---
name: garlond-html
description: |
  GarlondWorks が作成する HTML レポート・ダッシュボードのデザインスキル。
  以下のいずれかで起動する：
  - 「HTMLで出力して」「レポートにして」「ダッシュボードにして」
  - 「操作できる形で」「フィルタ付きで」「ブラウザで見られるように」
  - 「クライアント向けに整理して」「URLで共有したい」
  - 複雑な比較・進捗・レビュー結果をインタラクティブに見せたいとき
  単一ファイルHTML生成。Noto Sans JP・GarlondWorksブランドカラーで統一。
  フィルタ・折り畳み・ステータス表示など操作可能なインターフェースに仕上げる。
  Keywords: HTML report, dashboard, interactive, filter, browser, single file, Noto Sans
---

# GarlondWorks HTML スキル

## 基本哲学
- Markdown は「読むだけ」。HTML は「操作できる」インターフェース
- 単一ファイル（外部依存なし）が原則。ブラウザでそのまま開ける
- フィルタ・折り畳み・進捗バーで情報を整理する

## ステップ1：テーマカラーを選ぶ

`assets/brand.json` を参照。案件種別に合わせて `--primary` を切り替える。
`--accent: #00D5D5` は固定。

## ステップ2：パターンを選ぶ

`references/html-patterns.md` に5パターン収録。

| パターン | 用途 |
|---|---|
| 進捗レポート | フェーズ進捗バー＋リスクバッジ＋アクションリスト |
| 設計比較 | 複数案カード・フィルタ並び替え付き |
| コードレビュー | 重大度フィルタ付きレビュー結果 |
| 提案サマリー | 課題→解決策→効果の3ステップ＋Q&A折り畳み |
| KPIダッシュボード | 数値KPI大表示＋データテーブル |

## ステップ3：実装

`references/html-patterns.md` のテンプレートを使って生成する。
`{変数}` プレースホルダーを実際の内容に置き換える。
保存先：プロジェクトフォルダ内 `outputs/`
命名：`YYYYMMDD_[案件イニシャル]_[種別].html`

## QAチェック
- ブランドカラーが一致しているか（`--primary` / `--accent`）
- 単一ファイルで開けるか（外部CDN依存がある場合はオフライン注意）
- フィルタ・折り畳みが動作するか
- 印刷用CSS（`@media print`）が必要な場合は追加されているか

---

## ⚠️ Gotchas（やりがちな失敗）

**外部CDNを使う**
単一ファイル原則に反する。Tailwind・Font Awesome等のCDNは使わない。CSSはすべて `<style>` タグ内にインライン化する。Noto Sans JPはGoogle Fontsから読み込むが、オフライン利用が必要な場合はシステムフォントにフォールバックする旨を伝える。

**モバイル対応を後回しにする**
`<meta name="viewport">` と `@media` クエリは最初から入れる。

**JavaScriptをCDNから読み込む**
jQueryやlodash等の外部ライブラリは使わない。バニラJSで書く。

**`designer-elie` スキルとの使い分け**
このスキルはGarlondWorksブランドのHTMLに使う。クライアントのサイト風・別スタイルで作りたい場合は `designer-elie` を先に起動してデザイントークンを決める。
