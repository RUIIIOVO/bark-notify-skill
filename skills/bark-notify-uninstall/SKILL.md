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
2. Remove only the `Stop` hook that points to `claude-stop-bark.sh`.
   - Preserve all other hooks and settings.
   - If no `Stop` hook exists, skip this step.
   - If `Stop` has other hooks besides the Bark one, keep those.
3. Delete `~/.claude/claude-stop-bark.sh` if it exists.
4. Report what was removed to the user.

## Safety rules

- Never delete unrelated hooks or settings.
- If `~/.claude/settings.json` is malformed, stop and report the problem.
- Confirm with the user before removing files.
