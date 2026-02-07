# Tools Architecture - Complete JSON Schemas

## Tool: read
**Description**: Read the contents of a file. Supports text files and images (jpg, png, gif, webp). Images are sent as attachments. For text files, output is truncated to 2000 lines or 50KB (whichever is hit first). Use offset/limit for large files. When you need the full file, continue with offset until complete.

**JSON Schema**:
```json
{
  "name": "read",
  "description": "Read the contents of a file. Supports text files and images (jpg, png, gif, webp). Images are sent as attachments. For text files, output is truncated to 2000 lines or 50KB (whichever is hit first). Use offset/limit for large files. When you need the full file, continue with offset until complete.",
  "parameters": {
    "type": "object",
    "required": [],
    "properties": {
      "path": {
        "description": "Path to the file to read (relative or absolute)",
        "type": "string"
      },
      "offset": {
        "description": "Line number to start reading from (1-indexed)",
        "type": "number"
      },
      "limit": {
        "description": "Maximum number of lines to read",
        "type": "number"
      },
      "file_path": {
        "description": "Path to the file to read (relative or absolute)",
        "type": "string"
      }
    }
  }
}
```

## Tool: write
**Description**: Write content to a file. Creates the file if it doesn't exist, overwrites if it does. Automatically creates parent directories.

**JSON Schema**:
```json
{
  "name": "write",
  "description": "Write content to a file. Creates the file if it doesn't exist, overwrites if it does. Automatically creates parent directories.",
  "parameters": {
    "type": "object",
    "required": ["content"],
    "properties": {
      "path": {
        "description": "Path to the file to write (relative or absolute)",
        "type": "string"
      },
      "content": {
        "description": "Content to write to the file",
        "type": "string"
      },
      "file_path": {
        "description": "Path to the file to write (relative or absolute)",
        "type": "string"
      }
    }
  }
}
```

## Tool: edit
**Description**: Edit a file by replacing exact text. The oldText must match exactly (including whitespace). Use this for precise, surgical edits.

**JSON Schema**:
```json
{
  "name": "edit",
  "description": "Edit a file by replacing exact text. The oldText must match exactly (including whitespace). Use this for precise, surgical edits.",
  "parameters": {
    "type": "object",
    "required": [],
    "properties": {
      "path": {
        "description": "Path to the file to edit (relative or absolute)",
        "type": "string"
      },
      "oldText": {
        "description": "Exact text to find and replace (must match exactly)",
        "type": "string"
      },
      "newText": {
        "description": "New text to replace the old text with",
        "type": "string"
      },
      "file_path": {
        "description": "Path to the file to edit (relative or absolute)",
        "type": "string"
      },
      "old_string": {
        "description": "Exact text to find and replace (must match exactly)",
        "type": "string"
      },
      "new_string": {
        "description": "New text to replace the old text with",
        "type": "string"
      }
    }
  }
}
```

## Tool: exec
**Description**: Execute shell commands with background continuation. Use yieldMs/background to continue later via process tool. Use pty=true for TTY-required commands (terminal UIs, coding agents).

**JSON Schema**:
```json
{
  "name": "exec",
  "description": "Execute shell commands with background continuation. Use yieldMs/background to continue later via process tool. Use pty=true for TTY-required commands (terminal UIs, coding agents).",
  "parameters": {
    "type": "object",
    "required": ["command"],
    "properties": {
      "command": {
        "description": "Shell command to execute",
        "type": "string"
      },
      "workdir": {
        "description": "Working directory (defaults to cwd)",
        "type": "string"
      },
      "env": {
        "type": "object"
      },
      "yieldMs": {
        "description": "Milliseconds to wait before backgrounding (default 10000)",
        "type": "number"
      },
      "background": {
        "description": "Run in background immediately",
        "type": "boolean"
      },
      "timeout": {
        "description": "Timeout in seconds (optional, kills process on expiry)",
        "type": "number"
      },
      "pty": {
        "description": "Run in a pseudo-terminal (PTY) when available (TTY-required CLIs, coding agents)",
        "type": "boolean"
      },
      "elevated": {
        "description": "Run on the host with elevated permissions (if allowed)",
        "type": "boolean"
      },
      "host": {
        "description": "Exec host (sandbox|gateway|node).",
        "type": "string"
      },
      "security": {
        "description": "Exec security mode (deny|allowlist|full).",
        "type": "string"
      },
      "ask": {
        "description": "Exec ask mode (off|on-miss|always).",
        "type": "string"
      },
      "node": {
        "description": "Node id/name for host=node.",
        "type": "string"
      }
    }
  }
}
```

