---
name: fact-check
description: テキストまたはMarkdownレポートのファクトチェックを実行する。investigator→verifier→document-reviewerの順で処理する。
trigger: /fact-check
---

# fact-check

## 使い方

```
/fact-check [ファイルパス or テキスト]
```

例:
- `/fact-check input/reports/2026-04-01-report.md`
- `/fact-check "AIの世界市場は2025年に1兆ドルを超えた"`

## 実行フロー

1. **クレーム抽出**: 対象テキストから検証すべき主張を箇条書きで抽出する
2. **investigator起動**: 各クレームに対して証拠収集を並列実行する
3. **verifier起動**: 収集した証拠に対してACH分析を実行する
4. **document-reviewer起動**: 全クレームの判定結果を統合してスコアリングする
5. **レポート保存**: `output/YYYY-MM-DD/fact-check-[タイムスタンプ].md` に保存する

## 出力形式

```markdown
# ファクトチェックレポート

- 対象: [ファイル名またはテキスト冒頭50文字]
- 実行日時: YYYY-MM-DD HH:mm
- 総合スコア: XX/100

## クレーム別判定

### クレーム1: [主張内容]
判定: ✅/⚠️/❌/🔍
信頼度: [検証済み/直接証拠あり/間接証拠のみ/推測]
根拠: [判定理由]
ソース: [URL]

...

## ドキュメント評価
[document-reviewerの出力をそのまま記載]
```
