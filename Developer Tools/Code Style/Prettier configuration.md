# Prettier configuration

[TOC]



## Introduction

- code formatter
- few options, easy to set up



## Installation

1. Install `prettier` locally.

    ```bash
    npm install --save-dev prettier
    ```

1. If `eslint` is used install plugin to turn off conflicting rules.

    ```bash
    npm install --save-dev eslint-config-prettier
    ```

1. If needed install editor extension.



## Configuration

- use a configuration file `.prettierrc.json`
- if `eslint` is used, add `"prettier"` in `"extends"` in `.eslintrc.json`, also `"prettier/@typescript-eslint"` if typescript is installed

<!-- ToDo: Markdown 3 spaces between second level header -->