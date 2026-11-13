# code-project-init

English | [日本語](README.ja.md)

A [Claude Code](https://claude.com/claude-code) skill that sets up `CLAUDE.md` and a structured `.claude/` directory for your project.

Instead of one large `CLAUDE.md`, this skill keeps `CLAUDE.md` short and moves the details into separate files grouped by how often they change.

> **Note:** The skill's instructions and generated templates are written in Japanese. Claude can still follow them in any language, but the generated files start with Japanese headings.

## What it generates

```
project/
├── CLAUDE.md                        # Entry point (kept thin: @-imports + quick facts)
└── .claude/
    ├── settings.json                # plansDirectory -> .claude/plans
    ├── architecture.md              # Architecture policy (rarely changes)
    ├── coding-style.md              # Coding conventions (rarely changes)
    ├── workflows.md                 # Common commands / procedures
    ├── context/
    │   ├── current-sprint.md        # What you're working on now (changes often)
    │   └── known-issues.md          # Pitfalls and workarounds (appended as found)
    ├── plans/                       # Plan mode output
    └── skills/
        └── session-record/
            └── SKILL.md             # Skill to record session results into context/
```

| File | Update frequency | Content |
|---|---|---|
| `architecture.md` | Low (monthly) | Design policy, why each technology was chosen, patterns to avoid |
| `coding-style.md` | Low (monthly) | Naming, formatting, lint rules |
| `workflows.md` | Medium (weekly) | Build / test / deploy steps |
| `current-sprint.md` | High (daily) | Current tasks, temporary constraints |
| `known-issues.md` | As needed | Pitfalls, workarounds |

## Features

- **New projects**: collects project info from you or the repository and fills in the files from templates.
- **Existing projects with a large `CLAUDE.md`**: sorts its content into the files above and shows you the result before writing anything, then rewrites `CLAUDE.md` as a list of pointers to those files.
- **`plansDirectory`**: sets `.claude/plans` in `.claude/settings.json` so plan files stay inside the project instead of the global `~/.claude/plans/`. Other keys already in the file are left as they are.
- **`session-record` skill**: adds a project skill that updates `current-sprint.md` / `known-issues.md` and commits only those two files when you say things like "end session" or "record the session".

## Installation

### As a plugin (recommended)

In Claude Code:

```
/plugin marketplace add surface0/code-project-init
/plugin install code-project-init@code-project-init
```

### Manual

Copy the skill directory into your personal skills folder:

```bash
git clone https://github.com/surface0/code-project-init.git
cp -r code-project-init/skills/code-project-init ~/.claude/skills/
```

## Usage

In a project directory, ask Claude Code something like:

- "Create a CLAUDE.md for this project"
- "Set up the .claude directory"
- "CLAUDE.mdを作って"

## Repository layout

```
.claude-plugin/
├── marketplace.json
└── plugin.json
skills/
└── code-project-init/
    ├── SKILL.md
    ├── references/templates.md            # Templates for each generated file
    └── assets/session-record/SKILL.md.tmpl
```

## License

[MIT](LICENSE)
