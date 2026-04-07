# Codex Review Skill for Claude Code

A [Claude Code](https://claude.ai/code) skill that provides an interactive UI for running [OpenAI Codex CLI](https://github.com/openai/codex) code reviews.

## Features

- Interactive 4-step wizard: Model > Depth > Scope > Confirm
- Cancel at any step
- Shortcut mode for power users (`/codex-review 54h uc`)
- Runs review in background, notifies on completion
- Supports all Codex models (GPT-5.4, 5.3-codex, 5.2-codex, 5.1-codex-max)
- Three reasoning depths: Medium, High, Xhigh

## Prerequisites

1. [Claude Code](https://claude.ai/code) installed
2. [OpenAI Codex CLI](https://github.com/openai/codex) installed and configured:
   ```bash
   npm install -g @openai/codex
   ```

## Installation

Copy `SKILL.md` to your Claude Code skills directory:

```bash
mkdir -p ~/.claude/skills/codex-review
cp SKILL.md ~/.claude/skills/codex-review/
```

Or clone this repo:

```bash
git clone https://github.com/heishiqing/codex-review-skill.git ~/.claude/skills/codex-review
```

## Usage

### Interactive Mode

Type `/codex-review` in Claude Code and follow the prompts.

### Shortcut Mode

```
/codex-review 54h uc      # GPT-5.4 High + uncommitted changes
/codex-review 53x head    # GPT-5.3-codex Xhigh + last commit
/codex-review uc           # Default (GPT-5.4 High) + uncommitted
```

### Model Codes

| Code | Model | Reasoning |
|------|-------|-----------|
| 54m/h/x | gpt-5.4 | medium/high/xhigh |
| 53m/h/x | gpt-5.3-codex | medium/high/xhigh |
| 52m/h/x | gpt-5.2-codex | medium/high/xhigh |
| 51m/h/x | gpt-5.1-codex-max | medium/high/xhigh |

## License

MIT
