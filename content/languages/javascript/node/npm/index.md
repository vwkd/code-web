---
title: Node Package Manager (NPM)
author: vwkd
index: 2
tags:
  - languages
  - javascript
  - node
---

- package manager for JavaScript packages
- default package manager for Node.js runtime environment
- centralised registry of JavaScript packages



## Creating packages

### `package.json`

- contains package metadata as object, e.g. name, version, author, dependencies etc.
- can create interactively using `npm init`

#### `name` property

- package name, short but descriptive
- must be URL-safe characters, all lower-case, not start with underscore or dot
- must be unique in npm registry if package is unscoped

#### `version` property

- package version
- follows semver (semantiv versioning) system

| Release type | Version number | Convention                                |
| ------------ | -------------- | ----------------------------------------- |
| Patch        | \~1.1.x        | bug fix, backward compatible              |
| Minor        | \^1.x.y        | new features, backward compatible         |
| Major        | x.y.z          | changes that break backward compatibility |

- changes to the package should come with changes to the version
- no guarantee that other packages adhere to semver convention, i.e. a dependency of your package could introduce a non-backward compatible change in a minor or patch release, could break your package at any moment ⚠️

#### `main` property

- sets `.js` to be used when package is required

#### `bin` property

- sets `.js` to be used if package is invoked as script from command line
- script must have proper shebang to be invoked with Node instead of as script

```javascript
#!/usr/bin/env node
```

#### `dependencies` property

- specifies packages the package depends on
- are installed along when the package itself is installed, i.e. for each usual user

#### `devDependencies` property

- specifies packages the package depends on
- are installed only when executing `npm install` inside the package, i.e. not for usual user

### `index.js`

- default file to be used as executable of the package
- can be named differently as long as specified in `main` property in `package.json`
```javascript
'use strict';

/* ... */

module.exports = func1;
```

<!-- ToDo: Finish -->

### Set version number

```javascript
npm version <semantic-version>
```

- shortcut in editing version attribute in .json

### Publish

```javascript
npm publish
```

goes to `https://npmjs.com/package/<package>`

- by default is tagged with latest, can tag as beta, etc.

### Deprecate

```javascript
npm deprecate <package-name>@<version> "<message>"
```

- `<version>` is optional, latest by default ???




## Using packages

### Other commands

```javascript
npm list / ls
npm search
```

### Installation

```javascript
npm install / i <package-name>@<version>
```

- can specify version ranges of semver, see [semver.org](https://semver.org/)
- version is optional, latest is used by default, if inside another package and the package is inside `package.json` then this version is used by default
- if inside another package, i.e. with `package.json`, automatically adds package to `package.json`, use `--save-dev` to add only as dev dependency, uses minor version range "\^" by default
- if inside another package and no package-name is supplied, installs all packages as specified in `package.json`, if `--production` flag then only the non dev dependencies, if `package-lock.js` is present it is used instead of `package.json`, i.e. guarantees exact dependency tree
- by default installs package(s) locally in `./node_modules` and bins into `./node_modules/.bin` within current folder or if inside package in root of package, in that folder can require in .js file or run with `npx` from command line
- `-g` installs package(s) globally in `<node-path>/lib/node_modules` and bins into `<node-path>/bin` where, e.g. `/usr/local/`, can run in command line from anywhere

### NPM Dependency Model

- installing a packages creates a dependency tree, each package has own dependencies, those get installed in the packages' own `node_modules` directories, and the dependencies have their dependencies etc. pp.
- this allows two multiple packages to have different versions of same dependency, no dependency conflicts, but problem because dependencies might be duplicated, eat a lot of space, or worse infinite recursion would happen for two inderdependent packages
- NPM puts first package and all its dependencies on top level, second package and all it's dependencies as well except the duplicate dependencies, different version becomes nested while same versions are simply skipped, also skips if existing version is within specified semver range even though a newer version might be available, i.e. uses "oldest common denominator" of a dependency
- packages can still take up a lot of space, if first package depends on one version of another package, and all following packages on another versions of that other package, because then the first version of the other package occupies already the root level of the `.node_modules` folder, and all other versions need to be nested even if they are the same, it all depends on installation order, i.e. multiple copies of exact same dependencies can exist
- even if fixes versions in `package.json` might get different installs on different times because dependencies themselves likely won't have their dependencies fixed etc., if those dependencies don't follow semrev guidelines might break the app because new version introduced incompatibility, could have a bug even if fixed own dependencies, `npm install` is russian roulette
- `package-lock.js` stores exact versions of whole dependency tree using hashes, is generated automatically as soon as NPM changes the dependency packages, makes dependency tree reproducible, can reproduce exact package regardeless of intermediate dependency updates, package is "locked", should always be included in source control, but can't be published with a package since it overwrites `package.json` disabling any intermediate dependency updates, can use `npm-shrinkwrap.json` as "publishable `package-lock.json`" but not recommended

### Update

```javascript
npm update
```

- can update specific package, all packages, local, global
- check with npm outdated if update was successful

### Dedupe

```javascript
npm dedupe
```

- flattens dependency tree as much as possible, moves dependencies up the tree, removes duplicates if found
- doesn't solve problem that one dependency version blocks many dependency of another version

### Uninstall

```javascript
npm uninstall <package-name>
```

- uninstall package locally or globally
- remove from (dev) dependency list using --save or --save-dev



## Resources