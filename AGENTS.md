# AGENTS.md — mirror-switch

## Project structure

- `mirror` — single-file Python CLI (no deps beyond stdlib), shebang `#!/usr/bin/env python3`
- `config.json` — mirror definitions for each package manager

No test framework, no lint/type config, no build step. Run directly: `./mirror <command>`.

## Adding a new package manager

Two files to touch:

1. **`config.json`** — add a new top-level key with `env_var`, `config_path`, and `mirrors` (must include `"official"`).
2. **`mirror`** — add `elif` branches in all three format-specific functions:
   - `set_mirror()` — write the mirror URL to `config_path` in the correct format
   - `reset_mirror()` — undo what `set_mirror()` did
   - `read_url_from_file()` — parse `config_path` to extract current URL
   - `cmd_reset()` dry-run block — add `elif` for the new PM's reset message

## Configuration priority rule

`status` checks in order: **env var → config file → official default**.
`use` defaults to writing config file; `use --env` prints an `export` command instead.
If both env var and config file are present, env var wins (not overridden by the tool).

## Config format requirements per PM

Each PM's `config_path` has a different expected format — match exactly:

- **uv** (`~/.config/uv/uv.toml`): TOML, `[[index]]\nurl = "..."\ndefault = true`
- **pnpm** (`~/.config/pnpm/rc`): INI, `registry=...`

## Git policy

Only push to GitHub when the user explicitly says to. Do not auto-sync after changes.

## HISTORY.md 更新规则

每次 build 完成后必须更新 `HISTORY.md`，格式：

```markdown
## YYYY-MM-DD HH:MM — 修改人 [build]

- 修改内容描述
  - 详细说明
```

要求：
- 时间必须查询系统当前准确时间，不可随意填写
- 修改人填写实际执行修改的 agent 名称（如 MIMO）
- 每条变更独立列出，包含具体改了什么

## Git 提交与历史记录

每次 git commit 时同步更新 `HISTORY.md`，格式同上，后缀改为 `[git]`，内容为 git commit message。

```markdown
## YYYY-MM-DD HH:MM — 修改人 [git]

- commit message 内容
```

## Commands reference

```
mirror ls                           # list available mirrors
mirror status                       # show current config (env > config > default)
mirror use <pm> <mirror>            # switch (writes config file)
mirror use <pm> <mirror> --env      # switch (prints export command only)
mirror use <pm> <mirror> --dry-run  # preview without writing
mirror reset <pm>                   # restore official source
mirror reset <pm> --dry-run         # preview reset
```
