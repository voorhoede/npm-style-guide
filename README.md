# NPM Style Guide

Opinionated ​*NPM Style Guide*​ for teams by [De Voorhoede](https://twitter.com/devoorhoede).

## Purpose

This guide provides a set of rules to better manage, test and build your [NPM](https://npmjs.org) modules and project tasks runners. It should make them

* easier for a new developer to pick up
* reduce friction with different environment configurations
* have a predictable api
* easier to add new tasks

## Table of Contents

* [Use nvm to manage node versions](#use-nvm-to-manage-node-versions)
* [Configure your npm personal info](#configure-your-npm-personal-info)
* [Use `save exact` option](#use-save-exact-option)
* [Avoid installing modules globally](#avoid-installing-modules-globally)
* [Write atomic tasks](#write-atomic-tasks)
* [Use npm modules for system tasks](#use-npm-modules-for-system-tasks)


## Use nvm to manage node versions 

### Why?

With [nvm](https://github.com/creationix/nvm) you can have multiple different versions available and switch to the one that suits better your project.

### How?

To install or update nvm, you can use the install script using cURL:

```bash
curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.31.0/install.sh | bash
```

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

## Avoid installing modules globally

NPM first tries globally installed modules before looking for local ones. Globally installed modules are shared between projects and might not match the required version for the project.

### Why?

* Locally installed modules are custom and specific for the project.
* Locally installed modules are directly accessible via `npm scripts`.


### How?

```bash
# recommended
npm install grunt-cli grunt
```
and use in `package.json`:
```json
"scripts": {
  "icons": "grunt grunticons"
}
```
```bash
# avoid
npm install -g grunt-cli grunt
```

More about [`npm scripts`](https://docs.npmjs.com/misc/scripts).

[↑ back to Table of Contents](#table-of-contents)

## Write atomic tasks

Each task should be only responsible for one function. 

### Why?

    Atomic tasks are easy to read and understand
    Atomic tasks are easy to reuse.

### How?

Separate each step of the task to an individual task, e.g:

    generate icon
        clean directory
        optimize svg
        generate png
        genrate data-uri for svg


[↑ back to Table of Contents](#table-of-contents)

## Use npm modules for system tasks

### Why?

When you use system specific commands like `rm -rf` or `&&`, you are locking your tasks to your current system. If you want to make your scripts work everwhere think about windows developers also.

### How?

Use npm modules with node that mimic the same tasks but are system agnostic. For the examples above your could use [`rimraf`](https://www.npmjs.com/package/rimraf) for deleting files and [`npm-run-all`](https://www.npmjs.com/package/npm-run-all) to run tasks in parallel.

[↑ back to Table of Contents](#table-of-contents)
