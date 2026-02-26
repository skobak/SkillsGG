# SkillsGG

Codex Mac app-compatible distribution for a Codex-specific `gg` skill.

## Layout

- `skills/gg/SKILL.md` (required)
- `skills/gg/agents/openai.yaml` (UI metadata for Codex app)

## Install (skill-installer compatible)

Install from this repo path:

```bash
python3 ~/.codex/skills/.system/skill-installer/scripts/install-skill-from-github.py \
  --repo skobak/SkillsGG \
  --path skills/gg
```

Or copy the folder manually to `~/.codex/skills/gg`.
