# NPM Style Guide

Opinionated ​*NPM Style Guide*​ for teams by [De Voorhoede](https://twitter.com/devoorhoede).

## Purpose

This guide provides a set of rules to better manage, test and build your NPM modules and project tasks runners. It should make them

* easier for a new developer to pick up
* reduce friction with different environment configurations
* have a predictable api
* easier to add new tasks

## Table of Contents

* [Use nvm to manage node versions](#use-nvm-to-manage-node-versions)
* [Configure your npm personal info](#configure-your-npm-personal-info)
* [Use `save exact` option](#use-save-exact-option)
* [Write atomic tasks](#write-atomic-tasks)


## Use nvm to manage node versions 

### Why?

With [nvm](https://github.com/creationix/nvm) you can have multiple different versions available and switch to the one that suits better your project.

### How?

To install or update nvm, you can use the install script using cURL:

```bash
curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.31.0/install.sh | bash
```

If everything goes good, you can now install the latest node version.

```bash
nvm install stable
nvm alias default stable
``` 

It’s also easy when updating a newer version, copying your existing modules.

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

You even set more initial default information. Check [npm config](https://docs.npmjs.com/misc/config#init-module) docs. 

[↑ back to Table of Contents](#table-of-contents)

## Use save exact option

### Why?

By default, installing a package with the `—save` option, npm saves the package version with `^` prefix, meaning that will update minor versions if available. While this is a good idea as is, this makes it possible for different developers having different versions of the same package and making it harder to debug if there is inconsistency.  

### How?

```bash
npm config set save-exact
```

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