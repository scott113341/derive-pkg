# derive-pkg

[![build status][build-badge]][build-href]
[![coverage status][coverage-badge]][coverage-href]
[![dependencies status][deps-badge]][deps-href]
[![npm version][npm-badge]][npm-href]

A helper utility for publishing transpiled code to npm

###### Turn `require('your-module/lib/submodule')` into `require('your-module/submodule')`

#### What is this for?

The standard convention when publishing code transpiled with [Babel](https://github.com/babel/babel) is to place source code into `src/` and transpiled code into `lib/`. Unfortunately, this makes consuming individual submodules inconvenient because they don't exist at the root of the package, e.g. one must `require('the-module/lib/some-submodule.js')`.

This utility derives npm package metadata from your root directory and copies it to your build directory so you can publish it instead. Now `lib/` is the root of your package!

`derive-pkg` does the following:

- Copies `package.json` from root with the changes below
  - Rebase `main`, `bin`, and `browser` field entry paths from `lib/` to `/`
  - Omit `devDependencies`
- Copies files from root that npm will never ignore and should be included when publishing (e.g. readme, license, and changelog)
- Copies `.gitignore`/`.npmignore` from root as `.npmignore`

## Install

```
npm install derive-pkg --save-dev
```

## Quick Example

```
babel-cli src -d lib
derive-pkg -d lib
npm publish lib
```

## Usage

```
Usage: derive-pkg [directory] [options]

Options:
      directory   The base directory containing the package.json (default: ".")

  --out-dir, -d   The output directory for the derived package.json

     --name, -n   Override the name field of the derived package.json

  --version, -v   Override the version field of the derived package.json
```

## FAQ

**Couldn't this just be a shell script?**

Yep, but `derive-pkg` is cross-platform and has unit tests.

[npm-badge]: https://badge.fury.io/js/derive-pkg.svg
[npm-href]: https://www.npmjs.com/package/derive-pkg
[build-badge]: https://travis-ci.org/rtsao/derive-pkg.svg?branch=master
[build-href]: https://travis-ci.org/rtsao/derive-pkg
[coverage-badge]: https://coveralls.io/repos/rtsao/derive-pkg/badge.svg?branch=master&service=github
[coverage-href]: https://coveralls.io/github/rtsao/derive-pkg?branch=master
[deps-badge]: https://david-dm.org/rtsao/derive-pkg.svg
[deps-href]: https://david-dm.org/rtsao/derive-pkg
