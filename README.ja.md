# code-project-init

[English](README.md) | 日本語

プロジェクトに `CLAUDE.md` と `.claude/` ディレクトリ構成をセットアップする [Claude Code](https://claude.com/claude-code) 用スキルです。

`CLAUDE.md` は薄く保ち、詳細は更新頻度ごとに分けたファイルへ委譲します。

## 生成する構成

```
project/
├── CLAUDE.md                        # エントリポイント（@インポートと Quick facts のみ）
└── .claude/
    ├── settings.json                # plansDirectory を .claude/plans に設定
    ├── architecture.md              # アーキテクチャ方針（変化少ない）
    ├── coding-style.md              # コーディング規約（変化少ない）
    ├── workflows.md                 # よく使うコマンド・手順
    ├── context/
    │   ├── current-sprint.md        # 今やっていること（頻繁に変わる）
    │   └── known-issues.md          # 既知の問題・地雷（随時追記）
    ├── plans/                       # プランモードの出力先
    └── skills/
        └── session-record/
            └── SKILL.md             # セッション記録スキル
```

| ファイル | 更新頻度 | 書くべき内容 |
|---|---|---|
| `architecture.md` | 低（月単位） | 設計方針・技術選定の理由・禁止パターン |
| `coding-style.md` | 低（月単位） | 命名規則・フォーマット・lintルール |
| `workflows.md` | 中（週単位） | ビルド・テスト・デプロイ手順 |
| `current-sprint.md` | 高（日単位） | 今やっていること・一時的な制約 |
| `known-issues.md` | 随時 | ハマりポイント・地雷・回避策 |

## 機能

- **新規プロジェクト**：ユーザーへの確認やリポジトリの内容からプロジェクト情報を集め、テンプレートに沿って各ファイルを生成します。
- **肥大化した既存 `CLAUDE.md`**：内容を各ファイルに振り分け、その結果を確認してもらってから生成します。生成後、`CLAUDE.md` をポインタ形式に書き換えます。
- **`plansDirectory` 設定**：`.claude/settings.json` に `.claude/plans` を設定し、plan ファイルをグローバルの `~/.claude/plans/` ではなくプロジェクト内に保存します。既存のキーは変更しません。
- **`session-record` スキル**：「セッション終了して」などの指示で `current-sprint.md` と `known-issues.md` を更新し、その2ファイルだけをコミットするプロジェクト用スキルを配置します。

## インストール

### プラグインとして（推奨）

Claude Code で以下を実行します。

```
/plugin marketplace add surface0/code-project-init
/plugin install code-project-init@code-project-init
```

### 手動

スキルのディレクトリを個人用スキルフォルダにコピーします。

```bash
git clone https://github.com/surface0/code-project-init.git
cp -r code-project-init/skills/code-project-init ~/.claude/skills/
```

## 使い方

プロジェクトのディレクトリで Claude Code に次のように依頼します。

- 「CLAUDE.mdを作って」
- 「.claude ディレクトリを作りたい」
- 「プロジェクトにClaudeの設定を追加したい」

## ライセンス

[MIT](LICENSE)