## Tool: process
**Description**: Manage running exec sessions: list, poll, log, write, send-keys, submit, paste, kill.

**JSON Schema**:
```json
{
  "name": "process",
  "description": "Manage running exec sessions: list, poll, log, write, send-keys, submit, paste, kill.",
  "parameters": {
    "type": "object",
    "required": ["action"],
    "properties": {
      "action": {
        "description": "Process action",
        "type": "string"
      },
      "sessionId": {
        "description": "Session id for actions other than list",
        "type": "string"
      },
      "data": {
        "description": "Data to write for write",
        "type": "string"
      },
      "keys": {
        "description": "Key tokens to send for send-keys",
        "type": "array",
        "items": {
          "type": "string"
        }
      },
      "hex": {
        "description": "Hex bytes to send for send-keys",
        "type": "array",
        "items": {
          "type": "string"
        }
      },
      "literal": {
        "description": "Literal string for send-keys",
        "type": "string"
      },
      "text": {
        "description": "Text to paste for paste",
        "type": "string"
      },
      "bracketed": {
        "description": "Wrap paste in bracketed mode",
        "type": "boolean"
      },
      "eof": {
        "description": "Close stdin after write",
        "type": "boolean"
      },
      "offset": {
        "description": "Log offset",
        "type": "number"
      },
      "limit": {
        "description": "Log length",
        "type": "number"
      }
    }
  }
}
```

## Tool: browser
**Description**: Control the browser via OpenClaw's browser control server (status/start/stop/profiles/tabs/open/snapshot/screenshot/actions). Profiles: use profile="chrome" for Chrome extension relay takeover (your existing Chrome tabs). Use profile="openclaw" for the isolated openclaw-managed browser. If the user mentions the Chrome extension / Browser Relay / toolbar button / "attach tab", ALWAYS use profile="chrome" (do not ask which profile). When a node-hosted browser proxy is available, the tool may auto-route to it. Pin a node with node=<id|name> or target="node". Chrome extension relay needs an attached tab: user must click the OpenClaw Browser Relay toolbar icon on the tab (badge ON). If no tab is connected, ask them to attach it. When using refs from snapshot (e.g. e12), keep the same tab: prefer passing targetId from the snapshot response into subsequent actions (act/click/type/etc). For stable, self-resolving refs across calls, use snapshot with refs="aria" (Playwright aria-ref ids). Default refs="role" are role+name-based. Use snapshot+act for UI automation. Avoid act:wait by default; use only in exceptional cases when no reliable UI state exists. target selects browser location (sandbox|host|node). Default: host. Host target allowed.

