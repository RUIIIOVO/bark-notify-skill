# Bark prerequisites

Use this reference when gathering setup inputs.

## Required inputs

- Bark push URL, for example: `https://api.day.app/<device-key>`
- User intent: plain push or encrypted push

## Encrypted mode requirements

Encrypted mode needs all of these:
- a 16-character encryption key
- Bark iPhone app configured to the same settings
- matching algorithm, mode, and padding

Recommended Bark iPhone settings:
- Algorithm: `AES128`
- Mode: `CBC`
- Padding: `pkcs7`
- Key: exact 16-character string chosen by the user
- IV: any temporary 16-character value is fine for saving Bark app settings; real pushes can send a request-specific `iv`

## Expected outcome

After setup, Bark notifications should show:
- title = project name
- body = `Claude Code 已完成`
