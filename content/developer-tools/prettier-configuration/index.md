---
title: Prettier configuration
author: vwkd
index: 6
tags:
  - developer-tools
---

- code formatter
- few options, easy to set up



## Installation

1. Install `prettier` locally or use editor extension.

    ```bash
    npm install --save-dev prettier
    ```

1. If `eslint` is used before formatting with Prettier, install plugin to turn off conflicting rules.

    ```bash
    npm install --save-dev eslint-config-prettier
    ```

1. Create a `.prettierrc.json` file locally.



## Configuration

- use a configuration file `.prettierrc.json`
- if `eslint` is used before formatting with Prettier, add `"prettier"` in `"extends"` in `.eslintrc.json`, also `"prettier/@typescript-eslint"` if typescript is installed

<!-- ToDo: Markdown 3 spaces between second level header -->



## Example

- `prettierrc.json`

```json
{
  "printWidth": 100,
  "tabWidth": 4,
  "endOfLine": "lf",
  "useTabs": false,

  "trailingComma": "none",
  "semi": true,
  "singleQuote": false,
  "bracketSpacing": true,
  "arrowParens": "avoid",

  "proseWrap": "never",

  "overrides": [
    {
      "files": ["*.json", "*.md"],
      "options": {
        "tabWidth": 2
      }
    }
  ]
}
```

- `prettierignore`

```text
# Markdown files

*.md
```