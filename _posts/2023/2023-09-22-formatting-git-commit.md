---
layout: post
title: Formatting Git Commit
date: 2023-09-22 11:04 +0800
categories: [Gleanings, Technical Supports]
tags: [git]
math: false
---

# Formatting Git Commits
Using a uniform paradigm while commiting changes is recommended. Here one such tool is introduced to help generate standard git commit message.

## Commitizen
- `Commitizen` is a commit message formatting tool that gives prompts while committing. See <https://github.com/commitizen/cz-cli> for installation instructions.
  - run
    ```console
    npm install -g commitizen
    ```
    to install. If meet `EACCES` error, please follow the instructions [here](https://docs.npmjs.com/resolving-eacces-permissions-errors-when-installing-packages-globally) to reinstall npm by [node version manager](https://docs.npmjs.com/downloading-and-installing-node-js-and-npm):
    - See github repo <https://github.com/nvm-sh/nvm#install--update-script>
    - Then install node by 
        ```console
        nvm install node # "node" is an alias for the latest version
        ```
    - This would install a new version of node and npm, then rerun the previous command to install commitizen.
- It uses `Angular Commit Guidelines` by default. To enable `commitizen` for a git repository, run
  ```console
  $ commitizen init cz-conventional-changelog --save --save-exact
  ```
  under your git repo
- Then, to create a Angular styled commit, simply use
  ```console
  git cz
  ```
  to replace the original ``git commit`` command, then following the prompts, and your commit is done.
- For more information about the commit format or **how to generate change log automatically**, see [the bolg here](http://www.ruanyifeng.com/blog/2016/01/commit_message_change_log.html)
