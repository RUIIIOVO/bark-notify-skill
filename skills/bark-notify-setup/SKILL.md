---
name: bark-notify-setup
description: Set up Bark completion notifications for Claude Code on macOS by safely writing the local Bark sender script and merging a Stop hook into ~/.claude/settings.json. Use when the user explicitly asks to configure Bark for them or to directly install the completion notification flow.
argument-hint: [none]
version: 0.1.0
---

# Bark Notify Setup

Configure the current macOS Claude Code environment to send a Bark notification when Claude finishes a turn.

## Goals

After setup, the local machine should contain:
- `~/.claude/claude-stop-bark.sh`
- a `Stop` hook in `~/.claude/settings.json` that runs that script asynchronously

## Required behavior

1. Confirm Bark prerequisites first.
   - Read `../bark-notify/references/bark-prereqs.md`.
   - Make sure the user has a Bark push URL.
   - Ask whether they want plain mode or encrypted mode.
   - If encrypted mode, require a 16-character key and remind them to match Bark app settings on iPhone.

2. Inspect current Claude config before writing.
   - Read `~/.claude/settings.json` if it exists.
   - Never overwrite unrelated settings.
   - Merge in the `Stop` hook safely.

3. Use the config templates from `../bark-notify/references/config-patterns.md`.
   - Write a small shell script to `~/.claude/claude-stop-bark.sh`.
   - The script should derive the project name from `.cwd` in the hook payload.
   - Keep the notification minimal:
     - title = project name
     - body = `Claude Code 已完成`
   - Use the Claude app icon URL in the Bark payload.

4. Show the user what will change before applying it.
   - Mention both file paths.
   - Mention whether the Bark URL and encryption key will be stored locally.

5. Validate after writing.
   - Ensure the script is executable.
   - Confirm the `Stop` hook command points to `/bin/bash ~/.claude/claude-stop-bark.sh`.
   - If the user wants, immediately hand off to `/bark-notify-skill:bark-notify-test`.

## Safety rules

- Do not delete existing hooks unless the user explicitly asks.
- If there is already a `Stop` hook, merge carefully and explain the result.
- If JSON is malformed, stop and show the problem instead of forcing a write.
