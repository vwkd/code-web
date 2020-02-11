# NPM

[TOC]

## Terminology

### Modules

- `.js` file that contains export ??, i.e. can be imported, e.g. using `require()` in Node.js
- can also be package

### Packages

- folder containing a `package.json` and a .js file that is referenced as "main" attribute in `package.json`, by default `index.js`
- Packages are modules, but modules are not necessarily packages




## NPM (Node Package Manager)

- package manager for JavaScript packages
- default package manager for Node.js runtime environment
- centralised registry of JavaScript packages

<!-- Finish -->



## Creating packages

### `package.json`

- contains package meta data as objects, e.g.	name, version, main, dependencies, author, repository, bugs, license etc.
- can create interactively using `npm init`

#### `name`

- must be URL-safe characters, all lower-case, not start with underscore or dot
- must be unique in npm registry if package is unscoped
- short but descriptive

#### `version`

- follows semver (semantiv versioning) system

| Release | Rule | Meaning |
| ———- | —— | ————- |
| Patch | \~1.1.x | bug fix, backward compatible |
| Minor | ^1.x.y | new features, backward compatible |
| Major | x.y.z | changes that break backward compatibility |

- on installation later can also specify ranges, see [semver.org]
- changes to the package should come with changes to the version
- no guarantee that other packages adhere to Semver convention, i.e. a dependency of your package could introduce a non-backward compatible change and break your package at any moment

### `index.js`

- default ???
- can be specified in `main` attribute in `package.json`
-
```javascript
'use strict';

/* ... */

module.exports = func1;
```

### Set version number

npm version <semantic-version>

- shortcut in editing version attribute in .json

### Publish

npm publish

goes to https://npmjs.com/package/<package>

- by default is tagged with latest, can tag as beta, etc.


### Deprecate

npm deprecate <package-name>@<version> "<message>"

- <version> is optional, latest by default ???




## Using packages

### Other commands

npm list / ls
npm search


### Installieren

npm install <package-name>@<version>

- version is optional, latest is used by default, if inside another package with package.json the specified version is used
- downloads to node_modules folder in current folder
- if inside another package, i.e. with package.json, automatically adds package to package.json with ^ as version, use --save-dev to add only as dev dependency
- if no package-name is supplied, installs all dependencies in package.json, if --production then only the non dev dependencies
- -g installs package globally, i.e. in prefix/lib/node_modules and bins to prefix/bin
	locally in ./node_modules, and bins to ./node_modules/.bin
- if an package-lock.js exists, it will install all dependencies with specified version

### NPM Dependency Model

- installing a packages creates dependency tree, each package has own dependencies, get installed in it's own node_modules directory, and the dependencies their dependencies etc. pp.
- i.e. two packages can have different versions of same dependency without conflict, No dependency conflicts
- puts first package and all its dependencies on top level, second package and all it's additional dependencies as well, same dependencies are skipped, different version becomes nested, all depends on installation order, can be inefficient if first package depends on one version, and all following packages on another versions because need to nest for each package since top level is already taken
---> multiple copies of dependencies take space
- same fixed dependency install on different times may give different installs, because dependencies themselves likely won't have their dependencies fixed, if those dependencies don't follow semrev guidelines might break the app because version bump introduced incompatibility, could have a bug even if fixed own dependencies, npm install is always russian roulette!!!
---> package-lock.js generated automatically on every change of node_modules, stores exact versions, can be used to reproduce exact install independently if dependencies in the meantime got updated

### Update

npm update

- can update specific package, all packages, local, global
- check with npm outdated if update was successful

### Dedupe

npm dedupe

- flattens dependency tree as much as possible, moves dependencies up the tree, removes duplicates if found
- doesn't solve problem that one dependency version blocks many dependency of another version

### Uninstall

npm uninstall <package-name>

- uninstall package locally or globally
- remove from (dev) dependency list using --save or --save-dev
