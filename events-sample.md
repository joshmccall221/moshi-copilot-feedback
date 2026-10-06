# Copilot `events.jsonl` shape (synthetic content)

Located at `~/.copilot/session-state/<session-id>/events.jsonl`, one JSON object
per line: `{ "type", "data", "id", "timestamp", "parentId" }`.

Event types observed in a ~10 turn session (v1.0.92):

`session.start`, `session.model_change`, `session.permissions_changed`,
`session.usage_checkpoint`, `user.message`, `assistant.turn_start`,
`assistant.message`, `assistant.turn_end`, `tool.execution_start`,
`tool.execution_complete`, `hook.start`, `hook.end`, `system.message`,
`system.notification`, `model.*` (turn_started, model_call_started,
model_call_success, message, response, turn_ended, messages_snapshot,
captured_assignment_context).

Chat View probably only needs `user.message`, `assistant.message`, and
`tool.execution_*`. Hook and `model.*` rows are noise (the docs already say
hook status rows are hidden).

```json
{"type":"session.start","data":{"sessionId":"<uuid>","producer":"copilot-agent","copilotVersion":"1.0.92","selectedModel":"claude-sonnet-5.5","context":{"cwd":"/path","branch":"main"}},"id":"<uuid>","timestamp":"2026-01-01T00:00:00.000Z","parentId":null}
{"type":"user.message","data":{"content":"/model"},"id":"<uuid>","timestamp":"...","parentId":"<uuid>"}
{"type":"session.model_change","data":{"newModel":"..."},"id":"<uuid>","timestamp":"...","parentId":"<uuid>"}
```

Open question for Moshi: how are slash commands that change state
(`/model`, `/clear`, `/compact`) rendered in Chat View? In the log they show up
as `session.*` events, not as messages, so they may be invisible today.
(Field names above for `user.message`/`model_change` are illustrative; verify
against a real log.)
