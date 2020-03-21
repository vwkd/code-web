# ESLint configuration

[TOC]



## Introduction

- most powerful linter for JavaScript
- scans source code, shows syntax errors, style convention errors, structural errors
- enforces "The Good Parts" of JavaScript, subset that makes sense
- computes an AST from source code
- can be used to enforce style guide as well, e.g. consistent spacing, but better use separate formatter



## Installation

1. Install `eslint` locally.

    ```bash
    npm install eslint
    ```

1. If needed install `typescript` locally and support for eslint

    ```bash
    npm install --save-dev typescript @typescript-eslint/parser @typescript-eslint/eslint-plugin
    ```

1. If needed install editor extension.

1. Create a `.eslintrc.json` file locally. Select support for TypeScript if needed. Choose the recommended rule set.

    ```bash
    npx eslint --init 
    ```

1. Configure editor extension to only check on save instead of on type.



## Configuration

- use a configuration file `.eslintrc.json`

<!-- ToDo: Add plugins, like eslint-plugin-es, eslint-plugin-node, eslint-plugin-promises, eslint-plugin-standard, build fully customised .eslintrc.json -->