**JSON Schema**:
```json
{
  "name": "browser",
  "description": "Control the browser via OpenClaw's browser control server (status/start/stop/profiles/tabs/open/snapshot/screenshot/actions). Profiles: use profile=\"chrome\" for Chrome extension relay takeover (your existing Chrome tabs). Use profile=\"openclaw\" for the isolated openclaw-managed browser. If the user mentions the Chrome extension / Browser Relay / toolbar button / \"attach tab\", ALWAYS use profile=\"chrome\" (do not ask which profile). When a node-hosted browser proxy is available, the tool may auto-route to it. Pin a node with node=<id|name> or target=\"node\". Chrome extension relay needs an attached tab: user must click the OpenClaw Browser Relay toolbar icon on the tab (badge ON). If no tab is connected, ask them to attach it. When using refs from snapshot (e.g. e12), keep the same tab: prefer passing targetId from the snapshot response into subsequent actions (act/click/type/etc). For stable, self-resolving refs across calls, use snapshot with refs=\"aria\" (Playwright aria-ref ids). Default refs=\"role\" are role+name-based. Use snapshot+act for UI automation. Avoid act:wait by default; use only in exceptional cases when no reliable UI state exists. target selects browser location (sandbox|host|node). Default: host. Host target allowed.",
  "parameters": {
    "type": "object",
    "required": ["action"],
    "properties": {
      "action": {
        "type": "string",
        "enum": ["status", "start", "stop", "profiles", "tabs", "open", "focus", "close", "snapshot", "screenshot", "navigate", "console", "pdf", "upload", "dialog", "act"]
      },
      "target": {
        "type": "string",
        "enum": ["sandbox", "host", "node"]
      },
      "node": {
        "type": "string"
      },
      "profile": {
        "type": "string"
      },
      "targetUrl": {
        "type": "string"
      },
      "targetId": {
        "type": "string"
      },
      "limit": {
        "type": "number"
      },
      "maxChars": {
        "type": "number"
      },
      "mode": {
        "type": "string",
        "enum": ["efficient"]
      },
      "snapshotFormat": {
        "type": "string",
        "enum": ["aria", "ai"]
      },
      "refs": {
        "type": "string",
        "enum": ["role", "aria"]
      },
      "interactive": {
        "type": "boolean"
      },
      "compact": {
        "type": "boolean"
      },
      "depth": {
        "type": "number"
      },
      "selector": {
        "type": "string"
      },
      "frame": {
        "type": "string"
      },
      "labels": {
        "type": "boolean"
      },
      "fullPage": {
        "type": "boolean"
      },
      "ref": {
        "type": "string"
      },
      "element": {
        "type": "string"
      },
      "type": {
        "type": "string",
        "enum": ["png", "jpeg"]
      },
      "level": {
        "type": "string"
      },
      "paths": {
        "type": "array",
        "items": {
          "type": "string"
        }
      },
      "inputRef": {
        "type": "string"
      },
      "timeoutMs": {
        "type": "number"
      },
      "accept": {
        "type": "boolean"
      },
      "promptText": {
        "type": "string"
      },
      "request": {
        "type": "object",
        "required": ["kind"],
        "properties": {
          "kind": {
            "type": "string",
            "enum": ["click", "type", "press", "hover", "drag", "select", "fill", "resize", "wait", "evaluate", "close"]
          },
          "targetId": {
            "type": "string"
          },
          "ref": {
            "type": "string"
          },
          "doubleClick": {
            "type": "boolean"
          },
          "button": {
            "type": "string"
          },
          "modifiers": {
            "type": "array",
            "items": {
              "type": "string"
            }
          },
          "text": {
            "type": "string"
          },
          "submit": {
            "type": "boolean"
          },
          "slowly": {
            "type": "boolean"
          },
          "key": {
            "type": "string"
          },
          "startRef": {
            "type": "string"
          },
          "endRef": {
            "type": "string"
          },
          "values": {
            "type": "array",
            "items": {
              "type": "string"
            }
          },
          "fields": {
            "type": "array",
            "items": {
              "type": "object",
              "properties": {}
            }
          },
          "width": {
            "type": "number"
          },
          "height": {
            "type": "number"
          },
          "timeMs": {
            "type": "number"
          },
          "textGone": {
            "type": "string"
          },
          "fn": {
            "type": "string"
          }
        }
      }
    }
  }
}
```

## Tool: canvas
**Description**: Control node canvases (present/hide/navigate/eval/snapshot/A2UI). Use snapshot to capture the rendered UI.

