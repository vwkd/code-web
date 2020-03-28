# Nano

[TOC]

## Introduction

- simple text editor for command line
- macOS ships with it by default, but old version, might not support all keyboard shortcuts below, e.g. Undo, Redo, Save without prompt



## Keyboard Shortcuts

### Notation

- `^` is the control key, e.g. <kbd>CTRL</kbd> on Mac
- `M-` is the meta key, e.g. <kbd>ESC</kbd> on Mac (or <kbd>ALT</kbd> if "Use option as meta key" is enabled in Terminal &gt; Settings &gt; Profiles &gt; Keyboard)
- only uses lower case letters, e.g. `M-U` means <kbd>ESC</kbd> + <kbd>u</kbd>

### Shortcuts

| Keyboard shortcut    | Effect                            |
| -------------------- | --------------------------------- |
| `^G`                 | Show help                         |
| `^O`, `^S`           | Save with(out) prompt             |
| `^X`                 | Exit                              |
| `^C`                 | Cancel                            |
| ▲, `^P`              | Move up                           |
| ▼, `^N`              | Move down                         |
| ◀︎, `^B`              | Move left                         |
| ▶︎, `^N`              | Move up                           |
| `^A` / `^E`          | Jump to start / end of line       |
| `^Space` / `M-Space` | Jump forward / backward one word  |
| `^V` / `^Y`          | Jump down / up one page           |
| `^_`                 | Jump to line number               |
| `M-A`                | Start marking text                |
| `M-6`                | Copy marked region / current line |
| `^K`                 | Cut marked region / current line  |
| `^U`                 | Paste                             |
| `M-U` / `M-E`        | Undo / redo last operation        |