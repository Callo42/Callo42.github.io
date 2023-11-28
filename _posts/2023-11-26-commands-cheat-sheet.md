---
layout: post
title: Commands Cheat Sheet
date: 2023-11-27 14:05 +0800
categories: [Favourites, Technical Supports]
tags: [Command Line, Cheat Sheet]
math: false
---
## Commands Cheat Sheet
Recording commands that I have used during DevOps, and 
providing a quick reference for future use.


## Git

- Formatting git log output (from [Liao Xuefeng's blog](https://www.liaoxuefeng.com/wiki/896043488029600/898732837407424))
    ```console
    git config --global alias.lg "log --color --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit"
    ```
