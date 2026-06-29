# mirror-switch

统一切换包管理工具镜像源的 CLI 工具。通过统一的命令行接口管理多个包管理器的镜像源配置。

## 支持的包管理器

| 名称   | 配置文件路径            | 环境变量               |
| ------ | ----------------------- | ---------------------- |
| uv     | `~/.config/uv/uv.toml` | `UV_DEFAULT_INDEX`     |
| pnpm   | `~/.config/pnpm/rc`    | `pnpm_config_registry` |

## 安装

```bash
# 克隆仓库后将 mirror 放到 PATH 中
chmod +x ./mirror
sudo ln -s "$PWD/mirror" /usr/local/bin/mirror
```

## 使用

```bash
# 列出所有可用镜像
mirror ls

# 查看当前配置（优先级: 环境变量 > 配置文件 > 官方源）
mirror status

# 切换镜像（写入配置文件）
mirror use uv tsinghua
mirror use pnpm npmmirror

# 仅打印 export 命令（不写文件）
mirror use uv tsinghua --env

# 预览切换（不实际写入）
mirror use uv tsinghua --dry-run

# 恢复官方源
mirror reset uv

# 预览恢复
mirror reset uv --dry-run
```

## 配置优先级

`status` 命令按以下顺序检测当前源：

1. **环境变量** — 如果已设置，显示并标记为 `(env)`
2. **配置文件** — 如果文件存在且包含 URL，显示并标记为 `(config)`
3. **官方源** — 以上均未配置时，显示默认官方地址

`use` 默认写入配置文件；`use --env` 仅打印 `export` 命令供用户自行设置。

如果环境变量和配置文件同时存在，环境变量优先生效。

## 添加新的包管理器

编辑两个文件：

1. **`config.json`** — 添加新的顶级键，包含 `env_var`、`config_path` 和 `mirrors`（必须含 `official`）
2. **`mirror`** — 在 `set_mirror()`、`reset_mirror()`、`read_url_from_file()`、`cmd_reset()` 的 dry-run 分支中添加 `elif` 支持

## 许可证

MIT