**JSON Schema**:
```json
{
  "name": "canvas",
  "description": "Control node canvases (present/hide/navigate/eval/snapshot/A2UI). Use snapshot to capture the rendered UI.",
  "parameters": {
    "type": "object",
    "required": ["action"],
    "properties": {
      "action": {
        "type": "string",
        "enum": ["present", "hide", "navigate", "eval", "snapshot", "a2ui_push", "a2ui_reset"]
      },
      "gatewayUrl": {
        "type": "string"
      },
      "gatewayToken": {
        "type": "string"
      },
      "timeoutMs": {
        "type": "number"
      },
      "node": {
        "type": "string"
      },
      "target": {
        "type": "string"
      },
      "x": {
        "type": "number"
      },
      "y": {
        "type": "number"
      },
      "width": {
        "type": "number"
      },
      "height": {
        "type": "number"
      },
      "url": {
        "type": "string"
      },
      "javaScript": {
        "type": "string"
      },
      "outputFormat": {
        "type": "string",
        "enum": ["png", "jpg", "jpeg"]
      },
      "maxWidth": {
        "type": "number"
      },
      "quality": {
        "type": "number"
      },
      "delayMs": {
        "type": "number"
      },
      "jsonl": {
        "type": "string"
      },
      "jsonlPath": {
        "type": "string"
      }
    }
  }
}
```

## Tool: nodes
**Description**: Discover and control paired nodes (status/describe/pairing/notify/camera/screen/location/run/invoke).

**JSON Schema**:
```json
{
  "name": "nodes",
  "description": "Discover and control paired nodes (status/describe/pairing/notify/camera/screen/location/run/invoke).",
  "parameters": {
    "type": "object",
    "required": ["action"],
    "properties": {
      "action": {
        "type": "string",
        "enum": ["status", "describe", "pending", "approve", "reject", "notify", "camera_snap", "camera_list", "camera_clip", "screen_record", "location_get", "run", "invoke"]
      },
      "gatewayUrl": {
        "type": "string"
      },
      "gatewayToken": {
        "type": "string"
      },
      "timeoutMs": {
        "type": "number"
      },
      "node": {
        "type": "string"
      },
      "requestId": {
        "type": "string"
      },
      "title": {
        "type": "string"
      },
      "body": {
        "type": "string"
      },
      "sound": {
        "type": "string"
      },
      "priority": {
        "type": "string",
        "enum": ["passive", "active", "timeSensitive"]
      },
      "delivery": {
        "type": "string",
        "enum": ["system", "overlay", "auto"]
      },
      "facing": {
        "type": "string",
        "enum": ["front", "back", "both"],
        "description": "camera_snap: front/back/both; camera_clip: front/back only."
      },
      "maxWidth": {
        "type": "number"
      },
      "quality": {
        "type": "number"
      },
      "delayMs": {
        "type": "number"
      },
      "deviceId": {
        "type": "string"
      },
      "duration": {
        "type": "string"
      },
      "durationMs": {
        "type": "number"
      },
      "includeAudio": {
        "type": "boolean"
      },
      "fps": {
        "type": "number"
      },
      "screenIndex": {
        "type": "number"
      },
      "outPath": {
        "type": "string"
      },
      "maxAgeMs": {
        "type": "number"
      },
      "locationTimeoutMs": {
        "type": "number"
      },
      "desiredAccuracy": {
        "type": "string",
        "enum": ["coarse", "balanced", "precise"]
      },
      "command": {
        "type": "array",
        "items": {
          "type": "string"
        }
      },
      "cwd": {
        "type": "string"
      },
      "env": {
        "type": "array",
        "items": {
          "type": "string"
        }
      },
      "commandTimeoutMs": {
        "type": "number"
      },
      "invokeTimeoutMs": {
        "type": "number"
      },
      "needsScreenRecording": {
        "type": "boolean"
      },
      "invokeCommand": {
        "type": "string"
      },
      "invokeParamsJson": {
        "type": "string"
      }
    }
  }
}
```

## Tool: cron
**Description**: Manage Gateway cron jobs (status/list/add/update/remove/run/runs) and send wake events.

