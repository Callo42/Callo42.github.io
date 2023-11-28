---
layout: post
title: Linting python and format checking with pre-commit hook
date: 2023-11-05 22:32 +0800
categories: [Gleanings, Technical Supports]
tags: [pre-commit hook, git, python]
math: False
---
## Using Pre-commit Hooks to Lint and Check Format of Python Files

Pre-commit hooks are used to do some pre-commit automation
like code linting and format checking before git commit.
If the hook raise an error exit, for example, the format 
check is failed, then that commit is aborted.

### Using pre-commit package to manage pre-commit hooks

A very user-friendly python package is [pre-commit](https://pre-commit.com/),
which offers maintainence of various pre-commit hooks.

Here we take python linting and format checking as an example
to show how to install `pre-commit` and how to invoke it.

#### Installaton

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

#### Usage

For all the hooks available, see [list here](https://pre-commit.com/hooks.html).

To perform a pre-commit check for all files, one may run

```console
pre-commit run --all-files
```

This performs a pre-commit hook workflow for all the files in your 
working directory.

> Resolving Compatibility: by default, there might be incompatibility
between `flake8` and `black`, see the following instructions for
soving this.
{: .prompt-warning}

The solution is provided by [This Q&A](https://stackoverflow.com/questions/68161741/python-black-formatter-conflict-with-rule-flake8-w503-in-vscode)
and [The official docs from black](https://github.com/psf/black/blob/06ccb88bf2bd35a4dc5d591bb296b5b299d07323/docs/guides/using_black_with_other_tools.md#flake8).
The former one explains why use `extend-ignore` rather than `ignore` in flake8
config file, and the latter one offers an example config file for using flake8
with black.

- First, create a file named `.flake8` in the home directory
of your project.
- Then edit it as

```
[flake8]
max-line-length = 88
extend-ignore = E203
```

- This would set the max line length to 88, which is used as default by
`black` and ignore the error code `E203` which is not compatible with
black. The reason for this is explained [here](https://github.com/psf/black/blob/06ccb88bf2bd35a4dc5d591bb296b5b299d07323/docs/guides/using_black_with_other_tools.md#flake8).
- The `extend-ignore` used here is to avoid overwritting
the default ignore settings. For example, `W503` is disabled by default, so
if one use `extend-ignore` then this only **adds** a new ignore rule without
modifying the original default. But if one uses `ignore = E203` then it set
the entire ignore rule from scratch, which would in turn enable the `W503`
and causing conflicts.

#### Appendix

For `flake8` error codes, see [here](https://flake8.pycqa.org/en/latest/user/error-codes.html#)

For reference of `flake8`, see [here](https://flake8.pycqa.org/en/latest/index.html)

A helpful blog [here](https://ljvmiranda921.github.io/notebook/2018/06/21/precommits-using-black-and-flake8/)
