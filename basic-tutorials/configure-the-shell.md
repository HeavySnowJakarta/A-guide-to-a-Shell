# Configure the Shell

Because of the limitations of [a-shells-differences.md](a-shells-differences.md "mention"), `bash` under GPL can not be included. Instead, the default (and the only) shell of a-Shell is `dash`. There are almost no ways to customize `dash`, but there are some differences with our shell. When a-Shell starts, the shell reads `~/Documents/.profile` and `~/Documents/.bashrc` first, so it’s possible to customize its shell. But before that, let’s see if it can run Shell scripts.

### Execute scripts

Shell scripts can be executed. But one thing should be noticed: a number of scripts relies on `bash` actually. The first lines of some scripts is not `/user/bin/env sh` but `/user/bin/env bash`, making it not work, even including original `neofetch`. You can try to change `bash` to `dash` or `sh`, but all scripts using the features of `bash` won’t work anyway. In many cases you have to rewrite the script to avoid that.