**JSON Schema**:
```json
{
  "name": "cron",
  "description": "Manage Gateway cron jobs (status/list/add/update/remove/run/runs) and send wake events.",
  "parameters": {
    "type": "object",
    "required": ["action"],
    "properties": {
      "action": {
        "type": "string",
        "enum": ["status", "list", "add", "update", "remove", "run", "runs", "wake"]
      },
      "gatewayUrl": {
        "type": "string"
      },
      "gatewayToken": {
        "type": "string"
      },
      "timeoutMs": {
        "type": "number"
      },
      "includeDisabled": {
        "type": "boolean"
      },
      "job": {
        "type": "object",
        "properties": {}
      },
      "jobId": {
        "type": "string"
      },
      "id": {
        "type": "string"
      },
      "patch": {
        "type": "object",
        "properties": {}
      },
      "text": {
        "type": "string"
      },
      "mode": {
        "type": "string",
        "enum": ["now", "next-heartbeat"]
      },
      "contextMessages": {
        "type": "number"
      }
    }
  }
}
```

## Tool: message
**Description**: Send, delete, and manage messages via channel plugins. Current channel (whatsapp) supports: poll, react, send.

**JSON Schema**:
```json
{
  "name": "message",
  "description": "Send, delete, and manage messages via channel plugins. Current channel (whatsapp) supports: poll, react, send.",
  "parameters": {
    "type": "object",
    "required": ["action"],
    "properties": {
      "action": {
        "type": "string",
        "enum": ["send", "broadcast", "react", "poll"]
      },
      "channel": {
        "type": "string"
      },
      "target": {
        "description": "Target channel/user id or name.",
        "type": "string"
      },
      "targets": {
        "type": "array",
        "items": {
          "description": "Recipient/channel targets (same format as --target); accepts ids or names when the directory is available.",
          "type": "string"
        }
      },
      "accountId": {
        "type": "string"
      },
      "dryRun": {
        "type": "boolean"
      },
      "message": {
        "type": "string"
      },
      "effectId": {
        "description": "Message effect name/id for sendWithEffect (e.g., invisible ink).",
        "type": "string"
      },
      "effect": {
        "description": "Alias for effectId (e.g., invisible-ink, balloons).",
        "type": "string"
      },
      "media": {
        "description": "Media URL or local path. data: URLs are not supported here, use buffer.",
        "type": "string"
      },
      "filename": {
        "type": "string"
      },
      "buffer": {
        "description": "Base64 payload for attachments (optionally a data: URL).",
        "type": "string"
      },
      "contentType": {
        "type": "string"
      },
      "mimeType": {
        "type": "string"
      },
      "caption": {
        "type": "string"
      },
      "path": {
        "type": "string"
      },
      "filePath": {
        "type": "string"
      },
      "replyTo": {
        "type": "string"
      },
      "threadId": {
        "type": "string"
      },
      "asVoice": {
        "type": "boolean"
      },
      "silent": {
        "type": "boolean"
      },
      "quoteText": {
        "description": "Quote text for Telegram reply_parameters",
        "type": "string"
      },
      "bestEffort": {
        "type": "boolean"
      },
      "gifPlayback": {
        "type": "boolean"
      },
      "messageId": {
        "type": "string"
      },
      "emoji": {
        "type": "string"
      },
      "remove": {
        "type": "boolean"
      },
      "targetAuthor": {
        "type": "string"
      },
      "targetAuthorUuid": {
        "type": "string"
      },
      "groupId": {
        "type": "string"
      },
      "limit": {
        "type": "number"
      },
      "before": {
        "type": "string"
      },
      "after": {
        "type": "string"
      },
      "around": {
        "type": "string"
      },
      "fromMe": {
        "type": "boolean"
      },
      "includeArchived": {
        "type": "boolean"
      },
      "pollQuestion": {
        "type": "string"
      },
      "pollOption": {
        "type": "array",
        "items": {
          "type": "string"
        }
      },
      "pollDurationHours": {
        "type": "number"
      },
      "pollMulti": {
        "type": "boolean"
      },
      "channelId": {
        "description": "Channel id filter (search/thread list/event create).",
        "type": "string"
      },
      "channelIds": {
        "type": "array",
        "items": {
          "description": "Channel id filter (repeatable).",
          "type": "string"
        }
      },
      "guildId": {
        "type": "string"
      },
      "userId": {
        "type": "string"
      },
      "authorId": {
        "type": "string"
      },
      "authorIds": {
        "type": "array",
        "items": {
          "type": "string"
        }
      },
      "roleId": {
        "type": "string"
      },
      "roleIds": {
        "type": "array",
        "items": {
          "type": "string"
        }
      },
      "participant": {
        "type": "string"
      },
      "emojiName": {
        "type": "string"
      },
      "stickerId": {
        "type": "array",
        "items": {
          "type": "string"
        }
      },
      "stickerName": {
        "type": "string"
      },
      "stickerDesc": {
        "type": "string"
      },
      "stickerTags": {
        "type": "string"
      },
      "threadName": {
        "type": "string"
      },
      "autoArchiveMin": {
        "type": "number"
      },
      "query": {
        "type": "string"
      },
      "eventName": {
        "type": "string"
      },
      "eventType": {
        "type": "string"
      },
      "startTime": {
        "type": "string"
      },
      "endTime": {
        "type": "string"
      },
      "desc": {
        "type": "string"
      },
      "location": {
        "type": "string"
      },
      "durationMin": {
        "type": "number"
      },
      "until": {
        "type": "string"
      },
      "reason": {
        "type": "string"
      },
      "deleteDays": {
        "type": "number"
      },
      "gatewayUrl": {
        "type": "string"
      },
      "gatewayToken": {
        "type": "string"
      },
      "timeoutMs": {
        "type": "number"
      },
      "name": {
        "type": "string"
      },
      "type": {
        "type": "number"
      },
      "parentId": {
        "type": "string"
      },
      "topic": {
        "type": "string"
      },
      "position": {
        "type": "number"
      },
      "nsfw": {
        "type": "boolean"
      },
      "rateLimitPerUser": {
        "type": "number"
      },
      "categoryId": {
        "type": "string"
      },
      "clearParent": {
        "description": "Clear the parent/category when supported by the provider.",
        "type": "boolean"
      },
      "activityType": {
        "description": "Activity type: playing, streaming, listening, watching, competing, custom.",
        "type": "string"
      },
      "activityName": {
        "description": "Activity name shown in sidebar (e.g. 'with fire'). Ignored for custom type.",
        "type": "string"
      },
      "activityUrl": {
        "description": "Streaming URL (Twitch or YouTube). Only used with streaming type; may not render for bots.",
        "type": "string"
      },
      "activityState": {
        "description": "State text. For custom type this is the status text; for others it shows in the flyout.",
        "type": "string"
      },
      "status": {
        "description": "Bot status: online, dnd, idle, invisible.",
        "type": "string"
      }
    }
  }
}
```

