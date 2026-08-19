# dLazy PPT Skill

**简体中文** · [English](README_en.md) · [한국어](README_ko.md)

[![文档](https://img.shields.io/badge/%E6%96%87%E6%A1%A3-%E4%BD%BF%E7%94%A8%E6%8C%87%E5%8D%97-111827)](https://dlazyai.github.io/ai-ppt-slides/#/) [![ClawHub](https://img.shields.io/badge/ClawHub-dlazy--ppt-cd3b35)](https://clawhub.ai/dlazyai/dlazy-ppt) [![GitHub stars](https://img.shields.io/github/stars/dlazyai/ai-ppt-slides?style=flat&logo=github&label=stars)](https://github.com/dlazyai/ai-ppt-slides/stargazers) [![GitHub forks](https://img.shields.io/github/forks/dlazyai/ai-ppt-slides?style=flat&logo=github&label=forks)](https://github.com/dlazyai/ai-ppt-slides/forks)

一个 PPT 生成 skill，全部图片由 [dLazy](https://dlazy.com) 生成。可在 Codex、Claude Code、OpenClaw、Hermes Agent 等支持 `SKILL.md` 的 agent 中使用，只需要一个 dLazy API key，不需要另外准备 OpenAI 或其他第三方生图账号。它把文章、报告、论文、课程笔记等内容转换成“整页图片式”的演示文稿：先规划大纲和视觉风格，再生成每页幻灯片图片，最后用本地脚本组装为 `.pptx`。

## 温馨提示

> [!TIP]
> 本 skill 负责从文章、报告、大纲或想法生成图片式 PPT，适合强视觉表达，但页面元素本身不可直接编辑。如果每个文本框、图表都必须单独可编辑，这个 skill 不适用。

生成每页幻灯片都会消耗 dLazy 积分，默认是 2K 16:9、medium 质量；4K 或 high 质量单页更贵。做整套 PPT 前，建议先用样张确认风格，再进入全量生成。

如果你在做 PPT 的过程中遇到了自己喜欢的版式或排版，无论是这个 skill 做出来的，还是从别的地方找到的 PPT 风格图片，都可以让 AI 保存到你的个人风格库（`~/.dlazy-ppt/references/`）里，逐步沉淀自己的风格。个人风格库存放在 skill 安装目录之外，更新或重装 skill 都不会丢失。Skills 本质上是非常个性化的流程，鼓励大家在使用这个 skill 的基础上，按自己的偏好持续调优，让它更适配自己的工作流。

关于 skills 如何设计和使用，可以参考 [good-skill-design.pptx](assets/good-skill-design.pptx)。这个 PPT 也是用本 skill 做的，采用的是手绘技术解释风；内容基于 Claude 在设计 skills 方面的最佳实践文章 [The Complete Guide to Building Skills for Claude](https://resources.anthropic.com/hubfs/The-Complete-Guide-to-Building-Skill-for-Claude.pdf)。祝大家玩得愉快！

## 特点

- 多 agent 可用：支持 Codex、Claude Code、OpenClaw、Hermes Agent 等支持 `SKILL.md` 的环境，各环境行为一致。
- 只配一个 key：所有生图都走 dLazy 的 `gpt-image-2`，配一次 `DLAZY_API_KEY` 即可，无需 base URL、模型名等一堆参数，也不需要在不同 agent 里维护不同的生图账号。
- 稳定的阶段化流程：先确认大纲、页数、视觉风格和样张，再进入整套生成，降低一次生成完整 PPT 时的返工和偏航。
- 不是无脑生成：会先引导你确认 `outline.md`、每页要点、风格方向和样张效果，再按确认后的方案继续。
- 低门槛输入：文章、报告、论文、课程笔记、Markdown、大纲、PDF、Word 等材料都可以作为起点。
- 内置 12 种 PPT 风格参考：包括清爽专业、科研答辩、党政红、教学课件、电子墨水杂志、手绘技术解释、仪表盘、麦肯锡等；不会写提示词也可以先从内置风格开始，尤其推荐手绘技术解释风。
- 支持自定义风格复刻：可以上传喜欢的图片、PDF 或 PPT/PPTX，让 agent 先分析配色、版式、字体和视觉元素，再按该风格生成新 PPT。
- 可沉淀个人风格库：生成满意后，可以把当前风格保存到个人风格库（`~/.dlazy-ppt/references/`），下次直接复用；风格库存放在 skill 安装目录之外，更新 skill 不会丢失，同名时个人风格优先于内置风格。
- 多 agent 并发生成：样张确认后，支持一个子 agent 负责一页，并对文字清晰度、风格一致性和内容完整性做自检，发现问题及时返修。
- 支持指定图片插入：可以要求某一页必须放入论文原图、实验结果图、截图、架构图等素材，并让页面围绕这些图片适配主题和版式。
- 自动生成演讲稿：会生成 `speech.md`，并在组装 PPTX 时写入每页备注，方便直接演示或二次修改。

## 生成效果

下面是一套技术分享 PPT 的生成效果示例。每页都是由 `gpt-image-2` 生成的完整 16:9 幻灯片图片，再由本地脚本组装为 PPTX。

![生成 PPT 效果示例](assets/slides_example.png)

下面是一套论文答辩风案例，来源于论文 [Attention Is All You Need](https://arxiv.org/abs/1706.03762)。它展示了如何在指定页中插入论文原始图片作为输入素材，例如模型架构图、attention 模块图和 attention 可视化图，并围绕这些图片生成统一风格的 PPT（见 Issue #14）。

![论文原图插入案例](assets/paper-figures-example.png)

## 风格示例

以下是已生成预览图的风格，示例图均由 `gpt-image-2` 生成，用于帮助用户在开始制作前选择视觉方向。

| 清爽专业风 | 创意杂志风 |
| --- | --- |
| ![清爽专业风](assets/style-previews/clean-professional.png) | ![创意杂志风](assets/style-previews/creative-magazine.png) |
| 电子墨水杂志风 | 数据仪表盘风 |
| ![电子墨水杂志风](assets/style-previews/e-ink-magazine.png) | ![数据仪表盘风](assets/style-previews/data-dashboard.png) |
| 复古扁平插画风 | 手绘技术解释风 |
| ![复古扁平插画风](assets/style-previews/retro-flat-illustration.png) | ![手绘技术解释风](assets/style-previews/handdrawn-technical.png) |
| 手绘白板风 | 温暖手工风 |
| ![手绘白板风](assets/style-previews/handdrawn-whiteboard.png) | ![温暖手工风](assets/style-previews/warm-handmade.png) |
| 科研答辩风 | 麦肯锡风格 |
| ![科研答辩风](assets/style-previews/scientific-defense.png) | ![麦肯锡风格](assets/style-previews/mckinsey-style.png) |
| 党政红风格 | 教学课件风 |
| ![党政红风格](assets/style-previews/party-government-red.png) | ![教学课件风](assets/style-previews/teaching-courseware.png) |

## 输出结构

每个 PPT 会生成一个独立项目目录：

```text
{基础目录}/{PPT名称}/        # 当前 PPT 的独立项目目录
├── origin_image/           # 正式幻灯片图片目录，只放最终采用的页面
│   ├── slide_01.png        # 第 1 页幻灯片图片
│   ├── slide_02.png        # 第 2 页幻灯片图片
│   └── ...                 # 后续页面图片，按页码顺序命名
├── outline.md              # 经确认的 PPT 大纲、页数、每页标题和要点
├── speech.md               # 演讲稿，会写入 PPT 每页备注
└── {PPT名称}.pptx          # 最终组装生成的 PowerPoint 文件
```

你可以在 `origin_image/` 里查看每一页最终采用的幻灯片图片，文件会按 `slide_01.png`、`slide_02.png` 这样的顺序排列。想预览整套 PPT 的视觉效果，或只挑某一页继续修改时，直接看这里最方便。

`speech.md` 是配套演讲稿。生成 `.pptx` 时，这些内容会自动写入每页 PPT 的备注区，你可以在 PowerPoint 里直接查看、修改，或演示时作为讲稿使用。

## 适用场景

- 技术文章转分享 PPT
- 论文或报告转演示稿
- 课程笔记转课件
- 科研项目申报、中期检查、结题验收和论文答辩
- 商业汇报、产品介绍、调研总结
- 需要强视觉统一性的图片式演示文稿

## 安装

### 一句话安装

【推荐】可以直接把下面这句话发给你的 Agent，让它帮你安装：

```text
请帮我安装这个 dlazy-ppt skill，链接是：https://github.com/dlazyai/ai-ppt-slides
```

### 手动安装到 Codex

如需手动安装到 Codex，可以使用 `skills` CLI 安装到 Codex 的全局 skills 目录：

```bash
npx -y skills@latest add dlazyai/ai-ppt-slides \
  --skill dlazy-ppt \
  --agent codex \
  --global
```

安装完成后，重启 Codex 让新 skill 生效。

也可以从 GitHub Releases 下载 `dlazy-ppt-v*.zip`，解压后把其中的 `dlazy-ppt` 文件夹放到 `~/.codex/skills/dlazy-ppt`，然后重启 Codex。

如果你是在本地开发这个仓库，也可以把 skill 目录链接到 Codex skills 目录，方便实时调试修改：

```bash
mkdir -p ~/.codex/skills
ln -s /path/to/ai-ppt-slides/skills/dlazy-ppt ~/.codex/skills/dlazy-ppt
```

### OpenClaw

可以通过 ClawHub 安装：

```bash
openclaw skills install dlazy-ppt
```

ClawHub 页面：[clawhub.ai/dlazyai/dlazy-ppt](https://clawhub.ai/dlazyai/dlazy-ppt)

如果使用 OpenClaw 的 skill allowlist，需要把 `dlazy-ppt` 加入允许列表。

### Claude Code、Hermes Agent

这些 agent 都可以读取 `SKILL.md` 形式的 skill。也可以使用 `skills` CLI 安装：

```bash
# Claude Code
npx -y skills@latest add dlazyai/ai-ppt-slides \
  --skill dlazy-ppt \
  --agent claude-code \
  --global

# Hermes Agent
npx -y skills@latest add dlazyai/ai-ppt-slides \
  --skill dlazy-ppt \
  --agent hermes-agent \
  --global
```

常见目标目录是：Claude Code 使用 `~/.claude/skills/dlazy-ppt`，Hermes Agent 使用 `~/.hermes/skills/dlazy-ppt`。

如果你是在本地开发这个仓库，也可以用软链接替代复制，方便实时调试修改。

### 更新

重新执行一遍上面对应的安装命令即可覆盖为最新版本，也可以直接让 agent 帮你更新：

```text
请帮我更新 dlazy-ppt skill 到最新版本，仓库是：https://github.com/dlazyai/ai-ppt-slides
```

更新后重启 agent 生效。API key 配置（`~/.dlazy-ppt/.env`）和个人风格库（`~/.dlazy-ppt/references/`）都在 skill 安装目录之外，更新或重装不会丢失。

## 配置 dLazy API key

生图需要一个 dLazy API key，配置一次即可，所有 agent 共用：

1. 登录 [dlazy.com](https://dlazy.com)，在 [API Key 页面](https://dlazy.com/dashboard/organization/api-key)复制你的 key。
2. 执行：

```bash
python3 skills/dlazy-ppt/scripts/dlazy_ppt_runtime.py config --api-key "你的-dlazy-api-key"
```

也可以直接把 key 发给 agent，让它帮你写入。

配置会写到 `~/.dlazy-ppt/.env`（权限 0600），Codex、Claude Code、OpenClaw、Hermes Agent 都读这一份，更新或重装 skill 不会丢失。

验证配置是否可用：

```bash
python3 skills/dlazy-ppt/scripts/dlazy_ppt_runtime.py doctor --check-api
```

更多细节（自建部署的 base URL、换用其他 dLazy 生图模型）见[生图模型配置指南](skills/dlazy-ppt/docs/image-model-configuration.md)。

## 使用方式

在 Codex、Claude Code、OpenClaw 或 Hermes Agent 中明确指定使用 `dlazy-ppt` skill，例如：

```text
请使用 dlazy-ppt skill 把 /path/to/article.md 做成 10 页左右的 PPT。
```

skill 会按以下流程执行：

1. 阅读内容并规划 PPT 大纲
2. 生成 `outline.md`，并请求你确认页数、标题和每页要点
3. 给出 2-3 个视觉风格选项，并推荐一个让用户确认
4. 检查 dLazy 配置是否就绪
5. 生成 1 页样张，让用户确认风格、版式节奏和文字质量
6. 创建 PPT 项目目录
7. 逐页生成全部幻灯片图片
8. 检查文字清晰度、风格一致性和内容完整性
9. 生成 `speech.md`
10. 使用 `assemble_ppt.py` 组装 `.pptx`
11. 可选：如果生成的 PPT 风格你很喜欢，可以保存到风格库；如果使用的是内置风格，则无需重复保存

## 使用技巧

- 默认分辨率是 2K 16:9 横屏（`2048x1152`）。如果生成的幻灯片图片比较模糊，尤其是文字较多的页面，可以让 AI 改用 4K（`3840x2160`）加 `high` 质量重新生成——效果更清晰，但单页积分消耗明显更高。可用尺寸只有固定的几档，`2048x1152` 和 `3840x2160` 是仅有的两个 16:9 选项。
- 如果只是不满意某一页的内容、排版、配色或文字表达，可以直接让当前 agent 针对这一页做细致修改，不需要整套 PPT 重新生成。

![单页局部修改示意：打开 PPT、点击标注，并框选需要修改的位置](assets/single-slide-revision-example.png)

- 你也可以提供喜欢的 PPT 风格参考，可以是一张截图、多张截图，或完整 PPT/PDF。建议先让当前 agent 分析参考材料的配色、版式、字体和视觉元素，再按这个风格生成新 PPT。生成满意后，也可以让 agent 把这套风格保存到个人风格库（`~/.dlazy-ppt/references/`）里，方便以后复用，且不会因更新 skill 而丢失。
- 如果需要插入论文原图、实验结果图、截图或架构图，可以在大纲中指定这些图片对应的页码和用途。

## 支持

遇到问题？请查看[使用文档](https://dlazyai.github.io/ai-ppt-slides/#/)，或[提交 Issue](https://github.com/dlazyai/ai-ppt-slides/issues/new)。

## 许可证

MIT

## 致谢

本项目基于 [ningzimu/codex-ppt-skill](https://github.com/ningzimu/codex-ppt-skill) 改造，把生图链路整体切换到 dLazy，并去掉了多后端选择。感谢原作者的工作，也感谢 [LinuxDO](https://linux.do) 社区的支持。
