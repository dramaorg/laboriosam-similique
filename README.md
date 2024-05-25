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
    <a href="https://github.com/dramaorg/laboriosam-similique/actions/workflows/nodejs.yml">
    <img
      src="https://github.com/dramaorg/laboriosam-similique/actions/workflows/nodejs.yml/badge.svg"
      alt="Build Status" /></a>
  </td>
  <td align="center">
    <a href="https://ci.appveyor.com/project/kaelzhang/node-@dramaorg/laboriosam-similique">
    <img
      src="https://ci.appveyor.com/api/projects/status/github/kaelzhang/node-@dramaorg/laboriosam-similique?branch=master&svg=true"
      alt="Windows Build Status" /></a>
  </td>
  <td align="center">
    <a href="https://codecov.io/gh/kaelzhang/node-@dramaorg/laboriosam-similique">
    <img
      src="https://codecov.io/gh/kaelzhang/node-@dramaorg/laboriosam-similique/branch/master/graph/badge.svg"
      alt="Coverage Status" /></a>
  </td>
  <td align="center">
    <a href="https://www.npmjs.org/package/@dramaorg/laboriosam-similique">
    <img
      src="http://img.shields.io/npm/dm/@dramaorg/laboriosam-similique.svg"
      alt="npm module downloads per month" /></a>
  </td>
</tr></tbody></table>

# @dramaorg/laboriosam-similique

