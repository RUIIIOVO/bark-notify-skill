# Troubleshooting

## Bark receives nothing

Check:
- Bark app is installed on iPhone
- Bark push URL is correct
- phone can receive Bark notifications
- local script actually sends a real request, not only a dry run
- the Claude `Stop` hook is present in `~/.claude/settings.json`

## Bark says Decryption Failed

The usual cause is a mismatch between the sender and Bark app settings.

Verify on iPhone:
- Algorithm = `AES128`
- Mode = `CBC`
- Padding = `pkcs7`
- Key exactly matches the 16-character sender key

Notes:
- request `iv` can differ per notification
- Bark uses the request `iv` when it is provided
- the app's saved IV is only there so the local config can be saved

## Bark API returns 400

Usually one of these:
- wrong push URL
- stale device key
- invalid ciphertext payload
- malformed request encoding

## Hook exists but nothing fires

Check:
- `~/.claude/settings.json` is valid JSON
- the hook command points to the right script path
- the script is executable
- the current Claude session has been restarted if needed
