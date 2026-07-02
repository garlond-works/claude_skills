---
name: garlond-word
description: |
  GarlondWorks が作成する Word（.docx）文書のデザインスキル。
  以下のいずれかで起動する：
  - 「Wordで作って」「Word文書にして」「.docxで出力して」
  - 「マニュアル作って」「手順書にして」「引き継ぎ資料作って」
  - 「稟議書作って」「業務定義書」「仕様書」
  - 印刷して渡す・稟議にかける・クライアントがWordで受け取りたいとき
  python-docx で生成。游ゴシック・GarlondWorksブランドカラーで統一。
  Keywords: Word, .docx, manual, procedure, specification, document, python-docx
---

# GarlondWorks Word スキル

## 基本哲学
- 印刷して読む文書。余白・行間・フォントサイズが読みやすさを決める
- 見出し階層を2段まで（h1・h2）。それ以上は箇条書きで代用
- 表はヘッダー行をPrimary色で強調。交互背景で読みやすく

## ステップ1：テーマカラーを選ぶ

`assets/brand.json` を参照。案件種別に合わせて PRIMARY を切り替える。
ACCENT（ターコイズ `#00D5D5`）は区切り線・バッジに使う。

## ステップ2：パターンを選ぶ

`references/word-patterns.md` に4パターン収録。

| パターン | 用途 |
|---|---|
| 提案書・報告書 | 表紙＋見出し構成・A4縦標準 |
| マニュアル・手順書 | 番号付き手順＋スクリーンショット枠 |
| 比較表・仕様表 | ヘッダー強調テーブル・交互背景 |
| 引き継ぎ・業務定義書 | 担当者・頻度・手順のメタ情報付き |

## ステップ3：実装

`references/word-patterns.md` のコードを使って生成する。
保存先：プロジェクトフォルダ内 `outputs/`
命名：`YYYYMMDD_[案件イニシャル]_[種別].docx`

## 参照スキル

このスキル起動後、作業前に `/mnt/skills/public/docx/SKILL.md` を必ず読む。

## QAチェック
- フォントが游ゴシックになっているか
- ブランドカラーが一致しているか
- マージンが上下2.0cm・左右2.5cmか
- 表のヘッダー行が Primary 色になっているか

---

## ⚠️ Gotchas（やりがちな失敗）

**見出し階層を3段以上作る**
h3以降はWordで崩れやすく、印刷時に見にくくなる。3段必要な場合は箇条書きに切り替える。

**フォントを指定せずに生成する**
python-docxはデフォルトでCalibriになる。必ず游ゴシック（`Yu Gothic`）を明示的に指定する。

**HTMLで代替できる資料にWordを使う**
URL共有・フィルタ・インタラクションが必要なものはHTMLの方が適切。「印刷して読む・稟議にかける」が必要なときだけWordにする。

**表の列幅を自動任せにする**
python-docxの自動列幅は崩れやすい。列幅は必ず明示的に指定する（`table.columns[n].width = Cm(x)`）。
