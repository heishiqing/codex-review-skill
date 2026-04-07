# Codex Review Skill for Claude Code

**中文** | **[English](./README.md)**

> 把 OpenAI Codex CLI 的代码审查变成无需记命令的交互式体验。

## 它做什么

不用再记 `codex review -c 'model="gpt-5.4"' -c 'model_reasoning_effort="high"' --uncommitted` 这种长命令。只需输入 `/codex-review`，然后从菜单选：

```
第 1 步: 选模型     → GPT-5.4 / GPT-5.3 Codex
第 2 步: 选深度     → Medium / High / Xhigh
第 3 步: 选范围     → 最近一次提交 / 未提交改动 / 整个分支
第 4 步: 确认       → 开始 / 取消
```

审查在后台运行，完成后自动通知。

## 功能特点

- **不需要记命令** — 交互菜单引导你完成每一步
- **随时取消** — 每一步都有取消按钮
- **快捷模式** — 老手可以跳过菜单：`/codex-review 54h uc`
- **后台执行** — 审查异步运行，不阻塞工作流
- **支持所有模型** — GPT-5.4、5.3-codex、5.2-codex、5.1-codex-max
- **三种思考深度** — Medium（快速）、High（平衡）、Xhigh（最仔细）

## 前置条件

1. [Claude Code](https://claude.ai/code) 已安装
2. [OpenAI Codex CLI](https://github.com/openai/codex) 已安装：
   ```bash
   npm install -g @openai/codex
   ```

## 安装

```bash
# 克隆到 Claude Code skills 目录
git clone https://github.com/heishiqing/codex-review-skill.git ~/.claude/skills/codex-review
```

或手动下载 `SKILL.md`：

```bash
mkdir -p ~/.claude/skills/codex-review
curl -o ~/.claude/skills/codex-review/SKILL.md \
  https://raw.githubusercontent.com/heishiqing/codex-review-skill/main/SKILL.md
```

## 使用方法

### 交互模式

在 Claude Code 中输入 `/codex-review`，按 4 步向导操作。

### 快捷模式

跳过向导，直接传入模型代号 + 范围：

```
/codex-review 54h uc       # GPT-5.4 High + 未提交改动
/codex-review 53x head     # GPT-5.3 Codex Xhigh + 最近一次提交
/codex-review uc            # 默认 (GPT-5.4 High) + 未提交改动
```

### 模型代号速查

| 代号 | 模型 | 思考深度 |
|------|------|----------|
| `54m` `54h` `54x` | gpt-5.4 | 中 / 高 / 极深 |
| `53m` `53h` `53x` | gpt-5.3-codex | 中 / 高 / 极深 |
| `52m` `52h` `52x` | gpt-5.2-codex | 中 / 高 / 极深 |
| `51m` `51h` `51x` | gpt-5.1-codex-max | 中 / 高 / 极深 |

## 工作原理

这个 Skill 教会 Claude Code 如何：
1. 通过 `AskUserQuestion` 展示交互选择菜单
2. 将你的选择映射到正确的 `codex review -c` 参数
3. 在后台运行审查命令
4. 完成后通知你

实际审查由 OpenAI Codex CLI 执行 — 这个 Skill 只是 UI 层。

## License

MIT
