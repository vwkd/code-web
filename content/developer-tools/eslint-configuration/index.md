---
title: ESLint configuration
author: vwkd
index: 5
tags:
  - developer-tools
---

- most powerful linter for JavaScript
- scans source code, shows syntax errors, style convention errors, structural errors
- enforces "The Good Parts" of JavaScript, subset that makes sense
- computes an AST from source code
- can be used to enforce formatting as well, e.g. consistent spacing, but better use separate formatter, e.g. Prettier



## Installation

1. Install `eslint` locally.

    ```bash
    npm install --save-dev eslint
    ```

1. If needed install `typescript` locally and support for eslint

    ```bash
    npm install --save-dev typescript @typescript-eslint/parser @typescript-eslint/eslint-plugin
    ```

1. If needed install editor extension to automatically run checks while developing. Don't install, if only wants to check after development.

1. Create a `.eslintrc.json` file locally. Select support for TypeScript if needed. Choose the recommended rule set.

    ```bash
    npx eslint --init 
    ```



## Configuration

- use a configuration file `.eslintrc.json`

<!-- ToDo: Add plugins, like eslint-plugin-es, eslint-plugin-node, eslint-plugin-promises, eslint-plugin-standard -->



## Example

- `eslintrc.json`

```json
{
  "env": {
    "browser": true,
    "es2020": true,
    "node": true
  },
  "extends": [
    "plugin:@typescript-eslint/eslint-recommended",
    "plugin:@typescript-eslint/recommended"
  ],
  "globals": {},
  "parser": "@typescript-eslint/parser",
  "parserOptions": {
    "ecmaVersion": 2020,
    "sourceType": "module"
  },
  "plugins": ["@typescript-eslint"],
  "root": true,
  "rules": {
    // Possible Errors
    "for-direction": ["warn"],
    "getter-return": ["warn"],
    "no-async-promise-executor": ["warn"],
    "no-await-in-loop": ["warn"],
    "no-compare-neg-zero": ["warn"],
    "no-cond-assign": ["warn"],
    "no-console": ["warn"],
    "no-constant-condition": ["warn"],
    "no-control-regex": ["warn"],
    "no-debugger": ["warn"],
    "no-dupe-args": ["warn"],
    "no-dupe-else-if": ["warn"],
    "no-dupe-keys": ["warn"],
    "no-duplicate-case": ["warn"],
    "no-empty": ["warn"],
    "no-empty-character-class": ["warn"],
    "no-ex-assign": ["warn"],
    "no-extra-boolean-cast": ["warn"],
    "no-extra-parens": ["warn"],
    "no-extra-semi": ["warn"],
    "no-func-assign": ["warn"],
    "no-import-assign": ["warn"],
    "no-inner-declarations": ["warn"],
    "no-invalid-regexp": ["warn"],
    "no-irregular-whitespace": ["warn"],
    "no-misleading-character-class": ["warn"],
    "no-obj-calls": ["warn"],
    "no-prototype-builtins": ["warn"],
    "no-regex-spaces": ["warn"],
    "no-setter-return": ["warn"],
    "no-sparse-arrays": ["warn"],
    "no-template-curly-in-string": ["warn"],
    "no-unexpected-multiline": ["warn"],
    "no-unreachable": ["warn"],
    "no-unsafe-finally": ["warn"],
    "no-unsafe-negation": ["warn"],
    "no-useless-backreference": ["warn"],
    "require-atomic-updates": ["warn"],
    "use-isnan": ["warn"],
    "valid-typeof": ["warn"],
    // Best Practices
    "accessor-pairs": ["warn"],
    "array-callback-return": ["warn"],
    "block-scoped-var": ["warn"],
    "class-methods-use-this": ["warn"],
    "complexity": ["off"],
    "consistent-return": ["warn"],
    "curly": ["warn", "all"],
    "default-case": ["warn"],
    "default-case-last": ["warn"],
    "default-param-last": ["warn"],
    "dot-location": ["warn", "property"],
    "dot-notation": ["warn"],
    "eqeqeq": ["warn", "smart"],
    "grouped-accessor-pairs": ["warn"],
    "guard-for-in": ["warn"],
    "max-classes-per-file": ["off"],
    "no-alert": ["warn"],
    "no-caller": ["warn"],
    "no-case-declarations": ["warn"],
    "no-constructor-return": ["warn"],
    "no-div-regex": ["warn"],
    "no-else-return": ["off"],
    "no-empty-function": ["warn"],
    "no-empty-pattern": ["warn"],
    "no-eq-null": ["off"],
    "no-eval": ["warn"],
    "no-extend-native": ["warn"],
    "no-extra-bind": ["warn"],
    "no-extra-label": ["warn"],
    "no-fallthrough": ["warn"],
    "no-floating-decimal": ["warn"],
    "no-global-assign": ["warn"],
    "no-implicit-coercion": ["warn"],
    "no-implicit-globals": ["warn"],
    "no-implied-eval": ["warn"],
    "no-invalid-this": ["warn"],
    "no-iterator": ["warn"],
    "no-labels": ["warn"],
    "no-lone-blocks": ["warn"],
    "no-loop-func": ["warn"],
    "no-magic-numbers": ["off"],
    "no-multi-spaces": ["off"],
    "no-multi-str": ["warn"],
    "no-new": ["warn"],
    "no-new-func": ["warn"],
    "no-new-wrappers": ["warn"],
    "no-octal": ["warn"],
    "no-octal-escape": ["warn"],
    "no-param-reassign": ["warn"],
    "no-proto": ["warn"],
    "no-redeclare": ["warn"],
    "no-restricted-properties": ["off"],
    "no-return-assign": ["warn"],
    "no-return-await": ["warn"],
    "no-script-url": ["warn"],
    "no-self-assign": ["warn"],
    "no-self-compare": ["warn"],
    "no-sequences": ["warn"],
    "no-throw-literal": ["warn"],
    "no-unmodified-loop-condition": ["warn"],
    "no-unused-expressions": ["warn"],
    "no-unused-labels": ["warn"],
    "no-useless-call": ["warn"],
    "no-useless-catch": ["warn"],
    "no-useless-concat": ["warn"],
    "no-useless-escape": ["warn"],
    "no-useless-return": ["warn"],
    "no-void": ["warn"],
    "no-warning-comments": ["off"],
    "no-with": ["warn"],
    "prefer-named-capture-group": ["off"],
    "prefer-promise-reject-errors": ["warn"],
    "prefer-regex-literals": ["warn"],
    "radix": ["warn"],
    "require-await": ["warn"],
    "require-unicode-regexp": ["warn"],
    "vars-on-top": ["off"],
    "wrap-iife": ["warn", "outside"],
    "yoda": ["warn"],
    // Strict Mode
    "strict": ["warn"],
    // Variables
    "init-declarations": ["off"],
    "no-delete-var": ["warn"],
    "no-label-var": ["warn"],
    "no-restricted-globals": ["off"],
    "no-shadow": ["warn"],
    "no-shadow-restricted-names": ["warn"],
    "no-undef": ["warn", { "typeof": true }],
    "no-undef-init": ["warn"],
    "no-undefined": ["off"],
    "no-unused-vars": ["warn"],
    "no-use-before-define": ["warn", { "functions": false, "classes": false, "variables": false }],
    // Node.js and CommonJS
    "callback-return": ["warn", ["callback", "cb", "next", "done"]],
    "global-require": ["warn"],
    "handle-callback-err": ["warn", "err"],
    "no-buffer-constructor": ["warn"],
    "no-mixed-requires": ["warn"],
    "no-new-require": ["warn"],
    "no-path-concat": ["warn"],
    "no-process-env": ["off"],
    "no-process-exit": ["warn"],
    "no-restricted-modules": ["off"],
    "no-sync": ["warn"],
    // Stylistic Issues
    "array-bracket-newline": ["off"],
    "array-bracket-spacing": ["off"],
    "array-element-newline": ["off"],
    "block-spacing": ["off"],
    "brace-style": ["off"],
    "camelcase": ["warn"],
    "capitalized-comments": ["warn"],
    "comma-dangle": ["off"],
    "comma-spacing": ["off"],
    "comma-style": ["off"],
    "computed-property-spacing": ["off"],
    "consistent-this": ["warn", "that"],
    "eol-last": ["off"],
    "func-call-spacing": ["off"],
    "func-name-matching": ["off"],
    "func-names": ["warn", "as-needed"],
    "func-style": ["warn", "declaration"],
    "function-call-argument-newline": ["off"],
    "function-paren-newline": ["off"],
    "id-blacklist": ["off"],
    "id-length": ["off"],
    "id-match": ["off"],
    "implicit-arrow-linebreak": ["off"],
    "indent": ["warn", 4],
    "jsx-quotes": ["off"],
    "key-spacing": ["off"],
    "keyword-spacing": ["off"],
    "line-comment-position": ["warn", { "position": "above" }],
    "linebreak-style": ["warn", "unix"],
    "lines-around-comment": ["off"],
    "lines-between-class-members": ["off"],
    "max-depth": ["off"],
    "max-len": ["warn", { "code": 100, "ignoreRegExpLiterals": true }],
    "max-lines": ["off"],
    "max-lines-per-function": ["off"],
    "max-nested-callbacks": ["off"],
    "max-params": ["off"],
    "max-statements": ["off"],
    "max-statements-per-line": ["off"],
    "multiline-comment-style": ["warn", "starred-block"],
    "multiline-ternary": ["off"],
    "new-cap": ["warn"],
    "new-parens": ["warn"],
    "newline-per-chained-call": ["off"],
    "no-array-constructor": ["warn"],
    "no-bitwise": ["warn"],
    "no-continue": ["warn"],
    "no-inline-comments": ["off"],
    "no-lonely-if": ["warn"],
    "no-mixed-operators": ["warn"],
    "no-mixed-spaces-and-tabs": ["warn"],
    "no-multi-assign": ["warn"],
    "no-multiple-empty-lines": ["warn", { "max": 2, "maxBOF": 1, "maxEOF": 1 }],
    "no-negated-condition": ["warn"],
    "no-nested-ternary": ["off"],
    "no-new-object": ["warn"],
    "no-plusplus": ["warn"],
    "no-restricted-syntax": ["off"],
    "no-tabs": ["warn"],
    "no-ternary": ["off"],
    "no-trailing-spaces": ["warn"],
    "no-underscore-dangle": ["warn"],
    "no-unneeded-ternary": ["warn"],
    "no-whitespace-before-property": ["off"],
    "nonblock-statement-body-position": ["off"],
    "object-curly-newline": ["off"],
    "object-curly-spacing": ["off"],
    "object-property-newline": ["off"],
    "one-var": ["off"],
    "one-var-declaration-per-line": ["off"],
    "operator-assignment": ["warn", "always"],
    "operator-linebreak": ["off"],
    "padded-blocks": ["off"],
    "padding-line-between-statements": ["off"],
    "prefer-exponentiation-operator": ["warn"],
    "prefer-object-spread": ["warn"],
    "quote-props": ["off"],
    "quotes": ["warn", "double"],
    "semi": ["warn", "always"],
    "semi-spacing": ["off"],
    "semi-style": ["off"],
    "sort-keys": ["off"],
    "sort-vars": ["off"],
    "space-before-blocks": ["off"],
    "space-before-function-paren": ["off"],
    "space-in-parens": ["off"],
    "space-infix-ops": ["off"],
    "space-unary-ops": ["off"],
    "spaced-comment": ["warn", "always"],
    "switch-colon-spacing": ["off"],
    "template-tag-spacing": ["off"],
    "unicode-bom": ["off"],
    "wrap-regex": ["off"],
    // ECMAScript 6
    "arrow-body-style": ["warn", "as-needed"],
    "arrow-parens": ["warn", "as-needed"],
    "arrow-spacing": ["off"],
    "constructor-super": ["warn"],
    "generator-star-spacing": ["off"],
    "no-class-assign": ["warn"],
    "no-confusing-arrow": ["warn"],
    "no-const-assign": ["warn"],
    "no-dupe-class-members": ["warn"],
    "no-duplicate-imports": ["warn"],
    "no-new-symbol": ["warn"],
    "no-restricted-exports": ["off"],
    "no-restricted-imports": ["off"],
    "no-this-before-super": ["warn"],
    "no-useless-computed-key": ["warn", { "enforceForClassMembers": true }],
    "no-useless-constructor": ["warn"],
    "no-useless-rename": ["warn"],
    "no-var": ["warn"],
    "object-shorthand": ["warn", "always"],
    "prefer-arrow-callback": ["warn", { "allowNamedFunctions": true, "allowUnboundThis": true }],
    "prefer-const": ["warn"],
    "prefer-destructuring": ["warn", { "enforceForRenamedProperties": true }],
    "prefer-numeric-literals": ["warn"],
    "prefer-rest-params": ["warn"],
    "prefer-spread": ["warn"],
    "prefer-template": ["warn"],
    "require-yield": ["warn"],
    "rest-spread-spacing": ["off"],
    "sort-imports": ["off"],
    "symbol-description": ["warn"],
    "template-curly-spacing": ["off"],
    "yield-star-spacing": ["off"]
  }
}
```