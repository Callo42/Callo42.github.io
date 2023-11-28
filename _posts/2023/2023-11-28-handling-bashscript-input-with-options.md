---
layout: post
title: Handling BashScript Input With Options
date: 2023-11-28 15:39 +0800
categories: [Gleanings, Technical Support]
tags: [Bash Script]
math: false
---

# Handling Bash Scripts Input With Options

- To write bash scripts that accepts options and
arguments in input, like most linux command does:

```console
  command [OPTIONS] [ARGUMENTS]
```

- One could insert the following scripts into 
bash script:[reference](https://stackoverflow.com/questions/7069682/how-to-get-arguments-with-flags-in-bash)

```shell
a_flag=''
b_flag=''
files=''
verbose='false'

print_usage() {
  printf "Usage: ..."
}

while getopts 'abf:v' flag; do
  case "${flag}" in
    a) a_flag='true' ;;
    b) b_flag='true' ;;
    f) files="${OPTARG}" ;;
    v) verbose='true' ;;
    *) print_usage
       exit 1 ;;
  esac
done
```

- Which is actually following the [Google Shell Style Guide](https://google.github.io/styleguide/shellguide.html)
- Things one should notice while using `getopts` is as stated
in the reference:

- Note: If a character is followed by a colon (e.g.  `f:`), that option is expected to have an argument.

- Example usage: `./script -v -a -b -f filename`

- Using getopts has several advantages over the accepted answer:

  - the while condition is a lot more readable and shows what the accepted options are
  - cleaner code; no counting the number of parameters and shifting 
  - you can join options (e.g. -a -b -c → -abc)
  - However, a big disadvantage is that it doesn't support long options, only single-character options.
