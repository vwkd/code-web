# ESLint configuration

[TOC]

## Introduction

- powerful syntax and code style checker for JavaScript, e.g. can enforce "The Good Parts" of JavaScript
- can be used to enforce style guide as well, e.g. consistent spacing, but better use separate formatter


## Installation

1. Install `eslint` locally. Install editor extension if needed.

    ```bash
    npm install eslint
    ```

1. If needed install `typescript` locally and support for eslint

    ```bash
    npm install --save-dev typescript @typescript-eslint/parser @typescript-eslint/eslint-plugin
    ```

1. Create a `.eslintrc.json` file locally. Select support for TypeScript if needed. Choose the recommended rule set.

    ```bash
    npx eslint --init 
    ```

1. Configure editor extension to only check on save instead of on type.


## Configuration

- use a configuration file `.eslintrc.json`
- setup

```json
"env": {
    "browser": true,
    "es6": true
},
"parser": "@typescript-eslint/parser",
"parserOptions": {
    "ecmaVersion": 2020,
    "sourceType": "module"
},
"plugins": [
    "@typescript-eslint"
],
"extends": [
    "eslint:recommended",
    "plugin:@typescript-eslint/eslint-recommended",
    "plugin:@typescript-eslint/recommended"
],
"root": true
```

- rules

```json
// standard rules
"indent": [
    "error",
    4
],
"linebreak-style": [
    "error",
    "unix"
],
"quotes": [
    "error",
    "double"
],
"semi": [
    "error",
    "always"
],
// possible errors
"no-template-curly-in-string": [
    "error"
],
// best practices
"class-methods-use-this": [
    "error"
],
"consistent-return": [
    "error"
],
"curly": [
    "error",
    "all"
],
"default-case": [
    "error"
],
/* "default-case-last": [
    "error"
], */
"default-param-last": [
    "error"
],
"dot-location": [
    "error",
    "property"
],
"dot-notation": [
    "error"
],
/* "eqeqeq": [
    "error",
    "smart"
], */
"grouped-accessor-pairs": [
    "error"
],
"no-alert": [
    "error"
],
"no-caller": [
    "error"
],
"no-else-return": [
    "error"
],
"no-empty-function": [
    "error"
],
"no-eval": [
    "error"
],
"no-extend-native": [
    "error"
],
"no-extra-bind": [
    "error"
],
"no-extra-label": [
    "error"
],
"no-implied-eval": [
    "error"
],
"no-invalid-this": [
    "error"
],
"no-lone-blocks": [
    "error"
],
"no-loop-func": [
    "error"
],
"no-multi-str": [
    "error"
],
"no-new": [
    "error"
],
"no-new-func": [
    "error"
],
"no-new-wrappers": [
    "error"
],
"no-octal-escape": [
    "error"
],
"no-param-reassign": [
    "error"
],
"no-proto": [
    "error"
],
"no-return-assign": [
    "error"
],
"no-return-await": [
    "error"
],
"no-script-url": [
    "error"
],
"no-self-compare": [
    "error"
],
"no-throw-literal": [
    "error"
],
"no-unmodified-loop-condition": [
    "error"
],
"no-unused-expressions": [
    "error"
], // consider disabling if to tense
"no-useless-call": [
    "error"
],
"no-useless-concat": [
    "error"
],
"no-useless-return": [
    "error"
],
"no-void": [
    "error"
],
"prefer-promise-reject-errors": [
    "error"
],
"prefer-regex-literals": [
    "error"
],
"require-await": [
    "error"
],
"wrap-iife": [
    "error",
    "outside"
],
"yoda": [
    "error"
],
"no-restricted-syntax": [
    "error",
    "SequenceExpression"
], // disallow sequences completely
// strict mode
"strict": [
    "error",
    "global"
],
// variables
"no-label-var": [
    "error"
],
/* "no-shadow": [
    "error",
    {
        "builtinGlobals": true,
        "hoist": "functions"
    }
], */
"no-undef-init": [
    "error"
],
"no-undefined": [
    "error"
],
"no-use-before-define": [
    "error",
    {
        "variables": true,
        "functions": false,
        "classes": false
    }
],
// ES6
"no-duplicate-imports": [
    "error",
    {
        "includeExports": true
    }
],
"no-useless-computed-key": [
    "error",
    {
        "enforceForClassMembers": true
    }
],
"no-useless-rename": [
    "error"
],
"no-var": [
    "error"
],
"object-shorthand": [
    "error",
    "always",
    {
        "avoidExplicitReturnArrows": true
    }
],
"prefer-arrow-callback": [
    "error"
],
"prefer-const": [
    "error"
],
"prefer-destructuring": [
    "error",
    {"object": true, "array": false}
],
"prefer-numeric-literals": [
    "error"
],
"prefer-rest-params": [
    "error"
],
"prefer-spread": [
    "error"
],
"prefer-template": [
    "error"
],
"sort-imports": [
    "error"
], // consider disabling if to tense
"symbol-description": [
    "error"
],
// fix recommended rules
"no-inner-declarations": "off"
```