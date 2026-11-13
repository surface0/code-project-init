# テンプレート集

## CLAUDE.md

```markdown
# {プロジェクト名}

@.claude/architecture.md
@.claude/coding-style.md
@.claude/workflows.md
@.claude/context/current-sprint.md
@.claude/context/known-issues.md

## Quick facts
- 言語: {言語・バージョン}
- FW: {フレームワーク}
- DB: {データベース}
- CI: {CIサービス}
```

---

## .claude/settings.json

新規作成する場合はこの内容をそのまま使う。既存ファイルがある場合は
`plansDirectory` キーのみを追記し、他のキーは変更しない。

```json
{
  "plansDirectory": ".claude/plans"
}
```

| キー | 意味 |
|---|---|
| `plansDirectory` | プランモードが plan ファイルを書き出すディレクトリ。**プロジェクトルートからの相対パス**。未設定時は `~/.claude/plans/`（グローバル・全プロジェクト共有）。プロジェクトルート外を指す値は無視され、グローバルにフォールバックする。 |

plan をリポジトリに含めない場合は `.gitignore` に以下を追記する。

```gitignore
.claude/plans/
```

## .claude/architecture.md

```markdown
# アーキテクチャ方針

## 全体構成
{システム構成の概要}

## 技術選定の理由
- {技術}: {選んだ理由}

## 禁止パターン
- {やってはいけないこと}

## ディレクトリ構成
{主要ディレクトリの説明}
```

---

## .claude/coding-style.md

```markdown
# コーディング規約

## 命名規則
- 変数・関数: {camelCase / snake_case など}
- クラス: {PascalCase など}
- 定数: {UPPER_SNAKE_CASE など}
- ファイル: {kebab-case など}

## フォーマッタ / Linter
- {ツール名}: {設定ファイルのパス}

## コメント・ドキュメント
{コメントの書き方方針}

## その他のルール
- {プロジェクト固有のルール}
```

---

## .claude/workflows.md

```markdown
# よく使うコマンド・手順

## セットアップ
```bash
{セットアップコマンド}
```

## 開発サーバー起動
```bash
{起動コマンド}
```

## テスト
```bash
{テストコマンド}
```

## ビルド
```bash
{ビルドコマンド}
```

## デプロイ
{デプロイ手順}
```

---

## .claude/context/current-sprint.md

```markdown
# 現在の作業コンテキスト

最終更新: {日付}

## 今やっていること
- {現在のタスク}

## 一時的な制約・注意事項
- {今だけ有効なルールや制約}

## 次にやること
- {次のタスク}
```

---

## .claude/context/known-issues.md

```markdown
# 既知の問題・注意事項

## ハマりやすい箇所
### {問題のタイトル}
- **症状**: {どうなるか}
- **原因**: {なぜそうなるか}
- **回避策**: {どう対処するか}

## 地雷・禁止事項
- {絶対にやってはいけないこと・理由}
```
