---
name: openclaw-safe-update
description: Safely verify and apply OpenClaw upgrades with isolated sidecar checks. Use when asked to update OpenClaw, verify a target version before upgrading, avoid global package pollution, or run a production-safe upgrade flow with verify-only default and explicit --apply.
homepage: https://docs.openclaw.ai/install/updating
metadata:
  {
    "openclaw":
      {
        "emoji": "🛡️",
        "requires": { "bins": ["openclaw", "node", "npm", "curl", "bash"] },
      },
  }
---

# OpenClaw Safe Update

Verify first, upgrade second.

## Workflow

1. Run verify-only (default):

```bash
bash scripts/openclaw-safe-update.sh
```

Optional target pin:

```bash
bash scripts/openclaw-safe-update.sh --target 2026.3.2
```

2. If verification fails, read the printed log path and report root cause.
3. If verification passes, ask for explicit confirmation.
4. Apply upgrade only after confirmation:

```bash
bash scripts/openclaw-safe-update.sh --apply
```

## Critical behavior

- `--apply` does **not** skip verification. It verifies first, then applies global upgrade only on success.
- Default mode is verify-only.
- Sidecar port is auto-selected from free ports starting at `18000`.
- Candidate installs are isolated in `~/.openclaw/versions/<version>`.
- Candidate retention keeps latest 3 directories by mtime (`KEEP_VERSIONS`, default `3`).
- Log file is a single rolling file: `~/.openclaw/logs/openclaw-sidecar-verify.log` (overwritten per run).

## Platform references

- macOS (`uname` = `Darwin`) → `references/macos.md`
- Linux (`uname` = `Linux`) → `references/linux.md`
- Scheduled usage templates → `references/cron-templates.md`

## Safety rules

- Never run `--apply` without explicit user consent.
- If verify fails, do not mutate global install.
- If `gateway restart` fails after apply, report warning and request manual restart.
