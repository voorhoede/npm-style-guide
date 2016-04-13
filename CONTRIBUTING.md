# Contributing

For new additions or changes to the guide, create a branch and submit a Pull Request.
Only add/change 1 style guide rule per Pull Request.
The Pull Request serves as a place to discuss and refine the additions/changes.

## Format

Use [Github Flavoured Markdown](https://guides.github.com/features/mastering-markdown/#GitHub-flavored-markdown).

Use short and descriptive rule names as 2nd level heading:

```markdown
## Keep expressions simple
```
  
Optionally add an short summary for the rule.

Describe **why** the rule exists. What purpose does it serve?

```markdown
### Why?

With nvm you can have multiple different versions available and switch to the one that suits better your project.
```

Describe **how** (and how not) to apply the rule.

```markdown
### How?

To install or update nvm, you can use the install script using cURL
```

When using code snippets:
* use `<!-- recommended -->` and `/* recommended */` or `<!-- avoid -->` and `/* avoid */` at the start of the snippet to indicate if it's a good or bad practice.

When in need refer to npm in lowercase.

Add a [↑ back to Table of Contents](README.md#table-of-contents) link at the end of each rule, so the reader can easily navigate back:

```markdown
[↑ back to Table of Contents](#table-of-contents)
```
