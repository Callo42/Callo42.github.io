---
layout: post
title: 'bup: Incremental Backup Tool for Linux'
date: 2023-10-10 14:28 +0800
categories: [Gleanings, Technical Supports]
tags: [bup, backup tool]
math: False
---

# bup: A Linux backup tool

[bup](https://bup.github.io/) is a backup tool for creating incremental backups for Linux
system. It saves files in a compressed way and creating snapshots each time a backup is made 
by creating links bewteen backups.

It also supports remote backup, like rsync.

For more information and the usage of `bup`, see the [github repo](https://bup.github.io/)
or the [webpage](https://bup.github.io/).

# Some backup tools reminder

Some other backup tools may be useful in different scenes: 

- [rdiff-backup](https://github.com/rdiff-backup/rdiff-backup)
- [an open source script for automization of rsync command](https://github.com/opensourceway/rsync-backup-script)
