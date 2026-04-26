---
name: GAS開発
description: Google Apps Script（GAS）でスクリプトを作成・修正するときに使う。スプレッドシート自動化・Gmail操作・Google Drive連携・freee連携・定期実行など。「GASで作って」「スプレッドシートを自動化して」「Googleフォームと連携して」などの指示で起動する。
---

# GAS開発スキル

## 基本方針
- Google Workspace（スプレッドシート・Gmail・Drive・フォーム・カレンダー）との連携を前提とする
- 中華系APIやサービスは使用しない
- シンプルで保守しやすいコードを書く（クライアントが後から触れるレベル）

## 作業手順（必ずこの順番で）
1. 何を自動化したいか・入力と出力を確認する
2. 処理フローを図解（テキスト）で提示して承認を得る
3. コードを生成する
4. テスト手順を一緒に提示する
5. 必要に応じてトリガー設定方法も案内する

## コードの書き方

### 基本ルール
- 関数は1つの役割だけ持つ（単一責任）
- 変数名は日本語コメント付きで意味が分かるように
- エラー処理は必ず入れる（try-catch）
- ログは `Logger.log()` で残す

### スプレッドシート操作
```javascript
// 推奨：まとめて取得・まとめて書き込み（1行ずつはNG・遅い）
const sheet = SpreadsheetApp.getActiveSpreadsheet().getSheetByName('シート名');
const data = sheet.getDataRange().getValues(); // まとめて取得
sheet.getRange(1, 1, rows.length, rows[0].length).setValues(rows); // まとめて書き込み
```

### トリガーの種類
- **時間ベース**: 毎日・毎週など定期実行
- **スプレッドシートの変更時**: onEdit
- **フォーム送信時**: onFormSubmit
- **手動実行**: メニューから呼び出し

## よく使うパターン

### Gmail送信
```javascript
GmailApp.sendEmail(to, subject, body, {name: '送信者名'});
```

### Drive操作
```javascript
DriveApp.getFolderById('フォルダID').createFile(name, content, mimeType);
```

### 外部API連携（freeeなど）
```javascript
const response = UrlFetchApp.fetch(url, {
  method: 'GET',
  headers: { 'Authorization': 'Bearer ' + token }
});
```

## クライアント別の用途
- **H社**: 日報集計・LINE WORKS連携・グリーンサイトCSV処理・GMOサイン後処理
- **KT税理士法人**: freee連携・CSV仕訳修正・クライアント別レポート自動生成
- **SDT株式会社**: 指示があるまで詳細を聞かない

## 注意事項
- GASの実行時間制限は6分（重い処理は分割する）
- スプレッドシートのIDやAPIキーはスクリプトプロパティに保存（コードに直書きしない）
- デプロイ後の権限スコープは最小限にする
