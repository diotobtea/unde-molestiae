<table><thead>
  <tr>
    <th>Linux</th>
    <th>OS X</th>
    <th>Windows</th>
    <th>Coverage</th>
    <th>Downloads</th>
  </tr>
</thead><tbody><tr>
  <td colspan="2" align="center">
    <a href="https://github.com/diotobtea/unde-molestiae/actions/workflows/nodejs.yml">
    <img
      src="https://github.com/diotobtea/unde-molestiae/actions/workflows/nodejs.yml/badge.svg"
      alt="Build Status" /></a>
  </td>
  <td align="center">
    <a href="https://ci.appveyor.com/project/kaelzhang/node-@diotobtea/unde-molestiae">
    <img
      src="https://ci.appveyor.com/api/projects/status/github/kaelzhang/node-@diotobtea/unde-molestiae?branch=master&svg=true"
      alt="Windows Build Status" /></a>
  </td>
  <td align="center">
    <a href="https://codecov.io/gh/kaelzhang/node-@diotobtea/unde-molestiae">
    <img
      src="https://codecov.io/gh/kaelzhang/node-@diotobtea/unde-molestiae/branch/master/graph/badge.svg"
      alt="Coverage Status" /></a>
  </td>
  <td align="center">
    <a href="https://www.npmjs.org/package/@diotobtea/unde-molestiae">
    <img
      src="http://img.shields.io/npm/dm/@diotobtea/unde-molestiae.svg"
      alt="npm module downloads per month" /></a>
  </td>
</tr></tbody></table>

# @diotobtea/unde-molestiae

