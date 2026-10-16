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

## 2c. Edited files: truncated in chat, and invisible outside the workspace

- When Copilot edits a Markdown file, Chat View shows the edit but truncates it.
  Ask: a way to open the full file, a rendered preview, or a side panel.
- If the edited file is outside the workspace (for example under `~/`), it
  doesn't appear in the diff view or the files list, so there's no way to see
  what changed. Ask: show files that were edited by a session even when they're
  outside the project root, using the paths from the `tool.execution_*` events.

## 2d. Chat View and Term View issues

- **Duplicate messages:** Copilot chat mode sometimes shows messages twice
  (may be visual only).
- **Term view cursor:** you can't tap in the middle of the prompt to move the cursor.
- **Formatting lost when sending from chat to the terminal:** line breaks and
  spacing aren't preserved, so text runs together. This breaks slash commands
  and URLs.
- **Multi-step tabbed questions:** when Copilot asks a multi-step (tabbed)
  question, Chat View only shows an approval prompt. Nothing says the
  terminal needs more input. I only found out by accident. Ask: detect this
  state and show "needs input in terminal" with a jump-to-terminal button.
- **Chat to Term switch:** after flipping from Chat to Term, you still have
  to tap Reconnect. It should reconnect automatically.

## 2e. React error #185 in the app

Got this error in the Moshi UI (2026-10-06 14:14 CDT):

> Minified React error #185; visit https://react.dev/errors/185 for the full
> message or use the non-minified dev environment for full errors and
> additional helpful warnings.

React #185 is "Maximum update depth exceeded": a component keeps calling
setState in an effect or render, causing an infinite update loop. That could
also explain the slowdown in section 3 (a render loop burns CPU and memory
until the app is restarted). Context: Moshi locked up, so I quit and relaunched it. On relaunch the app was
blank, with this error shown at the top. So the render loop seems to be hit at
startup, possibly from restored state, which would mean a relaunch doesn't
clear it. A dev build or the component stack would pin it down.

## 2f. Can't sort or reorder pinned sessions

I couldn't find a way to sort or reorder pinned sessions, and the docs don't
mention one. The docs only say saved connections can be reordered from the home
list (docs/connections). Request: let pinned sessions be dragged into a custom
order, or sorted by name or most recent activity.

## 2g. Chat View: long comment/reply doesn't send

When I reply in Chat View using the comment feature and the message is large,
pressing Send does nothing: no error, no sign it was rejected. When I switch to
Term View, the full text is sitting in the Copilot CLI input box, but the Enter
that should submit it was never delivered. Short replies send fine.

My guess, not confirmed: the text is pasted into the terminal and the Enter is
sent right after. With a large paste, the CLI is still handling the paste (or
shows it as a collapsed "pasted text" block), so the Enter is dropped or
swallowed. Request: send Enter after a short delay or after the paste finishes,
and show an error or keep the draft if the send fails.

## 2h. Agent status and pinning should work the same on every app and host

Idle / done / waiting status and pinned sessions don't behave the same across
the mobile, web, and desktop apps. They also differ depending on which host
view I'm in, and in the "all hosts" view.

Request:
- Show idle, done, and waiting status for every agent on mobile, web, and desktop.
- Let me pin sessions on all three, and keep pinned sessions in sync between them.
- Show the same status and pins in the "all hosts" view as in a single-host view.

(Related: item 2f asks for sorting pinned sessions, and pinned agents
currently don't show a working status.)

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
