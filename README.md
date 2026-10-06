# Moshi feedback: GitHub Copilot CLI in Chat View

Feedback for the Moshi team. Clean-room: based only on Moshi's public docs
(`rjyo/homebrew-moshi/docs/hooks.md`) and Copilot CLI's own `/help` output.
No Moshi binaries were decompiled or inspected.

Environment: Moshi desktop (Intel iMac19,1, macOS 15.7.9), Mac mini (Apple Silicon),
moshi-hook 0.4.18, GitHub Copilot CLI 1.0.92.

## 1. Slash commands in Chat View

`hooks.md` says Copilot gets Chat View via its `events.jsonl`. Slash commands
are a core part of the Copilot workflow, but there is no way to discover or
pick one from the chat input.

Ask:
- Typing `/` opens a palette (fuzzy filter, description shown) like the CLI does.
- Also `@` (file mention), `#` (issue/PR mention), `!` (shell command).
- Commands that open interactive pickers (`/model`, `/resume`, `/agent`) should
  fall back to the terminal view, or render a picker.

See [slash-commands.md](slash-commands.md) for the full list, grouped by how
likely they are to be used from a phone.

## 2. Keyboard shortcuts / toolbar keys

See [shortcuts.md](shortcuts.md). Most important for mobile: **Shift+Tab**
(cycle mode), **Esc / Esc Esc**, **Ctrl+C**, **Tab**, **Shift+Enter**
(newline), **Ctrl+X then B** (background task), **Ctrl+O**.

Ask: a configurable shortcut bar, including chords (Ctrl+X then a key).

## 2b. Session/agent names don't match `/rename`

The agent name Moshi shows for a Copilot session doesn't match the name set
with Copilot's `/rename` (or the auto-generated name). The **pinned** agent's
name also doesn't match the Copilot agent's name. Ask: use the Copilot
session name as the display title for both the list and pinned entries, and
update it when it changes.

## 3. Performance bug: Mac app slows down until restarted

The Moshi Mac app gradually becomes slow and has to be quit and relaunched.
Not yet profiled. Happy to capture a sample (`Activity Monitor > Sample Process`)
next time it happens, and to say what was open (Chat View sessions, long
Copilot sessions, etc.). The crash report below may be related (a 2.7G
MALLOC footprint, 8.8G total VM at crash time).

## 4. Crash: file picker

[crash-summary.md](crash-summary.md): SIGABRT on the main thread inside
WebKit `runOpenPanel` when a file chooser opens.

## 5. Chat View data shape

[events-sample.md](events-sample.md): event types Copilot writes to
`events.jsonl` (synthetic content), including where slash commands appear.
