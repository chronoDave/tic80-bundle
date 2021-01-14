![tic-bundle logo](https://i.imgur.com/YpexCm4.png)

[![license GPLv3](https://img.shields.io/badge/license-GPLv3-blue.svg)](https://www.gnu.org/licenses/gpl-3.0)
[![npm](https://img.shields.io/npm/v/tic-bundle?label=npm)](https://www.npmjs.com/package/tic-bundle)
![Test](https://github.com/chronoDave/tic-bundle/workflows/Test/badge.svg?branch=master)

# tic-bundle

Simple CLI tool for bundling [TIC-80](https://tic.computer/) cartridge code. Currently supporting:
 - [JavaScript (ES5)](https://www.w3schools.com/js/js_es5.asp)
 - [Moonscript](https://github.com/leafo/moonscript)
 - [Fennel](https://fennel-lang.org/)
 - [Wren](https://wren.io/)

## Content

 - [Installation](#installation)
 - [Example](#example)
 - [CLI](#cli)
 - [Configuration](#configuration)
   - [Options](#options)
   - [Babel](#babel)
 - [License](./LICENSE)
 - [Donating](#donating)


## Installation

```
// Yarn
yarn add tic-bundle --dev

// Npm
npm i tic-bundle --save-dev
```

## Example

<b>Config</b>

```JS
{
  files: ['ui.js', 'main.js']
}
```

<b>Input</b>

`src/main.js`

```
function TIC() {

};
```

`src/ui.js`

```
function ui() {
  return 'ui';
};
```

<b>Output</b>

`build.js`

```
// script: js

function ui() {
  return 'ui';
};

function TIC() {

};
```

## CLI

`package.json`

```JSON
{
  "scripts": {
    "watch": "tic-bundle"
  }
}
```

<b>CLI options</b>

 - `-r / --root` - Root folder
 - `-c / --config` - Path to config file
 - `-o / --output` - Bundled file output path
 - `-n / --name` - Bundle file name
 - `-s / --script` - Language


## Configuration

`tic-bundle` supports config files. By default, `tic-bundle` looks for a `.ticbundle.js` file in the current directory, but an alternative location can be specified using `-c <file>` or `--config <file>`. 

The specificity is as folows:

 - CLI
 - `.ticbundle.js`
 - `.ticbundle.json`
 - Default config

<b>Default config</b>

```JS
{
  root: 'src',
  metadata: {
    title: null,
    author: null,
    desc: null,
    script: 'js',
    input: null,
    saveid: null
  },
  output: {
    path: './',
    extension: 'js',
    name: 'build'
  },
  files: [],
  after: bundle => bundle  
}
```

### Options

 - `root` (default `src`) - Folder to watch.
 - `metadata` - [Cartridge metadata](https://github.com/nesbox/TIC-80/wiki#cartridge-metadata)
 - `metadata.title` - The name of the cart.
 - `metadata.author` - The name of the developer.
 - `metadata.description` - Optional description of the game.
 - `metadata.script` (default `js`) - Used scripting language.
 - `metadata.input` - Selects gamepad, mouse or keyboard input source.
 - `metadata.saveid` - Allows save data to be shared within multiple games on a copy of TIC.
 - `output.path` (default `./`) - Bundled file output path.
 - `output.name` (default `build`) - Bundled file name.
 - `files` - Files to bundle. Files will be ordered by index (top first, bottom last).
 - `after` - Run after generating the bundle, this can be used to further modify the bundle.

### Babel

`after` can be used to transform the bundled code. A common use-case for `js` code is transforming `ES6` syntax to `ES5`.

<b>Example</b>

`.ticbundle.js`

```JS
const { transformSync } = require('@babel/standalone');
const pluginTransformArrowFunctions = require('@babel/plugin-transform-arrow-functions');

module.exports = {
  after: bundle => {
    const { code } = transformSync(bundle, {
      sourceType: 'script',
      plugins: [pluginTransformArrowFunctions]
    });

    return code;
  }
};

```

## Donating

[![ko-fi](https://www.ko-fi.com/img/githubbutton_sm.svg)](https://ko-fi.com/Y8Y41E23T)
