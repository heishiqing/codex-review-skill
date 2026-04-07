---
name: codex-review
description: |
  OpenAI Codex CLI 代码审查 Skill。
  交互式选择模型/思考深度/审查范围，无需记命令。
  触发词：/codexskill, codex review, 代码审查, 审查代码, review code
---

# Codex Review for Claude Code

前置条件：已安装 [OpenAI Codex CLI](https://github.com/openai/codex)。

## 交互流程（4 步，每步可取消）

### Step 1：选择模型

AskUserQuestion（header: "模型"），3 个选项：
1. label: "GPT-5.4（推荐）", description: "最新最强，日常审查首选"
2. label: "GPT-5.3 Codex", description: "Codex优化版，代码理解更强"
3. label: "取消审查", description: "放弃本次审查"

Other：输入 `52` 使用 gpt-5.2-codex，`51` 使用 gpt-5.1-codex-max

**如果选"取消审查"或 Other 输入"取消/cancel/q"** → 回复"已取消。"并结束。

选项映射：
- "GPT-5.4（推荐）" → model=gpt-5.4
- "GPT-5.3 Codex" → model=gpt-5.3-codex
- Other "52" → model=gpt-5.2-codex
- Other "51" → model=gpt-5.1-codex-max

### Step 2：选择思考深度

AskUserQuestion（header: "深度"），4 个选项：
1. label: "Medium", description: "中等思考，速度优先"
2. label: "High（推荐）", description: "高思考，平衡深度和速度"
3. label: "Xhigh", description: "极深思考，最仔细但最慢"
4. label: "取消审查", description: "放弃本次审查"

**如果选"取消审查"** → 回复"已取消。"并结束。

选项映射：
- "Medium" → effort=medium
- "High（推荐）" → effort=high
- "Xhigh" → effort=xhigh

### Step 3：选择审查范围

AskUserQuestion（header: "范围"），4 个选项：
1. label: "最近一次 commit", description: "只检查最新一次提交的改动"
2. label: "未提交改动", description: "检查还没 commit 的所有改动"
3. label: "整个分支", description: "当前分支相对于 main 的所有 commit 改动"
4. label: "取消审查", description: "放弃本次审查"

**如果选"取消审查"** → 回复"已取消。"并结束。

范围映射：
- "最近一次 commit" → --commit HEAD
- "未提交改动" → --uncommitted
- "整个分支" → （不加参数）

### Step 4：确认执行

AskUserQuestion（header: "确认"），2 个选项：
1. label: "开始审查", description: "{model} + {effort} + {scope_label}"
2. label: "取消", description: "放弃本次审查"

**如果选"取消"** → 回复"已取消。"并结束。
**如果选"开始审查"** → 用 Bash 执行（`run_in_background: true`），回复"Codex 审查已在后台启动（{model} {effort}），完成后会通知你。"

## 代号 → 模型参数映射表

**重要：`codex review` 不支持 `-p` profile 参数！必须用 `-c` 覆盖方式。**

| 代号 | -c model= | -c model_reasoning_effort= |
|------|-----------|---------------------------|
| 54m | gpt-5.4 | medium |
| 54h | gpt-5.4 | high |
| 54x | gpt-5.4 | xhigh |
| 53m | gpt-5.3-codex | medium |
| 53h | gpt-5.3-codex | high |
| 53x | gpt-5.3-codex | xhigh |
| 52m | gpt-5.2-codex | medium |
| 52h | gpt-5.2-codex | high |
| 52x | gpt-5.2-codex | xhigh |
| 51m | gpt-5.1-codex-max | medium |
| 51h | gpt-5.1-codex-max | high |
| 51x | gpt-5.1-codex-max | xhigh |

## 执行命令

```bash
codex review -c 'model="{model}"' -c 'model_reasoning_effort="{effort}"' {scope_flag}
```

## 快捷模式

跳过所有提问，直接传参：

格式：`/codexskill {代号} {范围}`

范围缩写：
- `uc` = --uncommitted（未提交改动）
- `head` = --commit HEAD（最近一次 commit）
- 不写 = 整个分支

示例：
- `/codexskill 54x uc` → gpt-5.4 xhigh + 未提交改动
- `/codexskill 53h` → gpt-5.3-codex high + 整个分支
- `/codexskill 51x head` → gpt-5.1-codex-max xhigh + 最近一次提交

只写范围不写代号时，默认 54h：
- `/codexskill uc` → gpt-5.4 high + 未提交改动
