# npm Style Guide

Opinionated ​*npm Style Guide*​ for teams by [De Voorhoede](https://twitter.com/devoorhoede).

## Purpose

This guide provides a set of rules to better manage, test and build your [npm](https://npmjs.org) modules and project tasks runners. It should make them

* easier for a new developer to pick up
* reduce friction with different environment configurations
* have a predictable api
* easier to add new tasks

## Table of Contents

* [Use nvm to manage node versions](#use-nvm-to-manage-node-versions)
* [Configure your npm personal info](#configure-your-npm-personal-info)
* [Use `save exact` option](#use-save-exact-option)
* [Specify engines on `package.json`](#specify-engines-on-package.json)
* [Avoid installing modules globally](#avoid-installing-modules-globally)
* [Write atomic tasks](#write-atomic-tasks)
* [Group related tasks by prefix](#group-related-tasks-by-prefix)
* [Use npm modules for system tasks](#use-npm-modules-for-system-tasks)
* [Document your script API](#document-your-script-API)
* [Avoid shorthand command flags](#avoid-shorthand-command-flags)


## Use nvm to manage node versions

### Why?

With [nvm](https://github.com/creationix/nvm) you can have multiple different versions available and switch to the one that suits better your project.

### How?

To install or update nvm, you can use the install script using cURL:

```bash
curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.31.0/install.sh | bash
```

For Windows check [nvm for windows](https://github.com/coreybutler/nvm-windows).


If everything goes well, you can now install a specific node version.

```bash
nvm install stable
nvm install vX.Y.Z
nvm alias default stable
```

It’s also easy when updating a newer version, copying your existing global modules.

```bash
nvm copy-packages <previous-version>
```

[↑ back to Table of Contents](#table-of-contents)

## Configure your npm personal info

### Why?

When creating a new package `npm init` your defaults will be already included on the scaffolding.

### How?

```bash
npm config set init-author-name "{name}"
npm config set init-author-email "{email}"
```

Check [npm config](https://docs.npmjs.com/misc/config#init-module) docs, for more info.

You can use `cat ~/.npmrc` to check your current definitions.  

[↑ back to Table of Contents](#table-of-contents)

## Use `save exact` option

### Why?

By default, installing a package with the `--save` or `--save-dev` option, npm saves the package version with `^` prefix, meaning that will update minor versions if available. While this is a good idea as is, this makes it possible for different developers having different versions of the same package and making it harder to debug if there is inconsistency. Defining the `save-exact` option prevents this. More info [npm config](https://docs.npmjs.com/misc/config#save-exact) docs.

### How?

```bash
npm config set save-exact
```

[↑ back to Table of Contents](#table-of-contents)

## Specify engines on `package.json`

### Why?

Specifying engine versions on your `package.json` module, warns the user if he is not using a supported version. This is specially important for ensuring npm@3 flat tree dependency on Windows, or ES2015 features that your tasks require on node.

### How?

```javascript
"engines" : {
  "node" : "5.10.0",
  "npm" : "3.8.5"
}
```

Preventing the user from using your module is also possible with [check-pkg-engines](https://www.npmjs.com/package/check-pkg-engines).

[↑ back to Table of Contents](#table-of-contents)

## Avoid installing modules globally

NPM first tries globally installed modules before looking for local ones. Globally installed modules are shared between projects and might not match the required version for the project.

### Why?

* Locally installed modules are custom and specific for the project.
* Locally installed modules are directly accessible via **npm scripts**.


### How?

```bash
# recommended: install locally
npm install --save-dev grunt-cli grunt
```
and use in `package.json`:
```json
"scripts": {
  "icons": "grunt grunticon"
}
```
```bash
# avoid: don't install modules globally
npm install -g grunt-cli grunt
```

More about [**npm scripts**](https://docs.npmjs.com/misc/scripts).

[↑ back to Table of Contents](#table-of-contents)

## Write atomic tasks

Each task should be only responsible for one action.

### Why?

* Atomic tasks are easy to read and understand.
* Atomic tasks are easy to reuse.

### How?

Separate each step of the task to an individual task. For example a "generate icon" task can be split into atomic tasks like "clean directory", "optimize SVGs", "generate PNGs" and "generate data-uris for SVGs".


[↑ back to Table of Contents](#table-of-contents)

## Group related tasks by prefix

Bundle your tasks with a prefix so you can execute them all at once.

### Why?

* Bundling helps keeping your tasks organized.
* Tasks grouped by prefix can be easily executed with one command.
* Your high-level script API remains unchanged when tasks are added, removed or renamed.

### How?

`package.json`:
```javascript
/* recommended: group related tasks by prefix */
scripts: {
    "test": "npm run eslint && npm run unit && npm run e2e",
	"test:eslint": "eslint src/**/*.js",
	"test:unit": "tape --require dist/index.js src/**/*.test.js",
	"test:e2e": "karma start test/config.js"
}

/* avoid */
scripts: {
	"eslint": "eslint src/**/*.js",
	"tape": "tape --require dist/index.js src/**/*.test.js",
	"karma": "karma start test/config.js",
}
```

Bundled scripts can be executed (in parallel or in sequence) using [npm-run-all](https://www.npmjs.com/package/npm-run-all):

```javascript
/* recommended: use `npm-run-all` to run all bundled tasks */
scripts: {
    "test": "npm-run-all test:*",
	"test:eslint": "eslint src/**/*.js",
	"test:unit": "tape --require dist/index.js src/**/*.test.js",
	"test:e2e": "karma start test/config.js"
}
```

[↑ back to Table of Contents](#table-of-contents)

## Use npm modules for system tasks

### Why?

When you use system specific commands like `rm -rf` or `&&`, you are locking your tasks to your current operating system. If you want to make your scripts work everywhere think about Windows developers also.

### How?

Use npm modules with node that mimic the same tasks but are system agnostic. Some examples:

* create directory (`mkdir` / `mkdir -p`) -> [`mkdirp`](https://www.npmjs.com/package/mkdirp)
* remove files and directories (`rm ...`) -> [`rimraf`](https://www.npmjs.com/package/rimraf)
* copy files (`cp ...`) -> [`ncp`](https://www.npmjs.com/package/ncp)
* run multiple tasks in sequence (`... && ...`) or in parallel (`... & ...`) -> [`npm-run-all`](https://www.npmjs.com/package/npm-run-all)
* set environment variable (`ENV_VAR = ...`) -> [`cross-env`](https://www.npmjs.com/package/cross-env)

[↑ back to Table of Contents](#table-of-contents)

## Document your script API

### Why?

* Documentation provides developers with a high level overview to the script, without the need to go through all its code. This makes a module more accessible and easier to use.

* Documentation formalises the API.

### How?

Document your script API in the project's README.md or CONTRIBUTING.md as those are the first places contributors will look.
Describe what each task using a simple table:
```markdown
`npm run ...` | Description
---|---
task | What it does as a plain human readable description.
```

An example:

`npm run ...` | Description
---|---
`build` | Compile, bundle and minify all CSS and JS files..
`build:css` | Compile, autoprefix and minify all CSS files to `dist/index.css`.
`build:js` | Compile, bundle and minify all JS files to `dist/index.js`.
`start` | Starts a server on `http://localhost:3000`.
`test` | Run all unit and end-to-end tests.
=======

## Avoid shorthand command flags

npm and npm modules with a command-line interface support different options using fully written out and / or shorthand flags. For instance, instead of `npm install --save-dev` you can use the shorter `npm i -D`. For `npm test` you can use simply `npm t`. But `npm start` is not the same as `npm s`, as that's an alias for `npm search`. So while you can use these shorthands in your daily routine, you should avoid them in scripts and documentation shared with other developers.  

## Why?

* Shorthand flags can only be understood by developers who know the modules and options well.
* Fully written out command options help in writing self documented scripts.
* Fully written out command options make scripts more accessible to other developers.

## How?

Always prefer fully written command flags over shorthand. Example using [uglifyjs](https://www.npmjs.com/package/uglify-js):

```bash
# recommended
uglify index.js --compress --mangle --reserved '$' --output index.min.js
```

```bash
# avoid
uglifyjs index.js -c -m -r '$' -o index.min.js
```

[↑ back to Table of Contents](#table-of-contents)
