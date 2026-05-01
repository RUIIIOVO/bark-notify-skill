---
name: bark-notify-test
description: Send a Bark verification notification for the current Claude Code Bark setup on macOS. Use when the user wants to test delivery, confirm encryption works, or check whether the configured completion notification is healthy.
argument-hint: [optional project name]
version: 0.2.0
---

# Bark Notify Test

Send one verification notification using the user's local Bark script or Bark configuration.

## Behavior

1. Read `~/.claude/claude-stop-bark.sh` if it exists.
2. If the script exists, use it for the test instead of inventing a second send path.
3. Use a safe payload with a current or user-provided project name.
4. Tell the user exactly what should appear on the phone:
   - title = project name
   - body = `Claude Code 已完成`
5. If the notification fails, read `../bark-notify/references/troubleshooting.md` and debug from there.

## Notes

- Prefer a real push test over a dry run when the user explicitly asks for verification.
- For encrypted mode, the most common failure is mismatched Bark iPhone settings or wrong key.
