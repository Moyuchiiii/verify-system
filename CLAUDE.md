# verify

情報の裏取り・ファクトチェック専門プロジェクト。

@.claude/rules/soul.md

## 基本方針

- 日本語で応答する
- ソースなし・裏取りなしの完了宣言は禁止
- すべての検証結果にソースURLを必ず付記する
- 不確実な情報は「⚠️ 未検証」として明示する
- IMPORTANT: タスク実行前に `.claude/rules/` を確認すること

## コマンド

| コマンド | 用途 |
|---|---|
| `/fact-check` | テキスト・MDレポートのファクトチェック |
| `/pdf-scan` | PDFを読み込んでクレーム抽出 → 裏取り |
| `/cross-verify` | 複数ソースでクロス検証 |

## エージェント構成

```
リード（Opus）
  ├── investigator（証拠収集・WebSearch）
  ├── verifier（ACH競合仮説分析・反証チェック）
  └── document-reviewer（一貫性・完全性スコアリング）
```

## ファイル配置

- 検証したいレポート: `input/reports/`
- 検証したいPDF: `input/pdfs/`
- 検証結果: `output/YYYY-MM-DD/`

## 出力形式

各クレームに対して以下の4段階で判定し、`output/YYYY-MM-DD/` に保存する:

- ✅ 検証済み（ソースURL必須）
- ⚠️ 部分的に正確（差異を詳細説明）
- ❌ 誤り（反証ソースURL必須）
- 🔍 未検証（追加調査が必要な理由を明記）

## コミット規約

- メッセージは日本語
- 形式: `<種別>: <概要>`
- 種別: feat / fix / docs / chore
