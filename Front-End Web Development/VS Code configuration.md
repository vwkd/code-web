# Visual Studio Code settings

[TOC]



## Setup

- Activate Command line integration



## Settings

### Text Editor

- Disable _Drag And Drop_
- Set _Render Whitespace_ to _selection_.
- Set _Cursor Blinking_ to _phase_.
- Enable _Cursor Smooth Caret Animation_.
- Set _Auto Safe_ to _afterDelay_.
- Disable _Minimap_.
- Set _Tab Completion_ to _on_.
- Disable _Lightbulb_.

### Workbench

- Disable _Activity Bar: Visible_
- Disable _Breadcrumbs_
- Disable _Zen Mode: Center Layout_
- Disable _Editor: Enable Preview_

### Window

- Enable _Auto Detect Color Scheme_



## Interface settings

- Disable _Open Editors_, _Outline_ and _NPM Scripts_ in Explorer view.



## Custom settings

Add the following to `settings.json`.

### Markdown

- Enable suggestions except for normal words
- Set tab to 2 spaces instead of default 4.

```json
"[markdown]": {
    "editor.quickSuggestions": true,
    "editor.suggest.showWords": false,
    // "editor.acceptSuggestionOnEnter": "off",
    "editor.tabSize": 2
}
```



## Keyboard shortcuts

- Set _Toggle Block Comments_ to CMD + ALT + /
- Set _Reset Zoom_ to CMD + SHIFT + 0
- Set _Go Forward_ to CMD + K then CMD + .
- Set _Go Back_ to CMD + K then CMD + ,
- Add Visual Studio Code to System settings > Keyboard > Shortcuts > Function Keys
- Create `~/Library/KeyBindings/DefaultKeyBinding.dict` to disable beeping for shortcuts CTRL + CMD + ↓ / ← / → ([stackexchange.com](https://apple.stackexchange.com/a/260566/54870))

```javascript
{
    "^@\UF701" = "noop:";
    "^@\UF702" = "noop:";
    "^@\UF703" = "noop:";
}
```



## Backup

- Save user config from `~/Library/Application Support/Code/User/`



## Fonts

- JetBrains Mono
- Fira Code
- activate font ligatures



## Extensions

- Chrome Debugger
- ESLint
- LiveServer

### ESLint configuration

- Set _ESLint: Run_ to _onSave_.

### LiveServer configuration

- Set _Live Server: Custom Browser_ to _Chrome_.
- Enable _Do not show Info messages_.
- Enable _Do not verify tags_.
- Set _Host_ to `localhost`
