# openclaw-safe-update

A verify-first OpenClaw update skill for agents.

This repository uses a multi-skill-ready layout:

```text
openclaw-safe-update/
  skills/
    openclaw-safe-update/
      SKILL.md
      scripts/
      references/
```

## What it does

- Verifies a candidate OpenClaw version in an isolated sidecar before global mutation
- Defaults to verify-only mode
- `--apply` still runs full verification first, then upgrades globally only on success
- Auto-selects a free sidecar port (starting at `18000`)
- Keeps candidate cache trimmed (latest 3 versions by default, by directory mtime)
- Uses a single rolling log file per run (`~/.openclaw/logs/openclaw-sidecar-verify.log`)

## How to use

```bash
cd skills/openclaw-safe-update

# Verify latest (safe default)
bash scripts/openclaw-safe-update.sh

# Verify a specific version
bash scripts/openclaw-safe-update.sh --target 2026.3.2

# Apply global upgrade after successful verification
bash scripts/openclaw-safe-update.sh --apply
```

## Log path

- `~/.openclaw/logs/openclaw-sidecar-verify.log` (single rolling file, overwritten each run)

## Scheduled usage

- Cron templates: `skills/openclaw-safe-update/references/cron-templates.md`

## Exit behavior

- `0`: success (verify-only success, or apply success)
- `1`: runtime failure (dependency/startup/health/apply failure)
- `2`: argument error (unknown argument)
