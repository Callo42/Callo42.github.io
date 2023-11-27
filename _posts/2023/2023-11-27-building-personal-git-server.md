---
layout: post
title: Building Personal Git Server
date: 2023-11-27 13:15 +0800
categories: [Gleanings, Technical Supports]
tags: [Git, Git server]
math: false
---

# Building Personal Git Server
 - In some situations, one may want to build up their own git server, 
either on a server that one has access or simply on another computer 
that is connect to the same LAN (local area network). Here is a brief
introduction for the latter one.

## Prerequests

- Let's say, one have two computers connect to the same LAN, with the
following hostname and ip address:

| computer | hostname | ip address| ssh port |
| -------- | -------- | --------- | -------- |
| git server   |  server |   192.168.1.1 | 8801 | 
| git user  |   user    | 192.168.1.2   | | 

- And you want to establish git server service on your `server`
machine, which allows you to modify your git repo on your
`user` machine and then push to `server`.

## Building

- Most of the contents here are from [This helpful website](https://linuxize.com/post/how-to-setup-a-git-server/).
- First make sure that on your `server` machine, the software `git` is 
successfully installed.
- Then, one need to create a user on `server` to host git server service:

```console
sudo useradd -r -m -U -d /home/git -s /bin/bash git
```

- Here the `-d` command specifies the home directory of the new user
    git and `-s` specifies the shell that user would use.
- Note All the repositories will be stored under the home directory of
    the user git. We did not set a password for the user “git”, the 
    login will be possible only using the ssh keys.
- To change current user into git,
    ```console
    sudo su - git
    ```
- Then create relavent ssh directory and files for ssh service:
    ```console
    mkdir -p ~/.ssh && chmod 0700 ~/.ssh
    touch ~/.ssh/authorized_keys && chmod 0600 ~/.ssh/authorized_keys
    ```
- One would then copy his or her ssh public key into 
  `/home/git/.ssh/authorized_keys`. For example, on one's `user` machine, locate 
  the `.ssh` directory, then cat one of the existing public key, like
  ```console
  cat ~/.ssh/id_rsa.pub
  ```
  and then copy the output into `authorized_keys`. Note that the entire 
  public key text should be on a single line.
- Then run the following command to initialize a bare git repo
    ```console
    git init --bare ~/projectname.git
    ```
    The `--bare` attribute here is vital for git server, since it
    will not save the workspace on the `server` machine (But files
    of the repo is still kept on `server`, the only difference is 
    these files are not directly displayed then you `ls` in the 
    `server`'s git repo), which would enable the push command from
    other machines to `server`. The git repo on `server` is only for
    serving different git clients for pull and push.
- Then `cd` to the git directory of your project on `user` machine
    ```console
    cd /path/to/local/project
    ```
- Adding the git server on `server` machine:
    ```console
    git remote add origin ssh://git@<host ip>:<ssh port>/home/git/projectname.git
    ```
    Note here the command is specified for a `server` machine whose 
    ssh service is enabled on a different port rather than 22 using ssh
    protocol. 
    
    For example, for the machines we assumed before, this would be

    ```console
    git remoe add origin ssh://git@192.168.1.1:8801/home/git/projectname.git
    ```

- Uploading repo
  - After that, one would simply upload an existing repo by

    ```console
    git push -u origin 
    ```


