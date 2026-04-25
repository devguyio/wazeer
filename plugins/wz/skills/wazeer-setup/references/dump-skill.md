---
name: wz:dump
description: Quick brain dump to Inbox with AI-inferred metadata. Zero friction GTD capture. Use when user says "dump", "capture this", "brain dump", "quick thought", "remember this", "jot down", or provides a raw thought to capture.
---

## Synopsis
```
/wz:dump <anything on your mind>
```

## Description
GTD-style quick capture. Dump a thought, task, idea, or reminder. Wazeer adds metadata and appends to Inbox.md. No friction, no questions.

## Vault Inbox Path

Inbox: {{VAULT_PATH}}/_wazeer/Inbox.md

## Implementation

1. Get context:
   ```bash
   date "+%Y-%m-%d %H:%M"
   pwd
   git branch --show-current 2>/dev/null
   ```

2. Analyze the input and infer relevant metadata:
   - No fixed categories - infer whatever is relevant to THIS specific dump
   - Examples: people mentioned, deadlines, project/area, emotion, action type
   - Keep it concise - only what helps recall later

3. Append to the Inbox path specified above:
   ```
   - [ ] [the dump content]
     - *[timestamp] | [directory] ([branch if any])*
     - [inferred metadata as pipe-separated tags]
   ```

4. Show confirmation:
   ```
   Captured:
   -> "[the dump content]"
     [inferred metadata]
   ```

## Examples

Input: `need to follow up with manager about release`
```
Captured:
-> "need to follow up with manager about release"
  people: manager | follow-up | release
```

Input: `buy milk`
```
Captured:
-> "buy milk"
  errand | home
```

Input: `what if we used Redis instead?`
```
Captured:
-> "what if we used Redis instead?"
  idea | architecture | caching
```
