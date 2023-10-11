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

## A typical workflow

To set a periodic backup, it is recommended to use crontab (more specificlly,
in the root user domain of crontab) to run bup commands.

One could first initialize a bup repo by running:

```console
bup -d /backup/bup_repo init
```

Then this initialize a bup repository under `/backup/bup_repo`

Then one could prepare a bash script like:

`sync_sudo.sh`

```bash
#! bin/bash
# This sync file is intended to run as root
time1=$(date)
echo backup job run as root
echo JOB starting at
echo $time1

# using bup to backup periodically
echo -e '\n backing up /home/ubuntu'
bup -d /backup/bup_repo index /home/ubuntu
bup -d /backup/bup_repo save -n home-ubuntu /home/ubuntu

echo -e '\n running bup fsck to generate recovery blocks'
bup -d /backup/bup_repo fsck -g

echo -e '\n bup job finished'
```
The script above would first index the files in `/home/ubuntu` and then 
make a backup to `/backup/bup_repo` with backup-name `home-ubuntu` which
is maintained by bup.

The `fsck` command should be run each time a backup is finished to enable 
recovery utilities from possible disk damages.(Note this needs `par2` to
be functional, to test if `par2` is ok, see the [man page of bup fsck](https://bup.github.io/man/bup-fsck.html))

To set up a crontab service for `root` user, typically we could

```console
sudo crontab -uroot -e
```

and then adding the following line:

```console
# m h  dom mon dow   command
  0 4 * * * /bin/bash /home/ubuntu/path_to_your_sync_sudo_script/sync_sudo.sh > /home/ubuntu/path_to_your_sync_sudo_script/sync_sudo.log 2>&1
```

This would run a backup following the script `sync_sudo.sh` everyday at 4 am
and saving log to the `sync_sudo.log`

For more information and the usage of `bup`, see the [github repo](https://bup.github.io/)
or the [webpage](https://bup.github.io/).

# Some backup tools reminder

Some other backup tools may be useful in different scenes: 

- [rdiff-backup](https://github.com/rdiff-backup/rdiff-backup)
- [an open source script for automization of rsync command](https://github.com/opensourceway/rsync-backup-script)