## Tool: tts
**Description**: Convert text to speech and return a MEDIA: path. Use when the user requests audio or TTS is enabled. Copy the MEDIA line exactly.

**JSON Schema**:
```json
{
  "name": "tts",
  "description": "Convert text to speech and return a MEDIA: path. Use when the user requests audio or TTS is enabled. Copy the MEDIA line exactly.",
  "parameters": {
    "type": "object",
    "required": ["text"],
    "properties": {
      "text": {
        "description": "Text to convert to speech.",
        "type": "string"
      },
      "channel": {
        "description": "Optional channel id to pick output format (e.g. telegram).",
        "type": "string"
      }
    }
  }
}
```

## Tool: gateway
**Description**: Restart, apply config, or update the gateway in-place (SIGUSR1). Use config.patch for safe partial config updates (merges with existing). Use config.apply only when replacing entire config. Both trigger restart after writing.

**JSON Schema**:
```json
{
  "name": "gateway",
  "description": "Restart, apply config, or update the gateway in-place (SIGUSR1). Use config.patch for safe partial config updates (merges with existing). Use config.apply only when replacing entire config. Both trigger restart after writing.",
  "parameters": {
    "type": "object",
    "required": ["action"],
    "properties": {
      "action": {
        "type": "string",
        "enum": ["restart", "config.get", "config.schema", "config.apply", "config.patch", "update.run"]
      },
      "delayMs": {
        "type": "number"
      },
      "reason": {
        "type": "string"
      },
      "gatewayUrl": {
        "type": "string"
      },
      "gatewayToken": {
        "type": "string"
      },
      "timeoutMs": {
        "type": "number"
      },
      "raw": {
        "type": "string"
      },
      "baseHash": {
        "type": "string"
      },
      "sessionKey": {
        "type": "string"
      },
      "note": {
        "type": "string"
      },
      "restartDelayMs": {
        "type": "number"
      }
    }
  }
}
```

