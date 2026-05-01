# Config patterns

This reference contains the two supported Bark hook patterns.

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
    ]
  }
}
```

Always merge instead of replacing unrelated config.

## Plain Bark script pattern

Plain mode uses the Bark push URL directly and sends readable title/body content.

Script responsibilities:
- read hook payload from stdin
- parse `.cwd`
- derive project name from the current path basename
- send title = project name
- send body = `Claude Code 已完成`
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

## Recommended Claude icon

```text
https://claude.ai/images/claude_app_icon.png
```
