---
name: codex-review
description: |
  OpenAI Codex CLI code review skill for Claude Code.
  Interactive model/depth/scope selection with shortcut mode.
  Triggers: /codex-review, codex review, code review
---

# Codex Review for Claude Code

Prerequisite: [OpenAI Codex CLI](https://github.com/openai/codex) installed and configured.

## Interactive Flow (4 steps, cancel anytime)

### Step 1: Select Model

AskUserQuestion (header: "Model"), 3 options:
1. label: "GPT-5.4 (Recommended)", description: "Latest and most capable"
2. label: "GPT-5.3 Codex", description: "Codex-optimized, stronger code understanding"
3. label: "Cancel", description: "Abort review"

Other: enter `52` for gpt-5.2-codex or `51` for gpt-5.1-codex-max

**If "Cancel" or Other = "cancel/q"** → reply "Cancelled." and stop.

Mapping:
- "GPT-5.4 (Recommended)" → model=gpt-5.4
- "GPT-5.3 Codex" → model=gpt-5.3-codex
- Other "52" → model=gpt-5.2-codex
- Other "51" → model=gpt-5.1-codex-max

### Step 2: Select Reasoning Depth

AskUserQuestion (header: "Depth"), 4 options:
1. label: "Medium", description: "Moderate thinking, faster"
2. label: "High (Recommended)", description: "Deep thinking, balanced"
3. label: "Xhigh", description: "Maximum depth, slowest but most thorough"
4. label: "Cancel", description: "Abort review"

**If "Cancel"** → reply "Cancelled." and stop.

Mapping:
- "Medium" → effort=medium
- "High (Recommended)" → effort=high
- "Xhigh" → effort=xhigh

### Step 3: Select Scope

AskUserQuestion (header: "Scope"), 4 options:
1. label: "Last commit", description: "Review only the latest commit"
2. label: "Uncommitted changes", description: "Review all staged + unstaged + untracked changes"
3. label: "Entire branch", description: "Review all commits vs main branch"
4. label: "Cancel", description: "Abort review"

**If "Cancel"** → reply "Cancelled." and stop.

Mapping:
- "Last commit" → --commit HEAD
- "Uncommitted changes" → --uncommitted
- "Entire branch" → (no flag)

### Step 4: Confirm

AskUserQuestion (header: "Confirm"), 2 options:
1. label: "Start review", description: "{model} + {effort} + {scope_label}"
2. label: "Cancel", description: "Abort review"

**If "Cancel"** → reply "Cancelled." and stop.
**If "Start review"** → execute via Bash with `run_in_background: true`, reply "Codex review started in background ({model} {effort}). You'll be notified when it completes."

## Model Code Reference

**Important: `codex review` does NOT support `-p` profiles. Use `-c` overrides.**

| Code | -c model= | -c model_reasoning_effort= |
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

## Command Template

```bash
codex review -c 'model="{model}"' -c 'model_reasoning_effort="{effort}"' {scope_flag}
```

## Shortcut Mode

Skip all prompts by passing arguments directly:

Format: `/codex-review {code} {scope}`

Scope shortcuts:
- `uc` = --uncommitted
- `head` = --commit HEAD
- (empty) = entire branch

Examples:
- `/codex-review 54x uc` → gpt-5.4 xhigh + uncommitted
- `/codex-review 53h` → gpt-5.3-codex high + entire branch
- `/codex-review 51x head` → gpt-5.1-codex-max xhigh + last commit

Default (scope only, no code): uses 54h
- `/codex-review uc` → gpt-5.4 high + uncommitted
- `/codex-review head` → gpt-5.4 high + last commit
