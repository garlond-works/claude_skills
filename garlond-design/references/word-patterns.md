# Word（.docx）出力パターン集

`python-docx` を使用して生成する。
フォント：游ゴシック（Windows標準）/ ヒラギノ角ゴシック（Mac）/ Noto Sans CJK（Linux）

```python
from docx import Document
from docx.shared import Pt, Cm, RGBColor
from docx.enum.text import WD_ALIGN_PARAGRAPH
from docx.oxml.ns import qn
import re

# ブランドカラー（案件に応じて差し替え）
PRIMARY  = RGBColor(0x3D, 0x39, 0x39)   # ダークチャコール
ACCENT   = RGBColor(0x00, 0xD5, 0xD5)   # ターコイズ
GRAY1    = RGBColor(0x6B, 0x72, 0x80)

def set_font(run, size_pt, bold=False, color=None, italic=False):
    run.font.size = Pt(size_pt)
    run.font.bold = bold
    run.font.italic = italic
    if color: run.font.color.rgb = color
    run.font.name = "游ゴシック"
    run._element.rPr.rFonts.set(qn("w:eastAsia"), "游ゴシック")

def set_paragraph_spacing(para, before=0, after=6, line=None):
    para.paragraph_format.space_before = Pt(before)
    para.paragraph_format.space_after  = Pt(after)
    if line: para.paragraph_format.line_spacing = Pt(line)
```

---

## パターン1：提案書・報告書（標準構成）

表紙＋本文構成。A4縦。

```python
doc = Document()

# ページマージン（A4: 上下20mm 左右25mm）
section = doc.sections[0]
section.top_margin    = Cm(2.0)
section.bottom_margin = Cm(2.0)
section.left_margin   = Cm(2.5)
section.right_margin  = Cm(2.5)

# ――― 表紙 ―――
doc.add_paragraph()  # 上部余白

title_para = doc.add_paragraph()
title_para.alignment = WD_ALIGN_PARAGRAPH.CENTER
run = title_para.add_run(document_title)
set_font(run, 22, bold=True, color=PRIMARY)
set_paragraph_spacing(title_para, before=60, after=12)

sub_para = doc.add_paragraph()
sub_para.alignment = WD_ALIGN_PARAGRAPH.CENTER
run = sub_para.add_run(subtitle)
set_font(run, 13, color=GRAY1)
set_paragraph_spacing(sub_para, before=0, after=60)

# アクセントライン（テーブルで代用）
line_table = doc.add_table(rows=1, cols=1)
line_table.style = "Table Grid"
cell = line_table.cell(0, 0)
cell.width = Cm(16)
cell._element.get_or_add_tcPr().append(
    # 背景色をACCENTに
)
# ※ python-docx でセル背景色設定：
from docx.oxml import OxmlElement
def set_cell_bg(cell, hex_color):
    tcPr = cell._tc.get_or_add_tcPr()
    shd = OxmlElement("w:shd")
    shd.set(qn("w:fill"), hex_color)
    shd.set(qn("w:val"), "clear")
    tcPr.append(shd)

set_cell_bg(cell, "00D5D5")
cell.paragraphs[0].paragraph_format.space_before = Pt(2)
cell.paragraphs[0].paragraph_format.space_after  = Pt(2)

doc.add_paragraph()

# 宛先・日付・作成者
for label, value in [("宛先", client_name), ("作成日", date_str), ("作成者", "GarlondWorks 宍戸俊郎")]:
    p = doc.add_paragraph()
    p.alignment = WD_ALIGN_PARAGRAPH.CENTER
    r1 = p.add_run(f"{label}：")
    set_font(r1, 11, color=GRAY1)
    r2 = p.add_run(value)
    set_font(r2, 11, bold=True, color=PRIMARY)
    set_paragraph_spacing(p, before=0, after=4)

doc.add_page_break()

# ――― 本文セクション ―――
def add_section_heading(doc, text, level=1):
    """h1: 大見出し / h2: 中見出し"""
    para = doc.add_paragraph()
    run = para.add_run(text)
    if level == 1:
        set_font(run, 14, bold=True, color=PRIMARY)
        # 左ボーダー（アクセント色）を疑似的にタブで表現
        para.paragraph_format.left_indent = Cm(0.4)
        set_paragraph_spacing(para, before=14, after=6, line=18)
    else:
        set_font(run, 11, bold=True, color=PRIMARY)
        set_paragraph_spacing(para, before=8, after=4, line=16)
    return para

def add_body(doc, text):
    para = doc.add_paragraph(text)
    set_paragraph_spacing(para, before=0, after=4, line=16)
    for run in para.runs:
        set_font(run, 10.5)
    return para

# 使用例
add_section_heading(doc, "1. 現状と課題", level=1)
add_body(doc, "現在の業務では〜")
```

---

## パターン2：マニュアル・手順書

番号付き手順＋スクリーンショット枠付き。