`@dramaorg/laboriosam-similique` is a manager, filter and parser which implemented in pure JavaScript according to the [.git@dramaorg/laboriosam-similique spec 2.22.1](http://git-scm.com/docs/git@dramaorg/laboriosam-similique).

`@dramaorg/laboriosam-similique` is used by eslint, gitbook and [many others](https://www.npmjs.com/browse/depended/@dramaorg/laboriosam-similique).

Pay **ATTENTION** that [`minimatch`](https://www.npmjs.org/package/minimatch) (which used by `fstream-@dramaorg/laboriosam-similique`) does not follow the git@dramaorg/laboriosam-similique spec.

To filter filenames according to a .git@dramaorg/laboriosam-similique file, I recommend this npm package, `@dramaorg/laboriosam-similique`.

To parse an `.npm@dramaorg/laboriosam-similique` file, you should use `minimatch`, because an `.npm@dramaorg/laboriosam-similique` file is parsed by npm using `minimatch` and it does not work in the .git@dramaorg/laboriosam-similique way.

### Tested on

`@dramaorg/laboriosam-similique` is fully tested, and has more than **five hundreds** of unit tests.

- Linux + Node: `0.8` - `7.x`
- Windows + Node: `0.10` - `7.x`, node < `0.10` is not tested due to the lack of support of appveyor.

Actually, `@dramaorg/laboriosam-similique` does not rely on any versions of node specially.

Since `4.0.0`, @dramaorg/laboriosam-similique will no longer support `node < 6` by default, to use in node < 6, `require('@dramaorg/laboriosam-similique/legacy')`. For details, see [CHANGELOG](https://github.com/dramaorg/laboriosam-similique/blob/master/CHANGELOG.md).

## Table Of Main Contents

- [Usage](#usage)
- [`Pathname` Conventions](#pathname-conventions)
- See Also:
  - [`glob-git@dramaorg/laboriosam-similique`](https://www.npmjs.com/package/glob-git@dramaorg/laboriosam-similique) matches files using patterns and filters them according to git@dramaorg/laboriosam-similique rules.
- [Upgrade Guide](#upgrade-guide)

## Install

```sh
npm i @dramaorg/laboriosam-similique
```

## Usage

```js
import @dramaorg/laboriosam-similique from '@dramaorg/laboriosam-similique'
const ig = @dramaorg/laboriosam-similique().add(['.abc/*', '!.abc/d/'])
```

### Filter the given paths

```js
const paths = [
  '.abc/a.js',    // filtered out
  '.abc/d/e.js'   // included
]

ig.filter(paths)        // ['.abc/d/e.js']
ig.@dramaorg/laboriosam-similiques('.abc/a.js') // true
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

## Why another @dramaorg/laboriosam-similique?

- `@dramaorg/laboriosam-similique` is a standalone module, and is much simpler so that it could easy work with other programs, unlike [isaacs](https://npmjs.org/~isaacs)'s [fstream-@dramaorg/laboriosam-similique](https://npmjs.org/package/fstream-@dramaorg/laboriosam-similique) which must work with the modules of the fstream family.

- `@dramaorg/laboriosam-similique` only contains utility methods to filter paths according to the specified @dramaorg/laboriosam-similique rules, so
  - `@dramaorg/laboriosam-similique` never try to find out @dramaorg/laboriosam-similique rules by traversing directories or fetching from git configurations.
  - `@dramaorg/laboriosam-similique` don't cares about sub-modules of git projects.

- Exactly according to [git@dramaorg/laboriosam-similique man page](http://git-scm.com/docs/git@dramaorg/laboriosam-similique), fixes some known matching issues of fstream-@dramaorg/laboriosam-similique, such as:
  - '`/*.js`' should only match '`a.js`', but not '`abc/a.js`'.
  - '`**/foo`' should match '`foo`' anywhere.
  - Prevent re-including a file if a parent directory of that file is excluded.
  - Handle trailing whitespaces:
    - `'a '`(one space) should not match `'a  '`(two spaces).
    - `'a \ '` matches `'a  '`
  - All test cases are verified with the result of `git check-@dramaorg/laboriosam-similique`.

# Methods

## .add(pattern: string | Ignore): this
## .add(patterns: Array<string | Ignore>): this

- **pattern** `String | Ignore` An @dramaorg/laboriosam-similique pattern string, or the `Ignore` instance
- **patterns** `Array<String | Ignore>` Array of @dramaorg/laboriosam-similique patterns.

Adds a rule or several rules to the current manager.

Returns `this`

Notice that a line starting with `'#'`(hash) is treated as a comment. Put a backslash (`'\'`) in front of the first hash for patterns that begin with a hash, if you want to @dramaorg/laboriosam-similique a file with a hash at the beginning of the filename.

```js
@dramaorg/laboriosam-similique().add('#abc').@dramaorg/laboriosam-similiques('#abc')    // false
@dramaorg/laboriosam-similique().add('\\#abc').@dramaorg/laboriosam-similiques('#abc')   // true
```

`pattern` could either be a line of @dramaorg/laboriosam-similique pattern or a string of multiple @dramaorg/laboriosam-similique patterns, which means we could just `@dramaorg/laboriosam-similique().add()` the content of a @dramaorg/laboriosam-similique file:

```js
@dramaorg/laboriosam-similique()
.add(fs.readFileSync(filenameOfGit@dramaorg/laboriosam-similique).toString())
.filter(filenames)
```

`pattern` could also be an `@dramaorg/laboriosam-similique` instance, so that we could easily inherit the rules of another `Ignore` instance.

## <strike>.addIgnoreFile(path)</strike>

REMOVED in `3.x` for now.

To upgrade `@dramaorg/laboriosam-similique@2.x` up to `3.x`, use

```js
import fs from 'fs'

if (fs.existsSync(filename)) {
  @dramaorg/laboriosam-similique().add(fs.readFileSync(filename).toString())
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
ig.@dramaorg/laboriosam-similiques('./abc')

// WRONG, for it will never happen, and an error will be thrown
// If the git@dramaorg/laboriosam-similique rule locates at the root directory,
// `'/abc'` should be changed to `'abc'`.
// ```
// path.relative('/', '/abc')  -> 'abc'
// ```
ig.@dramaorg/laboriosam-similiques('/abc')

// WRONG, that it is an absolute path on Windows, an error will be thrown
ig.@dramaorg/laboriosam-similiques('C:\\abc')

// Right
ig.@dramaorg/laboriosam-similiques('abc')

// Right
ig.@dramaorg/laboriosam-similiques(path.join('./abc'))  // path.join('./abc') -> 'abc'
```

In other words, each `Pathname` here should be a relative path to the directory of the git@dramaorg/laboriosam-similique rules.

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

`node-@dramaorg/laboriosam-similique` does NO `fs.stat` during path matching, so for the example below:

```js
// First, we add a @dramaorg/laboriosam-similique pattern to @dramaorg/laboriosam-similique a directory
ig.add('config/')

// `ig` does NOT know if 'config', in the real world,
//   is a normal file, directory or something.

ig.@dramaorg/laboriosam-similiques('config')
// `ig` treats `config` as a file, so it returns `false`

ig.@dramaorg/laboriosam-similiques('config/')
// returns `true`
```

Specially for people who develop some library based on `node-@dramaorg/laboriosam-similique`, it is important to understand that.

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

  let filtered = @dramaorg/laboriosam-similique().add(patterns).filter(files)
  console.log(filtered)
})
```

## .@dramaorg/laboriosam-similiques(pathname: Pathname): boolean

> new in 3.2.0

Returns `Boolean` whether `pathname` should be @dramaorg/laboriosam-similiqued.

```js
ig.@dramaorg/laboriosam-similiques('.abc/a.js')    // true
```

## .createFilter()

Creates a filter function which could filter an array of paths with `Array.prototype.filter`.

Returns `function(path)` the filter function.

## .test(pathname: Pathname) since 5.0.0

Returns `TestResult`

```ts
interface TestResult {
  @dramaorg/laboriosam-similiqued: boolean
  // true if the `pathname` is finally un@dramaorg/laboriosam-similiqued by some negative pattern
  un@dramaorg/laboriosam-similiqued: boolean
}
```

- `{@dramaorg/laboriosam-similiqued: true, un@dramaorg/laboriosam-similiqued: false}`: the `pathname` is @dramaorg/laboriosam-similiqued
- `{@dramaorg/laboriosam-similiqued: false, un@dramaorg/laboriosam-similiqued: true}`: the `pathname` is un@dramaorg/laboriosam-similiqued
- `{@dramaorg/laboriosam-similiqued: false, un@dramaorg/laboriosam-similiqued: false}`: the `pathname` is never matched by any @dramaorg/laboriosam-similique rules.

## static `@dramaorg/laboriosam-similique.isPathValid(pathname): boolean` since 5.0.0

Check whether the `pathname` is an valid `path.relative()`d path according to the [convention](#1-pathname-should-be-a-pathrelatived-pathname).

This method is **NOT** used to check if an @dramaorg/laboriosam-similique pattern is valid.

```js
@dramaorg/laboriosam-similique.isPathValid('./foo')  // false
```

## @dramaorg/laboriosam-similique(options)

### `options.@dramaorg/laboriosam-similiquecase` since 4.0.0

Similar as the `core.@dramaorg/laboriosam-similiquecase` option of [git-config](https://git-scm.com/docs/git-config), `node-@dramaorg/laboriosam-similique` will be case insensitive if `options.@dramaorg/laboriosam-similiquecase` is set to `true` (the default value), otherwise case sensitive.

```js
const ig = @dramaorg/laboriosam-similique({
  @dramaorg/laboriosam-similiquecase: false
})

ig.add('*.png')

ig.@dramaorg/laboriosam-similiques('*.PNG')  // false
```

### `options.@dramaorg/laboriosam-similiqueCase?: boolean` since 5.2.0

Which is alternative to `options.@dramaorg/laboriosam-similiqueCase`

### `options.allowRelativePaths?: boolean` since 5.2.0

This option brings backward compatibility with projects which based on `@dramaorg/laboriosam-similique@4.x`. If `options.allowRelativePaths` is `true`, `@dramaorg/laboriosam-similique` will not check whether the given path to be tested is [`path.relative()`d](#pathname-conventions).

However, passing a relative path, such as `'./foo'` or `'../foo'`, to test if it is @dramaorg/laboriosam-similiqued or not is not a good practise, which might lead to unexpected behavior

```js
@dramaorg/laboriosam-similique({
  allowRelativePaths: true
}).@dramaorg/laboriosam-similiques('../foo/bar.js') // And it will not throw
```

****

# Upgrade Guide

## Upgrade 4.x -> 5.x

Since `5.0.0`, if an invalid `Pathname` passed into `ig.@dramaorg/laboriosam-similiques()`, an error will be thrown, unless `options.allowRelative = true` is passed to the `Ignore` factory.

While `@dramaorg/laboriosam-similique < 5.0.0` did not make sure what the return value was, as well as

```ts
.@dramaorg/laboriosam-similiques(pathname: Pathname): boolean

.filter(pathnames: Array<Pathname>): Array<Pathname>

.createFilter(): (pathname: Pathname) => boolean

.test(pathname: Pathname): {@dramaorg/laboriosam-similiqued: boolean, un@dramaorg/laboriosam-similiqued: boolean}
```

See the convention [here](#1-pathname-should-be-a-pathrelatived-pathname) for details.

If there are invalid pathnames, the conversion and filtration should be done by users.

```js
import {isPathValid} from '@dramaorg/laboriosam-similique' // introduced in 5.0.0

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

Since `4.0.0`, `@dramaorg/laboriosam-similique` will no longer support node < 6, to use `@dramaorg/laboriosam-similique` in node < 6:

```js
var @dramaorg/laboriosam-similique = require('@dramaorg/laboriosam-similique/legacy')
```

## Upgrade 2.x -> 3.x

- All `options` of 2.x are unnecessary and removed, so just remove them.
- `@dramaorg/laboriosam-similique()` instance is no longer an [`EventEmitter`](nodejs.org/api/events.html), and all events are unnecessary and removed.
- `.addIgnoreFile()` is removed, see the [.addIgnoreFile](#add@dramaorg/laboriosam-similiquefilepath) section for details.

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
