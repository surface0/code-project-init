---
name: code-project-init
description: "プロジェクトに CLAUDE.md と .claude/ ディレクトリ構成をセットアップするスキル。 「CLAUDE.mdを作って」「Claude Code用のファイルを設定したい」「プロジェクトにClaudeの設定を追加したい」 「.claude ディレクトリを作りたい」「Claude用のコンテキストファイルを作りたい」など、 Claude Code向けのプロジェクト設定ファイルを生成・整備するときは必ずこのスキルを使うこと。\n"
---

# Claude Project Setup スキル

プロジェクトに Claude Code 向けの設定ファイル群を生成するスキルです。

## 生成する構成

```
project/
├── CLAUDE.md                        # エントリポイント（薄く保つ）
└── .claude/
    ├── settings.json                # プロジェクト設定（plan 保存先など・常に配置）
    ├── architecture.md              # アーキテクチャ方針（変化少ない）
    ├── coding-style.md              # コーディング規約（変化少ない）
    ├── workflows.md                 # よく使うコマンド・手順（中程度）
    ├── context/
    │   ├── current-sprint.md        # 今やっていること（頻繁に変わる）
    │   └── known-issues.md          # 既知の問題・地雷（随時追記）
    ├── plans/                       # プランモードの出力先（settings.json で指定）
    └── skills/
        └── session-record/
            └── SKILL.md             # セッション記録スキル（context更新用・常に配置）
```

## 手順

### Step 0: 既存ファイルを確認する

作業開始前に、プロジェクトに既存の `CLAUDE.md` や `.claude/` ディレクトリがあるか確認する。

- **既存ファイルがない場合** → Step 1 へ進む
- **CLAUDE.md はあるが `.claude/` がない（肥大化ケース）** → Step 0a へ進む
- **両方ある場合** → 内容を読んで不足ファイルを補う形で Step 2 へ進む

#### Step 0a: 既存 CLAUDE.md を分割する

1. 既存の `CLAUDE.md` を読み込む
2. 内容を以下の基準で分類する（`references/templates.md` の各ファイル定義を参照）
   - アーキテクチャ・設計方針 → `architecture.md`
   - 命名規則・規約 → `coding-style.md`
   - コマンド・手順 → `workflows.md`
   - 現在の作業・一時的制約 → `current-sprint.md`
   - 問題・地雷 → `known-issues.md`
   - 変わりにくい事実（言語・DBなど）→ `CLAUDE.md` の Quick facts に残す
3. 分類結果をユーザーに見せて確認を取ってから、ファイルを生成する
4. 既存 `CLAUDE.md` をポインタ形式に書き換える

### Step 1: プロジェクト情報を収集する

ユーザーに以下を確認するか、リポジトリ内のファイルから読み取る。

- **プロジェクト名・概要**（何を作っているか）
- **技術スタック**（言語・フレームワーク・DB・CI など）
- **主なコーディング規約**（命名規則・フォーマッタ・lintルール）
- **よく使うコマンド**（ビルド・テスト・デプロイ）
- **現在の作業コンテキスト**（今スプリントでやっていること）
- **既知の問題・注意事項**（ハマりやすい箇所、地雷）

情報が不足している項目はスキップしてプレースホルダーを入れる。

### Step 2: ファイルを生成する

各ファイルの中身は `references/templates.md` を参照して生成すること。

生成するファイルの優先順位：
1. `CLAUDE.md`（必須）
2. `.claude/architecture.md`（技術スタックが分かれば必須）
3. `.claude/coding-style.md`（規約が分かれば必須）
4. `.claude/workflows.md`（コマンドが分かれば必須）
5. `.claude/context/current-sprint.md`（現状が分かれば生成）
6. `.claude/context/known-issues.md`（問題が分かれば生成）

### Step 2b: session-record スキルを配置する（常に実行）

`.claude/skills/session-record/SKILL.md` を**必ず**配置する。中身はこのスキルに同梱した
`assets/session-record/SKILL.md.tmpl` を**そのまま丸ごとコピー**し、ファイル名を
`SKILL.md` にする（プロジェクト固有の置換は不要）。これにより、セットアップ済みプロジェクトでは「セッション終了して」等の
キーワードで context（current-sprint.md / known-issues.md）を更新できるようになる。

### Step 2c: settings.json に plan 保存先を設定する（常に実行）

`.claude/settings.json` に `plansDirectory` を設定し、プランモードの出力先を
プロジェクト内（`.claude/plans`）に固定する。既定では plan は
`~/.claude/plans/` にグローバル保存され、全プロジェクトが 1 つのフラットな
ディレクトリに混ざってしまうため。

テンプレートは `references/templates.md` の「.claude/settings.json」を参照。

手順：

1. `.claude/settings.json` が **存在しない場合** → テンプレートをそのまま作成する
2. **既に存在する場合** → 読み込んで `plansDirectory` キーのみを追記する
   （他のキーを絶対に消さない・上書きしない）
3. 既に `plansDirectory` が設定済みなら何もしない（ユーザーの設定を尊重する）
4. `.claude/plans/` を Git 管理するかユーザーに確認する
   - 管理しない場合 → `.gitignore` に `.claude/plans/` を追記する
   - 管理する場合 → 何もしない（チームで plan を共有できる）

**制約（Claude Code 本体の仕様）**：

- `plansDirectory` は **プロジェクトルートからの相対パス**で指定する
- 解決先がプロジェクトルート外になる値（`../` や絶対パス）は**エラーとして無視され**、
  グローバルの `~/.claude/plans/` にフォールバックする
- 未設定時の既定値は `~/.claude/plans/`

### Step 3: ユーザーに確認して調整する

生成後、変更した内容を簡潔に列挙してユーザーに確認を求める。

## CLAUDE.md の設計原則

- **薄く保つ**：本体には `@.claude/xxx.md` のポインタと Quick facts のみ書く
- **直接内容を書かない**：詳細は `.claude/` 配下のファイルに委譲する
- **Quick facts** には変わりにくい事実のみ（言語・DBなど）を書く

## 更新頻度による分類

| ファイル | 更新頻度 | 書くべき内容 |
|---|---|---|
| `architecture.md` | 低（月単位） | 設計方針・技術選定の理由・禁止パターン |
| `coding-style.md` | 低（月単位） | 命名規則・フォーマット・lintルール |
| `workflows.md` | 中（週単位） | ビルド・テスト・デプロイ手順 |
| `current-sprint.md` | 高（日単位） | 今やっていること・一時的な制約 |
| `known-issues.md` | 随時 | ハマりポイント・地雷・回避策 |

## テンプレートの参照

→ `references/templates.md` に各ファイルのテンプレートを収録
