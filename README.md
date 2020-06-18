
**Fork Info**: ensures spaces before and after `*` and `/` operators, which normally wouldn't happen


# PostCSS Calc [<img src="https://postcss.github.io/postcss/logo.svg" alt="PostCSS" width="90" height="90" align="right">][PostCSS]

[![NPM Version][npm-img]][npm-url]
[![Build Status][cli-img]][cli-url]
[![Support Chat][git-img]][git-url]

[PostCSS Calc] lets you reduce `calc()` references whenever it's possible. This
can be particularly useful with the [PostCSS Custom Properties] plugin.

When multiple units are mixed together in the same expression, the `calc()`
statement is left as is, to fallback to the [W3C calc() implementation].

## Installation

```bash
npm install postcss-calc
```

## Usage

```js
// dependencies
var fs = require("fs")
var postcss = require("postcss")
var calc = require("postcss-calc")

// css to be processed
var css = fs.readFileSync("input.css", "utf8")

// process css
var output = postcss()
  .use(calc())
  .process(css)
  .css
```

**Example** (with [PostCSS Custom Properties] enabled as well):

```js
// dependencies
var fs = require("fs")
var postcss = require("postcss")
var customProperties = require("postcss-custom-properties")
var calc = require("postcss-calc")

// css to be processed
var css = fs.readFileSync("input.css", "utf8")

// process css
var output = postcss()
  .use(customProperties())
  .use(calc())
  .process(css)
  .css
```

Using this `input.css`:

```css
:root {
  --main-font-size: 16px;
}

body {
  font-size: var(--main-font-size);
}

h1 {
  font-size: calc(var(--main-font-size) * 2);
  height: calc(100px - 2em);
  margin-bottom: calc(
      var(--main-font-size)
      * 1.5
    )
}
```

you will get:

```css
body {
  font-size: 16px
}

h1 {
  font-size: 32px;
  height: calc(100px - 2em);
  margin-bottom: 24px
}
```

Checkout [tests] for more examples.

### Options

#### `precision` (default: `5`)

Allow you to define the precision for decimal numbers.

```js
var out = postcss()
  .use(calc({precision: 10}))
  .process(css)
  .css
```

#### `preserve` (default: `false`)

Allow you to preserve calc() usage in output so browsers will handle decimal
precision themselves.

```js
var out = postcss()
  .use(calc({preserve: true}))
  .process(css)
  .css
```

#### `warnWhenCannotResolve` (default: `false`)

Adds warnings when calc() are not reduced to a single value.

```js
var out = postcss()
  .use(calc({warnWhenCannotResolve: true}))
  .process(css)
  .css
```

#### `mediaQueries` (default: `false`)

Allows calc() usage as part of media query declarations.

```js
var out = postcss()
  .use(calc({mediaQueries: true}))
  .process(css)
  .css
```

#### `selectors` (default: `false`)

Allows calc() usage as part of selectors.

```js
var out = postcss()
  .use(calc({selectors: true}))
  .process(css)
  .css
```

Example:

```css
div[data-size="calc(3*3)"] {
  width: 100px;
}
```

---

## Contributing

Work on a branch, install dev-dependencies, respect coding style & run tests
before submitting a bug fix or a feature.

```bash
git clone git@github.com:postcss/postcss-calc.git
git checkout -b patch-1
npm install
npm test
```

## [Changelog](CHANGELOG.md)

## [License](LICENSE)

[cli-img]: https://img.shields.io/travis/postcss/postcss-calc/master.svg
[cli-url]: https://travis-ci.org/postcss/postcss-calc
[git-img]: https://img.shields.io/badge/support-chat-blue.svg
[git-url]: https://gitter.im/postcss/postcss
[npm-img]: https://img.shields.io/npm/v/postcss-calc.svg
[npm-url]: https://www.npmjs.com/package/postcss-calc

[PostCSS]: https://github.com/postcss
[PostCSS Calc]: https://github.com/postcss/postcss-calc
[PostCSS Custom Properties]: https://github.com/postcss/postcss-custom-properties
[tests]: src/__tests__/index.js
[W3C calc() implementation]: https://www.w3.org/TR/css3-values/#calc-notation
