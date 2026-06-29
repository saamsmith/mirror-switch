# HISTORY

## 2026-06-30 01:25 — MIMO

- 添加 Homebrew 镜像源支持（ustc / tsinghua / aliyun / tencent）
  - 支持 4 个环境变量: HOMEBREW_BOTTLE_DOMAIN, HOMEBREW_API_DOMAIN, HOMEBREW_BREW_GIT_REMOTE, HOMEBREW_CORE_GIT_REMOTE
  - 新增 `env_vars` 配置格式，每个镜像可提供多个环境变量
  - brew use/reset 命令始终走 export/unset 路径（无配置文件）
