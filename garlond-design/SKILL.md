---
name: garlond-design
description: |
  GarlondWorks デザインシステムのルータースキル。
  出力形式が明示されていないとき、または複数形式を横断するときに発動する。
  形式が明示されている場合は各専用スキルが発動するため、このスキルは起動しない：
    - Word → garlond-word
    - HTML → garlond-html
    - PPTX → garlond-pptx（anthropic-skills）
  「どの形式がいいか」「形式を決めてほしい」「複数形式で作って」など
  形式選択が必要なときにのみ使うこと。
  Keywords: output format, document format, which format, HTML vs Word vs PPTX, design system
---

# GarlondWorks デザインシステム ルーター

## 形式選択ガイド

| 形式 | 判断軸 | 専用スキル |
|---|---|---|
| PPTX | 対面で「見せる」 | garlond-pptx |
| HTML | ブラウザで「使う」・URL共有 | garlond-html |
| Word | 印刷して「読む」・稟議・マニュアル | garlond-word |

迷ったら上記の判断軸で形式を決め、対応スキルに誘導する。

## ブランドカラー（全形式共通）

詳細は `assets/brand.json` を参照。

| 案件種別 | Primary | Accent（固定） |
|---|---|---|
| GarlondWorks自社 | #3D3939 | #00D5D5 |
| 建設業DX | #1E3A5F | #00D5D5 |
| 税務・会計 | #1B4332 | #00D5D5 |
| 製造・技術 | #312E81 | #00D5D5 |
| ヘルスケア | #065A82 | #00D5D5 |
| 汎用ビジネス | #1F2937 | #00D5D5 |

---

## ⚠️ Gotchas（やりがちな失敗）

**形式が決まっているのにこのスキルを使う**
形式が明示されていたら専用スキルに直接行く。このスキルは「形式を迷っているとき」専用。

**「なんとなくPPTX」にする**
印刷・稟議・マニュアルはWordの方が適切な場合が多い。判断軸の表で確認してから決める。

**ブランドカラーを案件種別に合わせず全部同じにする**
GarlondWorksカラー（#3D3939）はGarlondWorks自社案件専用。クライアント向けは案件種別に合わせて切り替える。
