# dush-options [![npm version][npmv-img]][npmv-url] [![github tags][ghtag-img]][ghtag-url] [![mit license][license-img]][license-url]

> Adds `.option`, `.enable` and `.disable` methods to your `dush` application

You might also be interested in [dush](https://github.com/tunnckocore/dush#readme).

## Quality 👌

> By using [commitizen][czfriendly-url] and [conventional commit messages][conventional-messages-url], 
maintaining meaningful [ChangeLog][changelogmd-url] 
and commit history based on [global conventions][conventions-url], 
following [StandardJS][standard-url] code style through [ESLint][eslint-url] and
having always up-to-date dependencies through integrations
like [GreenKeeper][gk-integration-url] and [David-DM][daviddm-url] service,
this package has top quality.

[![code climate][codeclimate-img]][codeclimate-url] 
[![code style][standard-img]][standard-url] 
[![commitizen friendly][czfriendly-img]][czfriendly-url] 
[![greenkeeper friendly][gkfriendly-img]][gkfriendly-url] 
[![dependencies][daviddm-deps-img]][daviddm-deps-url] 
<!-- uncomment when need -->
<!-- [![develop deps][daviddm-devdeps-img]][daviddm-devdeps-url] -->

## Stability 💯

> By following [Semantic Versioning][semver-url] through [standard-version][] releasing tool, 
this package is very stable and its tests are passing both on [Windows (AppVeyor)][appveyor-ci-url] 
and [Linux (CircleCI)][circle-ci-url] with results 
from 100% to [400%][absolute-coverage-url] test coverage, reported respectively
by [CodeCov][codecov-coverage-url] and [nyc (istanbul)][nyc-istanbul-url].

[![following semver][following-semver-img]][following-semver-url] 
[![semantic releases][strelease-img]][strelease-url] 
[![linux build][circle-img]][circle-url] 
[![windows build][appveyor-img]][appveyor-url] 
[![code coverage][codecov-img]][codecov-url] 
[![nyc coverage][istanbulcov-img]][istanbulcov-url] 

## Support :clap:

> If you have any problems, consider opening [an issue][open-issue-url],
ping me on twitter ([@tunnckoCore][tunnckocore-twitter-url]),
join the [support chat][supportchat-url] room
or queue a [live session][codementor-url] on CodeMentor with me.
If you don't have any problems, you're using it somewhere or
you just enjoy this product, then please consider [donating some cash][paypalme-url] at PayPal,
since this is [OPEN Open Source][opensource-project-url] project made
with love at [Sofia, Bulgaria][bulgaria-url] 🇧🇬.

[![tunnckoCore support][supportchat-img]][supportchat-url] 
[![code mentor][codementor-img]][codementor-url] 
[![paypal donate][paypalme-img]][paypalme-url] 
[![NPM monthly downloads](https://img.shields.io/npm/dm/dush-options.svg?style=flat)](https://npmjs.org/package/dush-options) 
[![npm total downloads][downloads-img]][downloads-url] 

## Table of Contents
- [Install](#install)
- [Usage](#usage)
- [API](#api)
  * [dushOptions](#dushoptions)
  * [.option](#option)
  * [.enable](#enable)
  * [.disable](#disable)
- [Related](#related)
- [Contributing](#contributing)
- [Building docs](#building-docs)
- [Running tests](#running-tests)
- [Author](#author)
- [License](#license)

_(TOC generated by [verb](https://github.com/verbose/verb) using [markdown-toc](https://github.com/jonschlinkert/markdown-toc))_

## Install
Install with [npm](https://www.npmjs.com/)

```
$ npm install dush-options --save
```

or install using [yarn](https://yarnpkg.com)

```
$ yarn add dush-options
```

## Usage
> For more use-cases see the [tests](test.js)

```js
const dushOptions = require('dush-options')
```

## API

### [dushOptions](index.js#L46)
> A plugin for [dush][]/[minibase][]/[base][] that adds `.option`, `.enable` and `.disable` methods to your app. You can pass `options` to be merged with `app.options`

**Params**

* `options` **{Object}**: optional, initial options to set to `app.options` property    
* `returns` **{Function}**: a plugin function, pass it to `.use` method of [dush][]/[minibase][]/[base][]  

**Example**

```js
var dush = require('dush')
var options = require('dush-options')

var app = dush()

// some initial options
var opts = { foo: 'bar' }

app.use(options(opts))

console.log(app.options) // => { foo: 'bar' }

console.log(app.option()) // => { foo: 'bar' }
console.log(app.option) // => function

console.log(app.enable) // => function
console.log(app.disable) // => function
```

### [.option](index.js#L100)
> Set or get an option(s). Support dot notation syntax too. If there are no arguments it returns `app.options`. If `key` is string and no `value` argument, it gets that property from the `app.options` object - using [get-value][], so `app.option('foo.bar.qux')`. If `key` is object it is merged with `app.options` using [mixin-deep][]. If both `key` and `value` is given then it sets `value` to `key` property, using [set-value][] library.

**Params**

* `key` **{String|Object}**: path to some option property, e.g. `a.b.c`    
* `value` **{any}**: if `key` is string, any value to set to `key` property    
* `returns` **{Object}**: _clone_ of the modified `app.options` object, or some `key` value  

**Example**

```js
var app = dush()
app.use(options({ initial: 'props' }))

console.log(app.options) // => { initial: 'props' }
console.log(app.option()) // => { initial: 'props' }

app.option({ foo: 'bar' })
console.log(app.options)
// => { initial: 'props', foo: 'bar' }

app.option('qux', 123)
console.log(app.options)
// => { initial: 'props', foo: 'bar', qux: 123 }

app.option('aa.bb.cc', 'dd')
console.log(app.options)
// => {
//  initial: 'props',
//  foo: 'bar',
//  qux: 123,
//  aa: { bb: { cc: 'dd' } }
// }

console.log(app.option('aa.bb')) // => { cc: 'dd' }
console.log(app.option('aa')) // => { bb: { cc: 'dd' }
console.log(app.option('foo')) // => 'bar'
```

### [.enable](index.js#L139)
> Enables a `key` to have `true` value. It is simply just a shortcut for `app.option('foo', true)`.

**Params**

* `key` **{String}**: a path to property to enable    
* `returns` **{Object}**: always self for chaining  

**Example**

```js
app.use(options())
console.log(app.options) // => {}

app.enable('foo')
console.log(app.options) // => { foo: true }

app.enable('qux.baz')
console.log(app.options) // => { foo: true, qux: { baz: true } }
```

### [.disable](index.js#L173)
> Disable a `key` to have `false` value. It is simply just a shortcut for `app.option('zzz', false)`.

**Params**

* `key` **{String}**: a path to property to disable    
* `returns` **{Object}**: always self for chaining  

**Example**

```js
app.use(options())
console.log(app.options) // => {}

app.enable('foo')
console.log(app.options) // => { foo: true }

app.disable('foo')
console.log(app.options) // => { foo: false }

app.enable('qux.baz')
console.log(app.options.qux) // => { baz: true }

app.disable('qux.baz')
console.log(app.options.qux) // => { baz: false }
```

## Related
- [base-options](https://www.npmjs.com/package/base-options): Adds a few options methods to base-methods, like `option`, `enable` and `disable`. See the readme for the full API. | [homepage](https://github.com/jonschlinkert/base-options "Adds a few options methods to base-methods, like `option`, `enable` and `disable`. See the readme for the full API.")
- [base-plugins](https://www.npmjs.com/package/base-plugins): Upgrade's plugin support in base applications to allow plugins to be called any time after init. | [homepage](https://github.com/node-base/base-plugins "Upgrade's plugin support in base applications to allow plugins to be called any time after init.")
- [base](https://www.npmjs.com/package/base): Framework for rapidly creating high quality node.js applications, using plugins like building blocks | [homepage](https://github.com/node-base/base "Framework for rapidly creating high quality node.js applications, using plugins like building blocks")
- [dush-no-chaining](https://www.npmjs.com/package/dush-no-chaining): A plugin that removes the emitter methods chaining support for `dush`, `base`, `minibase` or anything based on them | [homepage](https://github.com/tunnckocore/dush-no-chaining#readme "A plugin that removes the emitter methods chaining support for `dush`, `base`, `minibase` or anything based on them")
- [dush-promise](https://www.npmjs.com/package/dush-promise): Plugin for `dush` that makes it a Deferred promise and adds `.resolve`, `.reject`, `.than` and `.catch` methods for more better… [more](https://github.com/tunnckocore/dush-promise#readme) | [homepage](https://github.com/tunnckocore/dush-promise#readme "Plugin for `dush` that makes it a Deferred promise and adds `.resolve`, `.reject`, `.than` and `.catch` methods for more better error handling experience")
- [dush-router](https://www.npmjs.com/package/dush-router): A simple regex-based router for `dush`, `base`, `minibase` and anything based on them. Works on Browser and Node.js | [homepage](https://github.com/tunnckocore/dush-router#readme "A simple regex-based router for `dush`, `base`, `minibase` and anything based on them. Works on Browser and Node.js")
- [dush-tap-report](https://www.npmjs.com/package/dush-tap-report): A simple TAP report producer based on event system. A plugin for `dush` event emitter or anything based on it | [homepage](https://github.com/tunnckocore/dush-tap-report#readme "A simple TAP report producer based on event system. A plugin for `dush` event emitter or anything based on it")
- [dush](https://www.npmjs.com/package/dush): Microscopic & functional event emitter in ~260 bytes, extensible through plugins. | [homepage](https://github.com/tunnckocore/dush#readme "Microscopic & functional event emitter in ~260 bytes, extensible through plugins.")
- [minibase-create-plugin](https://www.npmjs.com/package/minibase-create-plugin): Utility for [minibase][] and [base][] that helps you create plugins | [homepage](https://github.com/node-minibase/minibase-create-plugin#readme "Utility for [minibase][] and [base][] that helps you create plugins")
- [minibase-is-registered](https://www.npmjs.com/package/minibase-is-registered): Plugin for [minibase][] and [base][], that adds `isRegistered` method to your application to detect if plugin is already registered and… [more](https://github.com/node-minibase/minibase-is-registered#readme) | [homepage](https://github.com/node-minibase/minibase-is-registered#readme "Plugin for [minibase][] and [base][], that adds `isRegistered` method to your application to detect if plugin is already registered and returns true or false if named plugin is already registered on the instance.")
- [minibase](https://www.npmjs.com/package/minibase): Minimalist alternative for Base. Build complex APIs with small units called plugins. Works well with most of the already existing… [more](https://github.com/node-minibase/minibase#readme) | [homepage](https://github.com/node-minibase/minibase#readme "Minimalist alternative for Base. Build complex APIs with small units called plugins. Works well with most of the already existing [base][] plugins.")

## Contributing
Pull requests and stars are always welcome. For bugs and feature requests, [please create an issue][open-issue-url].  
Please read the [contributing guidelines][contributing-url] for advice on opening issues, pull requests, and coding standards.  
If you need some help and can spent some cash, feel free to [contact me at CodeMentor.io][codementor-url] too.

**In short:** If you want to contribute to that project, please follow these things

1. Please DO NOT edit [README.md](README.md), [CHANGELOG.md][changelogmd-url] and [.verb.md](.verb.md) files. See ["Building docs"](#building-docs) section.
2. Ensure anything is okey by installing the dependencies and run the tests. See ["Running tests"](#running-tests) section.
3. Always use `npm run commit` to commit changes instead of `git commit`, because it is interactive and user-friendly. It uses [commitizen][] behind the scenes, which follows Conventional Changelog idealogy.
4. Do NOT bump the version in package.json. For that we use `npm run release`, which is [standard-version][] and follows Conventional Changelog idealogy.

Thanks a lot! :)

## Building docs
Documentation and that readme is generated using [verb-generate-readme][], which is a [verb][] generator, so you need to install both of them and then run `verb` command like that

```
$ npm install verbose/verb#dev verb-generate-readme --global && verb
```

_Please don't edit the README directly. Any changes to the readme must be made in [.verb.md](.verb.md)._

## Running tests
Clone repository and run the following in that cloned directory

```
$ npm install && npm test
```

## Author
**Charlike Mike Reagent**

+ [github/tunnckoCore](https://github.com/tunnckoCore)
+ [twitter/tunnckoCore](https://twitter.com/tunnckoCore)
+ [codementor/tunnckoCore](https://codementor.io/tunnckoCore)

## License
Copyright © 2017, [Charlike Mike Reagent](https://i.am.charlike.online). Released under the [MIT License](LICENSE).

***

_This file was generated by [verb-generate-readme](https://github.com/verbose/verb-generate-readme), v0.4.3, on April 02, 2017._  
_Project scaffolded using [charlike][] cli._

[base]: https://github.com/node-base/base
[charlike]: https://github.com/tunnckocore/charlike
[commitizen]: https://github.com/commitizen/cz-cli
[dush]: https://github.com/tunnckocore/dush
[get-value]: https://github.com/jonschlinkert/get-value
[merge-deep]: https://github.com/jonschlinkert/merge-deep
[minibase]: https://github.com/node-minibase/minibase
[set-value]: https://github.com/jonschlinkert/set-value
[standard-version]: https://github.com/conventional-changelog/standard-version
[verb-generate-readme]: https://github.com/verbose/verb-generate-readme
[verb]: https://github.com/verbose/verb

[license-url]: https://github.com/tunnckoCore/dush-options/blob/master/LICENSE
[license-img]: https://img.shields.io/npm/l/dush-options.svg

[downloads-url]: https://www.npmjs.com/package/dush-options
[downloads-img]: https://img.shields.io/npm/dt/dush-options.svg

[codeclimate-url]: https://codeclimate.com/github/tunnckoCore/dush-options
[codeclimate-img]: https://img.shields.io/codeclimate/github/tunnckoCore/dush-options.svg

[circle-url]: https://circleci.com/gh/tunnckoCore/dush-options
[circle-img]: https://img.shields.io/circleci/project/github/tunnckoCore/dush-options/master.svg?label=linux

[appveyor-url]: https://ci.appveyor.com/project/tunnckoCore/dush-options
[appveyor-img]: https://img.shields.io/appveyor/ci/tunnckoCore/dush-options/master.svg?label=windows

[codecov-url]: https://codecov.io/gh/tunnckoCore/dush-options
[codecov-img]: https://img.shields.io/codecov/c/github/tunnckoCore/dush-options/master.svg?label=codecov

[daviddm-deps-url]: https://david-dm.org/tunnckoCore/dush-options
[daviddm-deps-img]: https://img.shields.io/david/tunnckoCore/dush-options.svg

[daviddm-devdeps-url]: https://david-dm.org/tunnckoCore/dush-options?type=dev
[daviddm-devdeps-img]: https://img.shields.io/david/dev/tunnckoCore/dush-options.svg

[ghtag-url]: https://github.com/tunnckoCore/dush-options/tags
[ghtag-img]: https://img.shields.io/github/tag/tunnckoCore/dush-options.svg?label=github%20tag

[npmv-url]: https://www.npmjs.com/package/dush-options
[npmv-img]: https://img.shields.io/npm/v/dush-options.svg?label=npm%20version

[standard-url]: https://github.com/feross/standard
[standard-img]: https://img.shields.io/badge/code%20style-standard-brightgreen.svg

[paypalme-url]: https://www.paypal.me/tunnckoCore
[paypalme-img]: https://img.shields.io/badge/paypal-donate-brightgreen.svg

[czfriendly-url]: http://commitizen.github.io/cz-cli
[czfriendly-img]: https://img.shields.io/badge/commitizen-friendly-brightgreen.svg

[gkfriendly-url]: https://greenkeeper.io/
[gkfriendly-img]: https://img.shields.io/badge/greenkeeper-friendly-brightgreen.svg

[codementor-url]: https://www.codementor.io/tunnckocore?utm_source=github&utm_medium=button&utm_term=tunnckocore&utm_campaign=github
[codementor-img]: https://img.shields.io/badge/code%20mentor-live%20session-brightgreen.svg

[istanbulcov-url]: https://twitter.com/tunnckoCore/status/841768516965568512
[istanbulcov-img]: https://img.shields.io/badge/istanbul-400%25-brightgreen.svg

[following-semver-url]: http://semver.org
[following-semver-img]: https://img.shields.io/badge/following-semver-brightgreen.svg

[strelease-url]: https://github.com/conventional-changelog/standard-version
[strelease-img]: https://img.shields.io/badge/using-standard%20version-brightgreen.svg

[supportchat-url]: https://gitter.im/tunnckoCore/support
[supportchat-img]: https://img.shields.io/gitter/room/tunnckoCore/support.svg

[bulgaria-url]: https://www.google.bg/search?q=Sofia%2C+Bulgaria "One of the top 10 best places for start-up business in the world, especially in IT technologies"

[changelogmd-url]: https://github.com/tunnckoCore/dush-options/blob/master/CHANGELOG.md
[conventions-url]: https://github.com/bcoe/conventional-changelog-standard/blob/master/convention.md
[tunnckocore-twitter-url]: https://twitter.com/tunnckoCore
[opensource-project-url]: http://openopensource.org
[nyc-istanbul-url]: https://istanbul.js.org
[circle-ci-url]: https://circleci.com
[appveyor-ci-url]: https://appveyor.com
[codecov-coverage-url]: https://codecov.io
[semver-url]: http://semver.org
[eslint-url]: http://eslint.org
[conventional-messages-url]: https://github.com/conventional-changelog/conventional-changelog
[gk-integration-url]: https://github.com/integration/greenkeeper
[daviddm-url]: https://david-dm.org
[open-issue-url]: https://github.com/tunnckoCore/dush-options/issues/new
[contributing-url]: https://github.com/tunnckoCore/dush-options/blob/master/CONTRIBUTING.md
[absolute-coverage-url]: https://github.com/tunnckoCore/dush-options/blob/master/package.json

[mixin-deep]: https://github.com/jonschlinkert/mixin-deep