## Tool: agents_list
**Description**: List agent ids you can target with sessions_spawn (based on allowlists).

**JSON Schema**:
```json
{
  "name": "agents_list",
  "description": "List agent ids you can target with sessions_spawn (based on allowlists).",
  "parameters": {
    "type": "object",
    "properties": {}
  }
}
```

## Tool: sessions_list
**Description**: List sessions with optional filters and last messages.

**JSON Schema**:
```json
{
  "name": "sessions_list",
  "description": "List sessions with optional filters and last messages.",
  "parameters": {
    "type": "object",
    "properties": {
      "kinds": {
        "type": "array",
        "items": {
          "type": "string"
        }
      },
      "limit": {
        "type": "number"
      },
      "activeMinutes": {
        "type": "number"
      },
      "messageLimit": {
        "type": "number"
      }
    }
  }
}
```

## Tool: sessions_history
**Description**: Fetch message history for a session.

**JSON Schema**:
```json
{
  "name": "sessions_history",
  "description": "Fetch message history for a session.",
  "parameters": {
    "type": "object",
    "required": ["sessionKey"],
    "properties": {
      "sessionKey": {
        "type": "string"
      },
      "limit": {
        "type": "number"
      },
      "includeTools": {
        "type": "boolean"
      }
    }
  }
}
```

## Tool: sessions_send
**Description**: Send a message into another session. Use sessionKey or label to identify the target.

**JSON Schema**:
```json
{
  "name": "sessions_send",
  "description": "Send a message into another session. Use sessionKey or label to identify the target.",
  "parameters": {
    "type": "object",
    "required": ["message"],
    "properties": {
      "sessionKey": {
        "type": "string"
      },
      "label": {
        "type": "string"
      },
      "agentId": {
        "type": "string"
      },
      "message": {
        "type": "string"
      },
      "timeoutSeconds": {
        "type": "number"
      }
    }
  }
}
```

## Tool: sessions_spawn
**Description**: Spawn a background sub-agent run in an isolated session and announce the result back to the requester chat.

**JSON Schema**:
```json
{
  "name": "sessions_spawn",
  "description": "Spawn a background sub-agent run in an isolated session and announce the result back to the requester chat.",
  "parameters": {
    "type": "object",
    "required": ["task"],
    "properties": {
      "task": {
        "type": "string"
      },
      "label": {
        "type": "string"
      },
      "agentId": {
        "type": "string"
      },
      "model": {
        "type": "string"
      },
      "thinking": {
        "type": "string"
      },
      "runTimeoutSeconds": {
        "type": "number"
      },
      "timeoutSeconds": {
        "type": "number"
      },
      "cleanup": {
        "type": "string",
        "enum": ["delete", "keep"]
      }
    }
  }
}
```

## Tool: session_status
**Description**: Show a /status-equivalent session status card (usage + time + cost when available). Use for model-use questions (📊 session_status). Optional: set per-session model override (model=default resets overrides).

**JSON Schema**:
```json
{
  "name": "session_status",
  "description": "Show a /status-equivalent session status card (usage + time + cost when available). Use for model-use questions (📊 session_status). Optional: set per-session model override (model=default resets overrides).",
  "parameters": {
    "type": "object",
    "properties": {
      "sessionKey": {
        "type": "string"
      },
      "model": {
        "type": "string"
      }
    }
  }
}
```

## Tool: web_search
**Description**: Search the web using Brave Search API. Supports region-specific and localized search via country and language parameters. Returns titles, URLs, and snippets for fast research.

