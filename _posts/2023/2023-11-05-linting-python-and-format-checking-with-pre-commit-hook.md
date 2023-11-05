---
layout: post
title: Linting python and format checking with pre-commit hook
date: 2023-11-05 22:32 +0800
categories: [Gleanings, Technical Supports]
tags: [pre-commit hook, git, python]
math: False
---
# Using Pre-commit Hooks to Lint and Check Format of Python Files

Pre-commit hooks are used to do some pre-commit automation
like code linting and format checking before git commit.
If the hook raise an error exit, for example, the format 
check is failed, then that commit is aborted.

## Using pre-commit package to manage pre-commit hooks

A very user-friendly python package is [pre-commit](https://pre-commit.com/),
which offers maintainence of various pre-commit hooks.

Here we take python linting and format checking as an example
to show how to install `pre-commit` and how to invoke it.

### Installaton

```console
pip install pre-commit
```

then create a file named `.pre-commit-config.yaml` under the 
root directory of your python project.

For code linting and format checking, here is an example file
for `.pre-commit-config.yaml`:

```yaml
repos:
-   repo: https://github.com/psf/black
    rev: 22.10.0
    hooks:
    -   id: black
        types: [file, python]
-   repo: https://github.com/pre-commit/pre-commit-hooks
    rev: v2.3.0
    hooks:
    -   id: check-yaml
    -   id: end-of-file-fixer
        types: [file, python]
    -   id: trailing-whitespace
        types: [file, python]
    -   id: flake8
        types: [file, python]
```

within which we enabled `black` python linting hook and 
`flake8` format checking hook. The `types` below them
specifies which file the specific hook will be preformed.

Then to enable the hooks to run correctly before each
commit, one would

```console
pre-commit install
```

to  install the newly added pre-commit-hooks.

> NOTE to run `pre-commit install` each time after you change
the content of `.pre-commit-config.yaml`
{: .prompt-warning}

### Usage

For all the hooks available, see [list here](https://pre-commit.com/hooks.html).

### Appendix

- If one want to omit some specific PEP8 format standards,
first creating a file named `.flake8` and edit it as 

```
[flake8]
ignore = F841
```

This would ignore the error code F841 in `flake8`
checking.

For `flake8` error codes, see [here](https://flake8.pycqa.org/en/latest/user/error-codes.html#)

For reference of `flake8`, see [here](https://flake8.pycqa.org/en/latest/index.html)

A helpful blog [here](https://ljvmiranda921.github.io/notebook/2018/06/21/precommits-using-black-and-flake8/)