```python
def add_step(doc, step_num, title, body_text, note=None):
    # ステップ番号バッジ + タイトル
    p = doc.add_paragraph()
    r_num = p.add_run(f"  Step {step_num}  ")
    set_font(r_num, 10, bold=True, color=RGBColor(0xFF, 0xFF, 0xFF))
    # ※ ハイライト色はpython-docxのrun.font.highlight_colorで設定
    r_num.font.highlight_color = None  # 代わりにセル背景で対応
    r_title = p.add_run(f"  {title}")
    set_font(r_title, 11, bold=True, color=PRIMARY)
    set_paragraph_spacing(p, before=10, after=3)

    # 本文
    add_body(doc, body_text)

    # 注意事項（オプション）
    if note:
        p_note = doc.add_paragraph()
        r = p_note.add_run(f"⚠ {note}")
        set_font(r, 9.5, italic=True, color=RGBColor(0xD9, 0x77, 0x06))
        set_paragraph_spacing(p_note, before=2, after=6)

    # スクリーンショット枠（空のテーブルで代用）
    ss_table = doc.add_table(rows=1, cols=1)
    ss_table.style = "Table Grid"
    ss_cell = ss_table.cell(0, 0)
    set_cell_bg(ss_cell, "F5F8F8")
    p_ss = ss_cell.paragraphs[0]
    r_ss = p_ss.add_run("　（スクリーンショット）")
    set_font(r_ss, 9, color=GRAY1, italic=True)
    p_ss.alignment = WD_ALIGN_PARAGRAPH.CENTER
    p_ss.paragraph_format.space_before = Pt(20)
    p_ss.paragraph_format.space_after  = Pt(20)
    doc.add_paragraph()

# 使用例
add_step(doc, 1, "Sharepointにアクセスする", "ブラウザで〜を開く。", note="Internet Explorerは使用不可")
```

---

## パターン3：比較表・仕様表

ヘッダー行をアクセント色で強調。

```python
def add_comparison_table(doc, headers, rows_data):
    table = doc.add_table(rows=1 + len(rows_data), cols=len(headers))
    table.style = "Table Grid"

    # ヘッダー行
    hdr_row = table.rows[0]
    for i, h in enumerate(headers):
        cell = hdr_row.cells[i]
        set_cell_bg(cell, "3D3939")  # PRIMARY色
        p = cell.paragraphs[0]
        r = p.add_run(h)
        set_font(r, 9.5, bold=True, color=RGBColor(0xFF, 0xFF, 0xFF))
        p.alignment = WD_ALIGN_PARAGRAPH.CENTER

    # データ行
    for row_idx, row_data in enumerate(rows_data):
        row = table.rows[row_idx + 1]
        bg = "FFFFFF" if row_idx % 2 == 0 else "F5F8F8"
        for col_idx, val in enumerate(row_data):
            cell = row.cells[col_idx]
            set_cell_bg(cell, bg)
            p = cell.paragraphs[0]
            r = p.add_run(str(val))
            set_font(r, 9.5)

    doc.add_paragraph()

# 使用例
add_comparison_table(doc,
    headers=["項目", "現状", "改善後", "効果"],
    rows_data=[
        ["日報入力", "紙→手入力（30分）", "スマホ入力（5分）", "▲25分/日"],
        ["集計作業", "Excel手作業（2時間）", "自動集計（0分）", "▲120分/週"],
    ]
)
```

---

## パターン4：引き継ぎ・業務定義書

担当者・頻度・手順を整理する業務マスター形式。

```python
def add_task_definition(doc, task_name, owner, frequency, steps, notes=""):
    # タスクヘッダー
    add_section_heading(doc, task_name, level=2)

    # メタ情報テーブル
    meta_table = doc.add_table(rows=1, cols=4)
    meta_table.style = "Table Grid"
    for cell, label, value in zip(
        meta_table.rows[0].cells,
        ["担当者", owner, "頻度", frequency]
    ):
        set_cell_bg(cell, "F5F8F8")
        r = cell.paragraphs[0].add_run(label)
        is_label = label in ["担当者", "頻度"]
        set_font(r, 9.5, bold=is_label, color=GRAY1 if is_label else PRIMARY)

    doc.add_paragraph()

    # 手順
    for i, step in enumerate(steps, 1):
        p = doc.add_paragraph(style="List Number")
        r = p.add_run(step)
        set_font(r, 10)
        set_paragraph_spacing(p, before=0, after=3)

    # 注意事項
    if notes:
        p = doc.add_paragraph()
        r = p.add_run(f"注意: {notes}")
        set_font(r, 9.5, italic=True, color=RGBColor(0xD9, 0x77, 0x06))
        set_paragraph_spacing(p, before=4, after=8)

# 使用例
add_task_definition(doc,
    task_name="週次売上集計",
    owner="経理担当",
    frequency="毎週月曜 9:00",
    steps=["SharePointの〜フォルダを開く", "〜ファイルをダウンロード", "集計シートに貼り付け"],
    notes="月次処理と重なる月末は火曜に実施"
)
```

---

## 保存・命名規則

```python
import datetime
date_str = datetime.date.today().strftime("%Y%m%d")
doc.save(f"{date_str}_{client_initial}_{doc_type}.docx")
# 例: 20260519_H社_業務改善提案.docx
```

保存先：プロジェクトフォルダ内 `outputs/`
