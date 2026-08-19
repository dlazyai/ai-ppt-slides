# Installation and Configuration

## One-Sentence Installation

The recommended approach is to send the following sentence directly to Codex and let it install the skill for you:

```text
Please install this dlazy-ppt skill for me: https://github.com/dlazyai/ai-ppt-slides
```

## Manual Installation for Codex

Run the following command to install the `dlazy-ppt` skill in Codex's global skills directory:

```bash
npx -y skills@latest add dlazyai/ai-ppt-slides \
  --skill dlazy-ppt \
  --agent codex \
  --global
```

Restart Codex after installation so the new skill takes effect.

You can also download `ai-ppt-slides-v*.zip` from [GitHub Releases](https://github.com/dlazyai/ai-ppt-slides/releases), extract it, place the included `dlazy-ppt` folder at `~/.codex/skills/dlazy-ppt`, and restart Codex.

If you are developing this repository locally, you can symlink the skill directory into the Codex skills directory for real-time testing:

```bash
mkdir -p ~/.codex/skills
ln -s /path/to/ai-ppt-slides/skills/dlazy-ppt ~/.codex/skills/dlazy-ppt
```

## OpenClaw Installation

```bash
openclaw skills install dlazy-ppt
```

If you use OpenClaw's skill allowlist, add `dlazy-ppt` to the allowlist.

## Claude Code / Hermes Agent

Claude Code:

```bash
npx -y skills@latest add dlazyai/ai-ppt-slides \
  --skill dlazy-ppt \
  --agent claude-code \
  --global
```

Hermes Agent:

```bash
npx -y skills@latest add dlazyai/ai-ppt-slides \
  --skill dlazy-ppt \
  --agent hermes-agent \
  --global
```

Common destination directories are `~/.claude/skills/dlazy-ppt` for Claude Code and `~/.hermes/skills/dlazy-ppt` for Hermes Agent. During local development, you can likewise use a symlink instead of copying the directory.

## Updating the Skill

The recommended approach is to send the following sentence directly to your agent and let it update the skill for you:

```text
Please update the dlazy-ppt skill to the latest version. The repository is: https://github.com/dlazyai/ai-ppt-slides
```

For a manual update, rerun the installation command above for the relevant agent. This overwrites the installed skill with the latest version. Alternatively, download the latest `ai-ppt-slides-v*.zip` from [GitHub Releases](https://github.com/dlazyai/ai-ppt-slides/releases), extract it, and replace the existing `dlazy-ppt` directory. Restart the agent after the update.

Updates are safe: runtime configuration such as API keys is stored in `~/.dlazy-ppt/.env`, while your personal style library is stored in `~/.dlazy-ppt/references/`. Both are outside the skill installation directory, so updating or reinstalling the skill will not remove them. See the [Releases page](https://github.com/dlazyai/ai-ppt-slides/releases) or the repository's `CHANGELOG.md` for the changes in each version.

## dLazy API Key Configuration

Image generation requires a dLazy API key. Configure it once and every agent shares it.

1. Sign in at [dlazy.com](https://dlazy.com) and copy your key from the [API key page](https://dlazy.com/dashboard/organization/api-key).
2. Run:

```bash
python3 {skill_root}/scripts/dlazy_ppt_runtime.py config --api-key "your-dlazy-api-key"
```

You can also hand the key to your agent and ask it to save it.

The config lives in `~/.dlazy-ppt/.env` with mode `0600`. Codex, Claude Code, OpenClaw, and Hermes Agent all read that one file, and updating or reinstalling the skill never loses it.

## Verifying The Configuration

```bash
python3 {skill_root}/scripts/dlazy_ppt_runtime.py doctor --check-api
```

This checks the shared runtime, prints the effective configuration, and fetches the account's tool manifest to confirm the key works and the model is available.

- `HTTP 401`: the key is invalid or revoked; issue a new one on the API key page.
- `insufficient_balance`: the organization is out of credits; top up on the [credits page](https://dlazy.com/dashboard/organization/settings?tab=credits).
- Model reported as not in the manifest: that image tool is not available to this account.

## Optional Settings

- `DLAZY_BASE_URL`: only for self-hosted deployments; defaults to `https://dlazy.com`.
- `DLAZY_PPT_IMAGE_MODEL`: use a different dLazy image tool; defaults to `gpt-image-2`.

Both are written with the same `config` command:

```bash
python3 {skill_root}/scripts/dlazy_ppt_runtime.py config \
  --api-key "your-dlazy-api-key" \
  --base-url "https://dlazy.example.com" \
  --model gpt-image-2
```