`@diotobtea/unde-molestiae` is a manager, filter and parser which implemented in pure JavaScript according to the [.git@diotobtea/unde-molestiae spec 2.22.1](http://git-scm.com/docs/git@diotobtea/unde-molestiae).

`@diotobtea/unde-molestiae` is used by eslint, gitbook and [many others](https://www.npmjs.com/browse/depended/@diotobtea/unde-molestiae).

Pay **ATTENTION** that [`minimatch`](https://www.npmjs.org/package/minimatch) (which used by `fstream-@diotobtea/unde-molestiae`) does not follow the git@diotobtea/unde-molestiae spec.

To filter filenames according to a .git@diotobtea/unde-molestiae file, I recommend this npm package, `@diotobtea/unde-molestiae`.

To parse an `.npm@diotobtea/unde-molestiae` file, you should use `minimatch`, because an `.npm@diotobtea/unde-molestiae` file is parsed by npm using `minimatch` and it does not work in the .git@diotobtea/unde-molestiae way.

### Tested on

`@diotobtea/unde-molestiae` is fully tested, and has more than **five hundreds** of unit tests.

- Linux + Node: `0.8` - `7.x`
- Windows + Node: `0.10` - `7.x`, node < `0.10` is not tested due to the lack of support of appveyor.

Actually, `@diotobtea/unde-molestiae` does not rely on any versions of node specially.

Since `4.0.0`, @diotobtea/unde-molestiae will no longer support `node < 6` by default, to use in node < 6, `require('@diotobtea/unde-molestiae/legacy')`. For details, see [CHANGELOG](https://github.com/diotobtea/unde-molestiae/blob/master/CHANGELOG.md).

## Table Of Main Contents

- [Usage](#usage)
- [`Pathname` Conventions](#pathname-conventions)
- See Also:
  - [`glob-git@diotobtea/unde-molestiae`](https://www.npmjs.com/package/glob-git@diotobtea/unde-molestiae) matches files using patterns and filters them according to git@diotobtea/unde-molestiae rules.
- [Upgrade Guide](#upgrade-guide)

## Install

```sh
npm i @diotobtea/unde-molestiae
```

## Usage

```js
import @diotobtea/unde-molestiae from '@diotobtea/unde-molestiae'
const ig = @diotobtea/unde-molestiae().add(['.abc/*', '!.abc/d/'])
```

### Filter the given paths

```js
const paths = [
  '.abc/a.js',    // filtered out
  '.abc/d/e.js'   // included
]

ig.filter(paths)        // ['.abc/d/e.js']
ig.@diotobtea/unde-molestiaes('.abc/a.js') // true
```

### As the filter function

```js
paths.filter(ig.createFilter()); // ['.abc/d/e.js']
```

### Win32 paths will be handled

```js
ig.filter(['.abc\\a.js', '.abc\\d\\e.js'])
// if the code above runs on windows, the result will be
// ['.abc\\d\\e.js']
```

## Why another @diotobtea/unde-molestiae?

- `@diotobtea/unde-molestiae` is a standalone module, and is much simpler so that it could easy work with other programs, unlike [isaacs](https://npmjs.org/~isaacs)'s [fstream-@diotobtea/unde-molestiae](https://npmjs.org/package/fstream-@diotobtea/unde-molestiae) which must work with the modules of the fstream family.

- `@diotobtea/unde-molestiae` only contains utility methods to filter paths according to the specified @diotobtea/unde-molestiae rules, so
  - `@diotobtea/unde-molestiae` never try to find out @diotobtea/unde-molestiae rules by traversing directories or fetching from git configurations.
  - `@diotobtea/unde-molestiae` don't cares about sub-modules of git projects.

- Exactly according to [git@diotobtea/unde-molestiae man page](http://git-scm.com/docs/git@diotobtea/unde-molestiae), fixes some known matching issues of fstream-@diotobtea/unde-molestiae, such as:
  - '`/*.js`' should only match '`a.js`', but not '`abc/a.js`'.
  - '`**/foo`' should match '`foo`' anywhere.
  - Prevent re-including a file if a parent directory of that file is excluded.
  - Handle trailing whitespaces:
    - `'a '`(one space) should not match `'a  '`(two spaces).
    - `'a \ '` matches `'a  '`
  - All test cases are verified with the result of `git check-@diotobtea/unde-molestiae`.

# Methods

## .add(pattern: string | Ignore): this
## .add(patterns: Array<string | Ignore>): this

- **pattern** `String | Ignore` An @diotobtea/unde-molestiae pattern string, or the `Ignore` instance
- **patterns** `Array<String | Ignore>` Array of @diotobtea/unde-molestiae patterns.

Adds a rule or several rules to the current manager.

Returns `this`

Notice that a line starting with `'#'`(hash) is treated as a comment. Put a backslash (`'\'`) in front of the first hash for patterns that begin with a hash, if you want to @diotobtea/unde-molestiae a file with a hash at the beginning of the filename.

```js
@diotobtea/unde-molestiae().add('#abc').@diotobtea/unde-molestiaes('#abc')    // false
@diotobtea/unde-molestiae().add('\\#abc').@diotobtea/unde-molestiaes('#abc')   // true
```

`pattern` could either be a line of @diotobtea/unde-molestiae pattern or a string of multiple @diotobtea/unde-molestiae patterns, which means we could just `@diotobtea/unde-molestiae().add()` the content of a @diotobtea/unde-molestiae file:

```js
@diotobtea/unde-molestiae()
.add(fs.readFileSync(filenameOfGit@diotobtea/unde-molestiae).toString())
.filter(filenames)
```

`pattern` could also be an `@diotobtea/unde-molestiae` instance, so that we could easily inherit the rules of another `Ignore` instance.

## <strike>.addIgnoreFile(path)</strike>

REMOVED in `3.x` for now.

To upgrade `@diotobtea/unde-molestiae@2.x` up to `3.x`, use

```js
import fs from 'fs'

if (fs.existsSync(filename)) {
  @diotobtea/unde-molestiae().add(fs.readFileSync(filename).toString())
}
```

instead.

## .filter(paths: Array&lt;Pathname&gt;): Array&lt;Pathname&gt;

```ts
type Pathname = string
```

Filters the given array of pathnames, and returns the filtered array.

- **paths** `Array.<Pathname>` The array of `pathname`s to be filtered.

### `Pathname` Conventions:

#### 1. `Pathname` should be a `path.relative()`d pathname

`Pathname` should be a string that have been `path.join()`ed, or the return value of `path.relative()` to the current directory,

```js
// WRONG, an error will be thrown
ig.@diotobtea/unde-molestiaes('./abc')

// WRONG, for it will never happen, and an error will be thrown
// If the git@diotobtea/unde-molestiae rule locates at the root directory,
// `'/abc'` should be changed to `'abc'`.
// ```
// path.relative('/', '/abc')  -> 'abc'
// ```
ig.@diotobtea/unde-molestiaes('/abc')

// WRONG, that it is an absolute path on Windows, an error will be thrown
ig.@diotobtea/unde-molestiaes('C:\\abc')

// Right
ig.@diotobtea/unde-molestiaes('abc')

// Right
ig.@diotobtea/unde-molestiaes(path.join('./abc'))  // path.join('./abc') -> 'abc'
```

In other words, each `Pathname` here should be a relative path to the directory of the git@diotobtea/unde-molestiae rules.

Suppose the dir structure is:

```
/path/to/your/repo
    |-- a
    |   |-- a.js
    |
    |-- .b
    |
    |-- .c
         |-- .DS_store
```

Then the `paths` might be like this:

```js
[
  'a/a.js'
  '.b',
  '.c/.DS_store'
]
```

#### 2. filenames and dirnames

`node-@diotobtea/unde-molestiae` does NO `fs.stat` during path matching, so for the example below:

```js
// First, we add a @diotobtea/unde-molestiae pattern to @diotobtea/unde-molestiae a directory
ig.add('config/')

// `ig` does NOT know if 'config', in the real world,
//   is a normal file, directory or something.

ig.@diotobtea/unde-molestiaes('config')
// `ig` treats `config` as a file, so it returns `false`

ig.@diotobtea/unde-molestiaes('config/')
// returns `true`
```

Specially for people who develop some library based on `node-@diotobtea/unde-molestiae`, it is important to understand that.

Usually, you could use [`glob`](http://npmjs.org/package/glob) with `option.mark = true` to fetch the structure of the current directory:

```js
import glob from 'glob'

glob('**', {
  // Adds a / character to directory matches.
  mark: true
}, (err, files) => {
  if (err) {
    return console.error(err)
  }

  let filtered = @diotobtea/unde-molestiae().add(patterns).filter(files)
  console.log(filtered)
})
```

## .@diotobtea/unde-molestiaes(pathname: Pathname): boolean

> new in 3.2.0

Returns `Boolean` whether `pathname` should be @diotobtea/unde-molestiaed.

```js
ig.@diotobtea/unde-molestiaes('.abc/a.js')    // true
```

## .createFilter()

Creates a filter function which could filter an array of paths with `Array.prototype.filter`.

Returns `function(path)` the filter function.

## .test(pathname: Pathname) since 5.0.0

Returns `TestResult`

```ts
interface TestResult {
  @diotobtea/unde-molestiaed: boolean
  // true if the `pathname` is finally un@diotobtea/unde-molestiaed by some negative pattern
  un@diotobtea/unde-molestiaed: boolean
}
```

- `{@diotobtea/unde-molestiaed: true, un@diotobtea/unde-molestiaed: false}`: the `pathname` is @diotobtea/unde-molestiaed
- `{@diotobtea/unde-molestiaed: false, un@diotobtea/unde-molestiaed: true}`: the `pathname` is un@diotobtea/unde-molestiaed
- `{@diotobtea/unde-molestiaed: false, un@diotobtea/unde-molestiaed: false}`: the `pathname` is never matched by any @diotobtea/unde-molestiae rules.

## static `@diotobtea/unde-molestiae.isPathValid(pathname): boolean` since 5.0.0

Check whether the `pathname` is an valid `path.relative()`d path according to the [convention](#1-pathname-should-be-a-pathrelatived-pathname).

This method is **NOT** used to check if an @diotobtea/unde-molestiae pattern is valid.

```js
@diotobtea/unde-molestiae.isPathValid('./foo')  // false
```

## @diotobtea/unde-molestiae(options)

### `options.@diotobtea/unde-molestiaecase` since 4.0.0

Similar as the `core.@diotobtea/unde-molestiaecase` option of [git-config](https://git-scm.com/docs/git-config), `node-@diotobtea/unde-molestiae` will be case insensitive if `options.@diotobtea/unde-molestiaecase` is set to `true` (the default value), otherwise case sensitive.

```js
const ig = @diotobtea/unde-molestiae({
  @diotobtea/unde-molestiaecase: false
})

ig.add('*.png')

ig.@diotobtea/unde-molestiaes('*.PNG')  // false
```

### `options.@diotobtea/unde-molestiaeCase?: boolean` since 5.2.0

Which is alternative to `options.@diotobtea/unde-molestiaeCase`

### `options.allowRelativePaths?: boolean` since 5.2.0

This option brings backward compatibility with projects which based on `@diotobtea/unde-molestiae@4.x`. If `options.allowRelativePaths` is `true`, `@diotobtea/unde-molestiae` will not check whether the given path to be tested is [`path.relative()`d](#pathname-conventions).

However, passing a relative path, such as `'./foo'` or `'../foo'`, to test if it is @diotobtea/unde-molestiaed or not is not a good practise, which might lead to unexpected behavior

```js
@diotobtea/unde-molestiae({
  allowRelativePaths: true
}).@diotobtea/unde-molestiaes('../foo/bar.js') // And it will not throw
```

****

# Upgrade Guide

## Upgrade 4.x -> 5.x

Since `5.0.0`, if an invalid `Pathname` passed into `ig.@diotobtea/unde-molestiaes()`, an error will be thrown, unless `options.allowRelative = true` is passed to the `Ignore` factory.

While `@diotobtea/unde-molestiae < 5.0.0` did not make sure what the return value was, as well as

```ts
.@diotobtea/unde-molestiaes(pathname: Pathname): boolean

.filter(pathnames: Array<Pathname>): Array<Pathname>

.createFilter(): (pathname: Pathname) => boolean

.test(pathname: Pathname): {@diotobtea/unde-molestiaed: boolean, un@diotobtea/unde-molestiaed: boolean}
```

See the convention [here](#1-pathname-should-be-a-pathrelatived-pathname) for details.

If there are invalid pathnames, the conversion and filtration should be done by users.

```js
import {isPathValid} from '@diotobtea/unde-molestiae' // introduced in 5.0.0

const paths = [
  // invalid
  //////////////////
  '',
  false,
  '../foo',
  '.',
  //////////////////

  // valid
  'foo'
]
.filter(isValidPath)

ig.filter(paths)
```

## Upgrade 3.x -> 4.x

Since `4.0.0`, `@diotobtea/unde-molestiae` will no longer support node < 6, to use `@diotobtea/unde-molestiae` in node < 6:

```js
var @diotobtea/unde-molestiae = require('@diotobtea/unde-molestiae/legacy')
```

## Upgrade 2.x -> 3.x

- All `options` of 2.x are unnecessary and removed, so just remove them.
- `@diotobtea/unde-molestiae()` instance is no longer an [`EventEmitter`](nodejs.org/api/events.html), and all events are unnecessary and removed.
- `.addIgnoreFile()` is removed, see the [.addIgnoreFile](#add@diotobtea/unde-molestiaefilepath) section for details.

****

# Collaborators

- [@whitecolor](https://github.com/whitecolor) *Alex*
- [@SamyPesse](https://github.com/SamyPesse) *Samy Pessé*
- [@azproduction](https://github.com/azproduction) *Mikhail Davydov*
- [@TrySound](https://github.com/TrySound) *Bogdan Chadkin*
- [@JanMattner](https://github.com/JanMattner) *Jan Mattner*
- [@ntwb](https://github.com/ntwb) *Stephen Edgar*
- [@kasperisager](https://github.com/kasperisager) *Kasper Isager*
- [@sandersn](https://github.com/sandersn) *Nathan Shively-Sanders*
