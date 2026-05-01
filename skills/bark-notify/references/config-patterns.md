# Config patterns

This reference contains the supported Bark hook patterns.

## Stop hook shape

Merge this structure into `~/.claude/settings.json`:

```json
{
  "hooks": {
    "Stop": [
      {
        "hooks": [
          {
            "type": "command",
            "command": "/bin/bash /Users/<user>/.claude/claude-stop-bark.sh",
            "async": true
          }
        ]
      }
    ],
    "StopFailure": [
      {
        "hooks": [
          {
            "type": "command",
            "command": "/bin/bash /Users/<user>/.claude/claude-stop-bark.sh",
            "async": true
          }
        ]
      }
    ],
    "SessionEnd": [
      {
        "hooks": [
          {
            "type": "command",
            "command": "/bin/bash /Users/<user>/.claude/claude-stop-bark.sh",
            "async": true
          }
        ]
      }
    ]
  }
}
```

Always merge instead of replacing unrelated config.

## Event types

| Hook event | Trigger | Notification body |
|------------|---------|-------------------|
| `Stop` | Claude finishes responding normally | `Claude Code 已完成` |
| `StopFailure` | Turn ends due to API error | `Claude Code 出错` |
| `SessionEnd` | Session terminates (Ctrl+C exit, /exit) | `Claude Code 会话结束` |

The script reads the `HOOK_EVENT` environment variable (set by Claude Code) to determine the event type and choose the notification body.

## Plain Bark script pattern

Plain mode uses the Bark push URL directly and sends readable title/body content.

Script responsibilities:
- read hook payload from stdin
- parse `.cwd`
- derive project name from the current path basename
- read `HOOK_EVENT` to determine notification body
- send title = project name
- send body based on event type (see table above)
- use Bark icon field with Claude icon URL

## Encrypted Bark script pattern

Encrypted mode follows the same notification content, but sends a ciphertext payload plus `iv`.

Script responsibilities:
- build plaintext JSON with title/body/group/level/isArchive/icon
- generate a random 16-character `iv`
- convert key and `iv` to hex for OpenSSL
- encrypt with `AES-128-CBC`
- base64 the ciphertext
- send `ciphertext` and `iv` to the Bark push URL

## Terminal bell for tmux / terminal notifications

After sending the Bark push, the script should output a terminal bell character (`\a`). This enables tmux and terminal apps to detect activity and notify the user — including on Ctrl+C interrupts where Claude Code hooks do not fire.

Add this line at the end of the script:

```bash
printf '\a'
```

For tmux, users should enable bell monitoring:

```bash
tmux set -g monitor-bell on
tmux set -g visual-bell on
```

Or use activity monitoring as a fallback:

```bash
tmux set -g monitor-activity on
tmux set -g visual-activity on
```

This way, even when a hook event does not fire (e.g., mid-turn Ctrl+C), tmux still detects the pane activity and notifies the user.

## Recommended Claude icon

```text
https://claude.ai/images/claude_app_icon.png
```
