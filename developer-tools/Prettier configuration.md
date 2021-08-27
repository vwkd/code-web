# Prettier configuration

[TOC]



## Introduction

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