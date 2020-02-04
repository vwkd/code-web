# IntelliJ WebStorm configuration

[TOC]

(necessary config for IntelliJ WebStorm on macOS with German keyboard)

## Keymap

- Set _Find Action_ to <kbd>CMD</kbd> + <kbd>Shift</kbd> + <kbd>Space</kbd> to not conflict with searching man page index in terminal, [jetbrains.com](https://intellij-support.jetbrains.com/hc/en-us/articles/360005137400-Cmd-Shift-A-hotkey-opens-Terminal-with-apropos-search-instead-of-the-Find-Action-dialog)
- Set _Comment with Line_ / _Block Comment_ to <kbd>CMD</kbd> + (<kbd>Shift</kbd>) + <kbd>ö</kbd>
- Set _Select Previous_ / _Next Tab_ to <kbd>CMD</kbd> + <kbd>Shift</kbd> + <kbd>1</kbd>/<kbd>2</kbd>
- Reassign _Expand (all)_ and _Collapse (all)_ to <kbd>CMD</kbd> + <kbd>+</kbd>/<kbd>-</kbd>
- Set _Go to previous_ / _next method_ to  <kbd>ALT</kbd> + <kbd>1</kbd>/<kbd>2</kbd>

## Editor

### General

- Disable _Enable Drag'n'Drop functionality in editor_



### Inspections

- disable _Equality operator may cause type coercion_



### File and Code Templates

#### HTML

```html
<!DOCTYPE html>
<html lang="en" dir="ltr">

<head>
  <meta charset="UTF-8">
  <title>#[[$Title$]]#</title>
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
</head>

<body>
#[[$END$]]#
</body>

</html>
```

#### JavaScript

```javascript
"use strict";
$END
```



## Plugins

- presentation assistant
- Material Theme UI



## Tools

- disable Opera and IE browsers