---
name: bark-notify-uninstall
description: Remove Bark notification configuration from Claude Code on macOS. Use when the user wants to uninstall, remove, or clean up Bark notifications, delete the Bark hook, or reset their Bark setup.
argument-hint: [none]
version: 0.1.0
---

# Bark Notify Uninstall

Remove the Bark notification hook and script from the local Claude Code environment.

## Behavior

1. Read `~/.claude/settings.json` if it exists.
2. Remove the `Stop`, `StopFailure`, and `SessionEnd` hooks that point to `claude-stop-bark.sh`.
   - Preserve all other hooks and settings.
   - If a hook event has other hooks besides the Bark one, keep those.
   - If a hook event does not exist, skip it.
   - If removing all Bark hooks leaves the `hooks` key empty, remove the `hooks` key entirely.
3. Delete `~/.claude/claude-stop-bark.sh` if it exists.
4. Report what was removed to the user.

## Safety rules

- Never delete unrelated hooks or settings.
- If `~/.claude/settings.json` is malformed, stop and report the problem.
- Confirm with the user before removing files.
