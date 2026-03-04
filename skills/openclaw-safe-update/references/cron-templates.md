# Cron templates

Use these templates when users want scheduled updates. Adjust timezone and schedule before applying.

## 1) Daily verify-only (recommended baseline)

Runs verification daily without global mutation.

```bash
openclaw cron add \
  --name "openclaw-safe-update-verify-daily" \
  --cron "30 3 * * *" \
  --tz "Asia/Shanghai" \
  --session isolated \
  --wake now \
  --deliver \
  --message "Run safe update verification: bash ~/project/openclaw-safe-update/skills/openclaw-safe-update/scripts/openclaw-safe-update.sh"
```

## 2) Weekly apply (explicitly mutating)

Runs full verify + apply once a week.

```bash
openclaw cron add \
  --name "openclaw-safe-update-apply-weekly" \
  --cron "0 4 * * 0" \
  --tz "Asia/Shanghai" \
  --session isolated \
  --wake now \
  --deliver \
  --message "Run safe update apply: bash ~/project/openclaw-safe-update/skills/openclaw-safe-update/scripts/openclaw-safe-update.sh --apply"
```

## 3) Verify pinned target version

Useful for controlled rollouts.

```bash
openclaw cron add \
  --name "openclaw-safe-update-verify-pinned" \
  --cron "15 3 * * *" \
  --tz "Asia/Shanghai" \
  --session isolated \
  --wake now \
  --deliver \
  --message "Verify pinned target: bash ~/project/openclaw-safe-update/skills/openclaw-safe-update/scripts/openclaw-safe-update.sh --target 2026.3.2"
```

## Notes

- Keep timezone explicit with `--tz`.
- Prefer verify-only for daily automation.
- Use weekly apply only if automatic mutation is acceptable.
