# 安装与配置

## 一句话安装

推荐直接把下面这句话发给 Codex，让它帮你安装：

```text
请帮我安装这个 dlazy-ppt skill，链接是：https://github.com/dlazyai/ai-ppt-slides
```

## Codex 手动安装

在命令行中执行以下命令，将 `dlazy-ppt` skill 安装到 Codex 全局 skills 目录：

```bash
npx -y skills@latest add dlazyai/ai-ppt-slides \
  --skill dlazy-ppt \
  --agent codex \
  --global
```

安装后重启 Codex，让新 skill 生效。

也可以从 [GitHub Releases](https://github.com/dlazyai/ai-ppt-slides/releases) 下载 `dlazy-ppt-v*.zip`，解压后把其中的 `dlazy-ppt` 文件夹放到 `~/.codex/skills/dlazy-ppt`，然后重启 Codex。

如果你在本地开发这个仓库，可以把 skill 目录软链接到 Codex skills 目录，方便实时调试修改：

```bash
mkdir -p ~/.codex/skills
ln -s /path/to/ai-ppt-slides/skills/dlazy-ppt ~/.codex/skills/dlazy-ppt
```

## OpenClaw 安装

```bash
openclaw skills install dlazy-ppt
```

如果使用 OpenClaw 的 skill allowlist，需要把 `dlazy-ppt` 加入允许列表。

## Claude Code / Hermes Agent

Claude Code：

```bash
npx -y skills@latest add dlazyai/ai-ppt-slides \
  --skill dlazy-ppt \
  --agent claude-code \
  --global
```

Hermes Agent：

```bash
npx -y skills@latest add dlazyai/ai-ppt-slides \
  --skill dlazy-ppt \
  --agent hermes-agent \
  --global
```

常见目标目录：Claude Code 使用 `~/.claude/skills/dlazy-ppt`，Hermes Agent 使用 `~/.hermes/skills/dlazy-ppt`。本地开发时同样可以用软链接替代复制。

## 更新 skill

推荐直接把下面这句话发给你的 agent，让它帮你更新：

```text
请帮我更新 dlazy-ppt skill 到最新版本，仓库是：https://github.com/dlazyai/ai-ppt-slides
```

手动更新时，重新执行上面对应 agent 的安装命令即可，会用最新版本覆盖已安装的 skill；也可以从 [GitHub Releases](https://github.com/dlazyai/ai-ppt-slides/releases) 下载最新的 `dlazy-ppt-v*.zip`，解压后替换原来的 `dlazy-ppt` 目录。更新完成后重启 agent 生效。

更新是安全的：API key 等运行时配置保存在 `~/.dlazy-ppt/.env`，个人风格库保存在 `~/.dlazy-ppt/references/`，都在 skill 安装目录之外，更新或重装不会丢失。每个版本的变更内容可以查看 [Releases 页面](https://github.com/dlazyai/ai-ppt-slides/releases)或仓库的 `CHANGELOG.md`。

## dLazy API key 配置

生图需要一个 dLazy API key，配置一次即可，所有 agent 共用。

1. 登录 [dlazy.com](https://dlazy.com)，在 [API Key 页面](https://dlazy.com/dashboard/organization/api-key)复制 key。
2. 执行：

```bash
python3 {skill_root}/scripts/dlazy_ppt_runtime.py config --api-key "你的-dlazy-api-key"
```

也可以直接把 key 交给 agent，让它帮你写入。

配置写在 `~/.dlazy-ppt/.env`（权限 0600），Codex、Claude Code、OpenClaw、Hermes Agent 共用同一份，更新或重装 skill 都不会丢失。

## 验证配置

```bash
python3 {skill_root}/scripts/dlazy_ppt_runtime.py doctor --check-api
```

这个命令会检查共享运行时、打印当前配置，并拉取账号的工具清单确认 key 有效、模型可用。

- `HTTP 401`：key 无效或已吊销，去 API Key 页面重新生成。
- `insufficient_balance`：组织积分不足，在[积分页面](https://dlazy.com/dashboard/organization/settings?tab=credits)充值。
- 提示模型不在清单里：该账号没有这个生图工具的权限。

## 可选配置

- `DLAZY_BASE_URL`：仅自建部署需要，默认 `https://dlazy.com`。
- `DLAZY_PPT_IMAGE_MODEL`：换用其他 dLazy 生图工具，默认 `gpt-image-2`。

两者都可以用同一个 `config` 命令写入：

```bash
python3 {skill_root}/scripts/dlazy_ppt_runtime.py config \
  --api-key "你的-dlazy-api-key" \
  --base-url "https://dlazy.example.com" \
  --model gpt-image-2
```
