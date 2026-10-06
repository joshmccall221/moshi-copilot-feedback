# Copilot CLI keyboard shortcuts (from `/help`, v1.0.92)

## Suggested mobile shortcut bar
| Label | Keys |
|---|---|
| Mode | `Shift+Tab` |
| Esc | `Esc` (and `Esc Esc`: clear input, interrupt, rewind) |
| Cancel | `Ctrl+C` (twice exits) |
| Complete | `Tab` |
| Newline | `Shift+Enter` |
| Timeline | `Ctrl+O` |
| Background | `Ctrl+X` then `B` (chord) |
| Queue | `Ctrl+Q` |
| Stash | `Ctrl+S` |
| History | `Ctrl+R` |

## Full list
Global: `Shift+Tab` switch mode, `Ctrl+S` stash/pop prompt, `Ctrl+Q` enqueue prompt,
`Ctrl+R` reverse history search, `Ctrl+O` toggle timeline, `Ctrl+C` cancel (x2 exit),
`Esc Esc`, `Ctrl+D` shutdown, `Ctrl+Z` suspend, `Ctrl+L` clear screen,
`Ctrl+T` toggle reasoning, `Ctrl+X B` background task, `Ctrl+X G` goal panel,
`Ctrl+X O` open last link.

Input editing: `Ctrl+A/E` line start/end, `Ctrl+H` delete char, `Ctrl+W` delete word,
`Ctrl+U/K` delete to start/end, `Meta+Left/Right` word move, `Shift+Enter` newline,
`Ctrl+G` edit in `$EDITOR`.

## Notes
- Chords (`Ctrl+X` then key) need a "send sequence" button type.
- `Shift+Enter` only works if the terminal is configured (`/terminal-setup`);
  a dedicated newline button avoids that dependency.