**JSON Schema**:
```json
{
  "name": "web_search",
  "description": "Search the web using Brave Search API. Supports region-specific and localized search via country and language parameters. Returns titles, URLs, and snippets for fast research.",
  "parameters": {
    "type": "object",
    "required": ["query"],
    "properties": {
      "query": {
        "description": "Search query string.",
        "type": "string"
      },
      "count": {
        "description": "Number of results to return (1-10).",
        "type": "number"
      },
      "country": {
        "description": "2-letter country code for region-specific results (e.g., 'DE', 'US', 'ALL'). Default: 'US'.",
        "type": "string"
      },
      "search_lang": {
        "description": "ISO language code for search results (e.g., 'de', 'en', 'fr').",
        "type": "string"
      },
      "ui_lang": {
        "description": "ISO language code for UI elements.",
        "type": "string"
      },
      "freshness": {
        "description": "Filter results by discovery time (Brave only). Values: 'pd' (past 24h), 'pw' (past week), 'pm' (past month), 'py' (past year), or date range 'YYYY-MM-DDtoYYYY-MM-DD'.",
        "type": "string"
      }
    }
  }
}
```

## Tool: web_fetch
**Description**: Fetch and extract readable content from a URL (HTML → markdown/text). Use for lightweight page access without browser automation.

**JSON Schema**:
```json
{
  "name": "web_fetch",
  "description": "Fetch and extract readable content from a URL (HTML → markdown/text). Use for lightweight page access without browser automation.",
  "parameters": {
    "type": "object",
    "required": ["url"],
    "properties": {
      "url": {
        "description": "HTTP or HTTPS URL to fetch.",
        "type": "string"
      },
      "extractMode": {
        "type": "string",
        "enum": ["markdown", "text"],
        "description": "Extraction mode (\"markdown\" or \"text\").",
        "default": "markdown"
      },
      "maxChars": {
        "description": "Maximum characters to return (truncates when exceeded).",
        "type": "number"
      }
    }
  }
}
```

## Tool: image
**Description**: Analyze an image with the configured image model (agents.defaults.imageModel). Provide a prompt and image path or URL.

**JSON Schema**:
```json
{
  "name": "image",
  "description": "Analyze an image with the configured image model (agents.defaults.imageModel). Provide a prompt and image path or URL.",
  "parameters": {
    "type": "object",
    "required": ["image"],
    "properties": {
      "prompt": {
        "type": "string"
      },
      "image": {
        "type": "string"
      },
      "model": {
        "type": "string"
      },
      "maxBytesMb": {
        "type": "number"
      }
    }
  }
}
```

## Tool: memory_search
**Description**: Mandatory recall step: semantically search MEMORY.md + memory/*.md (and optional session transcripts) before answering questions about prior work, decisions, dates, people, preferences, or todos; returns top snippets with path + lines.

**JSON Schema**:
```json
{
  "name": "memory_search",
  "description": "Mandatory recall step: semantically search MEMORY.md + memory/*.md (and optional session transcripts) before answering questions about prior work, decisions, dates, people, preferences, or todos; returns top snippets with path + lines.",
  "parameters": {
    "type": "object",
    "required": ["query"],
    "properties": {
      "query": {
        "type": "string"
      },
      "maxResults": {
        "type": "number"
      },
      "minScore": {
        "type": "number"
      }
    }
  }
}
```

## Tool: memory_get
**Description**: Safe snippet read from MEMORY.md or memory/*.md with optional from/lines; use after memory_search to pull only the needed lines and keep context small.

**JSON Schema**:
```json
{
  "name": "memory_get",
  "description": "Safe snippet read from MEMORY.md or memory/*.md with optional from/lines; use after memory_search to pull only the needed lines and keep context small.",
  "parameters": {
    "type": "object",
    "required": ["path"],
    "properties": {
      "path": {
        "type": "string"
      },
      "from": {
        "type": "number"
      },
      "lines": {
        "type": "number"
      }
    }
  }
}
```