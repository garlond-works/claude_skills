---
name: PDFドキュメント生成
description: |
  ReportLab（Python）で新規PDFを生成するときに使う。
  以下のいずれかで起動する：
  - 「PDFで作って」「PDF出力して」「PDFレポート作って」
  - 「証明書にして」「帳票を作って」「印刷・配布・署名前提の文書にして」
  ※ 既存PDFを読む・テキスト抽出・ページ操作 → anthropic-skills:pdf を使うこと
  ※ PPTX生成 → garlond-pptx、Word生成 → garlond-word を使うこと
  Keywords: PDF, generate, ReportLab, certificate, form, print, document
---

# PDFドキュメント生成スキル（新規作成専用）

## このスキルの用途
**ReportLab（Python）で PDF を新規生成する。**
- 使い捨てではなく印刷・配布・署名を前提とした文書
- PPTX/HTML/Wordで代替できない場合（例：証明書、複雑な帳票）

## 混同しやすいスキルとの違い

| やりたいこと | 使うスキル |
|---|---|
| PDF を新規作成する | **このスキル**（pdf-document） |
| 既存PDFを読む・抽出・操作 | anthropic-skills:pdf |
| PPTX作成 | garlond-pptx |
| Word文書作成 | garlond-word |
| HTMLレポート作成 | garlond-html |

## 使用技術
- ReportLab（Python）
- フォント：IPA Gothic（`/usr/share/fonts/opentype/ipafont-gothic/ipag.ttf`）

## 作業手順
1. 構成・レイアウト案を提示して確認を取る
2. 承認後にコードを生成する
3. はみ出しチェックを実施してから納品する

## レイアウトルール（厳守）
- テーブル列幅は **CONTENT_W（174mm）以内**
- 横並びで収まらない場合は縦積みに切り替える
- 3列以上の横並びフローボックス・タグ＋テキスト混在列は特に注意

## デザインルール（厳守）
- サブセクション見出しと直後の本文・テーブルは **whitecard 内部に物理的に一体化**する
- 見出し単独の `story.append` 禁止
- **孤立・重複見出し 絶対禁止**

## ブランドカラー
- メイン：`#3D3939` / アクセント：`#00D5D5`

## 参照スキル

このスキル起動後、作業前に `/mnt/skills/public/pdf/SKILL.md` を必ず読む。

---

## ⚠️ Gotchas（やりがちな失敗）

**はみ出しチェックを省く**
ReportLabはテーブルが用紙幅を超えても自動折り返しせずはみ出す。列幅の合計が必ずCONTENT_W（174mm）以内に収まっているか確認してから納品する。

**見出しを単独でstory.appendする**
見出し直後に改ページが入ると見出しだけが前ページに残る（孤立見出し）。`KeepTogether` でセクション全体をまとめるか、whitecard内に一体化する。

**フォントを指定しない**
ReportLabのデフォルトフォントは日本語非対応。必ずIPA Gothicを登録してから使う。

**HTMLやWordで代替できるものにReportLabを使う**
ReportLabは設定が複雑。帳票・証明書以外はgarlond-html / garlond-wordで作る方が速い。

---

## 注意事項
- クライアント社名はイニシャル表記（H社・KT・SDT）
- 中華系フォント・サービスは使用しない
