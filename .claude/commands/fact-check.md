---
name: fact-check
description: テキストまたはMarkdownレポートのファクトチェックを実行する。investigator→verifier→document-reviewerの順で処理し、対応するHTMLも修正する。
trigger: /fact-check
---

# fact-check

## 使い方

```
/fact-check [ファイルパス or テキスト]
```

例:
- `/fact-check input/report.md`
- `/fact-check "AIの世界市場は2025年に1兆ドルを超えた"`

## 実行フロー

1. **クレーム抽出**: 対象MDから検証すべき主張を箇条書きで抽出する
2. **investigator起動**: 各クレームに対して証拠収集を並列実行する
3. **verifier起動**: 収集した証拠に対してACH分析を実行する
4. **document-reviewer起動**: 全クレームの判定結果を統合してスコアリングする
5. **HTMLの修正**: `input/` 内に同名のHTMLファイルがあれば、判定結果を反映して修正する
   - ❌ 誤りのクレームは該当箇所に打ち消し線＋修正注記を挿入する
   - ⚠️ 部分的に正確な箇所は該当箇所に注記を挿入する
   - ✅ 検証済みはそのまま
6. **ファイル移動**: 処理完了後、入力ファイル（MD・HTML）を `input/` から `output/YYYY-MM-DD/` に移動する
7. **レポート保存**: `output/YYYY-MM-DD/fact-check-[元ファイル名].md` に保存する

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
