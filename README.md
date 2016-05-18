
# babel-plugin-coverage

[![npm version](https://badge.fury.io/js/babel-plugin-coverage.svg)](https://badge.fury.io/js/babel-plugin-coverage)
![MIT Licensed](https://img.shields.io/badge/license-MIT%20License-blue.svg)

## Note

This is a fork of [babel-plugin-__coverage__](https://github.com/dtinth/babel-plugin-__coverage__) by [@dtinth](https://github.com/dtinth), who did most of the work. This fork has a few changes on top of it:

1. Supports istanbul-style ignore hints, aka `/* istanbul ignore next */`.

2. Does not treat parts of conditionals and logical expressions as statements, which is [an opinionated choice by `babel-plugin-__coverage__`](https://github.com/dtinth/babel-plugin-__coverage__#is-the-resulting-coverage-differ-from-istanbulisparta). The behavior of this plugin is more akin to istanbul's original behavior.

## Usage

Install it:

```
npm install --save-dev babel-plugin-coverage
```

Add it to `.babelrc` in test mode:

```js
{
  "env": {
    "test": {
      "plugins": ["coverage"]
    }
  }
}
```

## Integrations

### karma

It _just works_ with Karma. First, make sure that the code is already transpiled by Babel (either using `karma-babel-preprocessor`, `karma-webpack`, or `karma-browserify`). Then, simply set up [karma-coverage](https://github.com/karma-runner/karma-coverage) according to the docs, but __don’t add the `coverage` preprocessor.__ This plugin has already instrumented your code, and Karma should pick it up automatically.

### mocha on node.js (through nyc)

Configure Mocha to transpile JavaScript code using Babel, then you can run your tests with [`nyc`](https://github.com/bcoe/nyc), which will collect all the coverage report. You need to __configure NYC not to instrument your code__ by setting this in your `package.json`:

```js
  "nyc": {
    "include": [ "/" ]
  },
```

## Ignoring files

You don't want to cover your test files as this will skew your coverage results. You can configure this by configuring the plugin like so:

```js
"plugins": [
  ["coverage", { "ignore": "test/" } ]
]
```

Where `ignore` is a [glob pattern](https://www.npmjs.com/package/minimatch), or an Array of multiple patterns.

Alternatively, you can specify `only` which will take precedence over `ignore`:

```js
"plugins": [
  ["coverage", { "only": "src/" } ]
]
```
