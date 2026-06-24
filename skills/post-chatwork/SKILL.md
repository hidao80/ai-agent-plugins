---
name: post-chatwork
description: Post a message or file to a Chatwork room. Use when Claude needs to notify the user via Chatwork during a chat session or routine execution.
---

# Post Chatwork Skill

- [Post Chatwork Skill](#post-chatwork-skill)
  - [When to Use](#when-to-use)
  - [Prerequisites](#prerequisites)
    - [Environment Variables (`env` section in `settings.json`)](#environment-variables-env-section-in-settingsjson)
    - [Room Config File](#room-config-file)
  - [Steps](#steps)
    - [Step 1: Load room config](#step-1-load-room-config)
    - [Step 2: Determine the target room](#step-2-determine-the-target-room)
    - [Step 3: Determine the message content](#step-3-determine-the-message-content)
    - [Step 4: Detect the OS](#step-4-detect-the-os)
    - [Step 5: Call the API](#step-5-call-the-api)
      - [Text-only post](#text-only-post)
      - [File upload (message optional)](#file-upload-message-optional)
    - [Step 6: Report the result](#step-6-report-the-result)
  - [Adding or Changing Rooms](#adding-or-changing-rooms)
  - [Chatwork API Reference](#chatwork-api-reference)

## When to Use

- When the user asks to "post to Chatwork" or "send via Chatwork"
- When a routine execution result needs to be reported via Chatwork
- When notifying asynchronously about task completion or errors

## Prerequisites

### Environment Variables (`env` section in `settings.json`)

| Variable | Description |
|----------|-------------|
| `CHATWORK_API_KEY` | Chatwork API token |

### Room Config File

Create `.claude/references/chatwork.json` at the project root:

```json
{
  "rooms": [
    { "name": "My Room", "id": "109627044" },
    { "name": "Meeting Room", "id": "111111" }
  ]
}
```

Additional per-room settings (e.g. `mention_to`, `prefix`) can be added to this file in the future.

## Steps

### Step 1: Load room config

Search for `.claude/references/chatwork.json` by walking up from the project root.

**Windows (PowerShell)**

```powershell
# Walk up directories to find .claude/references/chatwork.json
$dir = $PWD.Path
while ($dir -and -not (Test-Path "$dir\.claude\references\chatwork.json")) {
    $parent = Split-Path $dir -Parent
    if ($parent -eq $dir) { $dir = $null; break }
    $dir = $parent
}
if (-not $dir) { throw "chatwork.json not found. Please create .claude/references/chatwork.json" }

$config = Get-Content "$dir\.claude\references\chatwork.json" -Raw -Encoding UTF8 | ConvertFrom-Json
$rooms  = $config.rooms
```

**Linux / macOS (bash)**

```bash
dir="$PWD"
while [ "$dir" != "/" ]; do
  [ -f "$dir/.claude/references/chatwork.json" ] && break
  dir="$(dirname "$dir")"
done
[ ! -f "$dir/.claude/references/chatwork.json" ] && echo "chatwork.json not found" && exit 1
rooms_json="$(cat "$dir/.claude/references/chatwork.json")"
```

### Step 2: Determine the target room

- If the user specifies a destination → select the room matching by `name`
- If not specified → list available rooms and ask the user to choose
- If only one room exists → use it automatically

**Windows (PowerShell)**

```powershell
# Select by name
$targetRoom = $rooms | Where-Object { $_.name -eq "My Room" }
# If only one room
$targetRoom = $rooms[0]
```

**Linux / macOS (bash)**

```bash
# Select by name using jq
ROOM_ID="$(echo "$rooms_json" | jq -r '.rooms[] | select(.name=="My Room") | .id')"
# If only one room
ROOM_ID="$(echo "$rooms_json" | jq -r '.rooms[0].id')"
```

### Step 3: Determine the message content

- **Message text** (required): the body of the message to send
- **File path** (optional): absolute path to a file to attach

### Step 4: Detect the OS

- **Windows**: PowerShell environment → use `Invoke-RestMethod`
- **Linux / macOS**: detected via `uname` → use `curl`

### Step 5: Call the API

#### Text-only post

**Windows (PowerShell)**

```powershell
$headers = @{ "X-ChatWorkToken" = $env:CHATWORK_API_KEY }
$body    = @{ body = "message text" }
$res     = Invoke-RestMethod `
    -Uri     "https://api.chatwork.com/v2/rooms/$($targetRoom.id)/messages" `
    -Method  POST `
    -Headers $headers `
    -Body    $body
Write-Output "Posted [$($targetRoom.name)]: message_id=$($res.message_id)"
```

**Linux / macOS (curl)**

```bash
curl -s -X POST \
  -H "X-ChatWorkToken: $CHATWORK_API_KEY" \
  --data-urlencode "body=message text" \
  "https://api.chatwork.com/v2/rooms/$ROOM_ID/messages"
```

#### File upload (message optional)

**Windows (PowerShell)**

```powershell
$headers = @{ "X-ChatWorkToken" = $env:CHATWORK_API_KEY }
$form    = @{
    file    = Get-Item -Path "file path"
    message = "optional message for the attachment"
}
$res     = Invoke-RestMethod `
    -Uri     "https://api.chatwork.com/v2/rooms/$($targetRoom.id)/files" `
    -Method  POST `
    -Headers $headers `
    -Form    $form
Write-Output "Uploaded [$($targetRoom.name)]: file_id=$($res.file_id)"
```

> **Note**: `-Form` requires PowerShell 7.1 or later. On PS 5.1, use `curl.exe` (built into Windows 10+).

**Linux / macOS (curl)**

```bash
curl -s -X POST \
  -H "X-ChatWorkToken: $CHATWORK_API_KEY" \
  -F "file=@/path/to/file" \
  -F "message=optional message for the attachment" \
  "https://api.chatwork.com/v2/rooms/$ROOM_ID/files"
```

### Step 6: Report the result

| Result | Report |
|--------|--------|
| Success | Tell the user the target room name and `message_id` or `file_id` |
| Failure | Report the HTTP status code and error message, then investigate the cause |

## Adding or Changing Rooms

Edit `.claude/references/chatwork.json` in your project:

```json
{
  "rooms": [
    { "name": "My Room",      "id": "109627044" },
    { "name": "Meeting Room", "id": "111111" },
    { "name": "Alice",        "id": "222222" }
  ]
}
```

## Chatwork API Reference

| Operation | Method | Endpoint |
|-----------|--------|----------|
| Post text | POST | `/v2/rooms/{room_id}/messages` |
| Upload file | POST | `/v2/rooms/{room_id}/files` |

Common header: `X-ChatWorkToken: {CHATWORK_API_KEY}`
