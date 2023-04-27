# Configure lg2 for version controlling

A number of users have been seeking for an available version controlling tool for a long time. When it comes to that tool for iOS/iPadOS, many users may look at those for a special purpose, like Working Copy or Git Torch. However, the push functions are all collecting fees——Does anyone can accept a “free” Git client without `git push`? Also some Apps have built-in Git support, but none of them is free of charge, except SpckEditor. You can truly use Git on iSH, but the process of it is slow and unstable. Fortunately, a-Shell has a built-in Git support. This article focus on how to use `lg2` for version controlling on a-Shell.

### Git command?

Some new users have already found there is a `git` package on the repository. However, it will just add a link to `lg2` so that you can use `git` command directly. It may be useful but be careful that there are differences between `git` and `lg2`. Due to FSF’s policy, the original `git` can not be included, but `lg2` is enough for most cases. Attention many tools to manage Git directories won’t work because of the differences even when you have `git` package installed.

Now let’s try to install the `git` command.

```
$ pkg install git
Downloading git
The git command cannot be included with
a-Shell. Would you like to create a
script at $HOME/Documents/bin/git that
wraps lg2 with the following contents?
#!/bin/sh
lg2 "$@"
Keep in mind that git and lg2 options
are not 100% compatible, and they also
do not use the same configuration files
or environment variables.
Create $HOME/Documents/bin/git? (y/n [n]) y
The $HOME/Documents/bin directory does not exist.
Creating it first.
Creating $HOME/Documents/bin/git
Creation complete
Done
```

Now you can use either `git` or `lg2` to manage Git repositories.

### SSH configuration

You may want a SSH key to link to GitHub or other Git repositories. Let’s generate an SSH key first.

```
$ ssh-keygen -t ed25519 -c “<user name>”
```

Some users prefer `rsa`. Attention sometimes RSA keys don’t work for GitHub or some other websites because of a confusing SHA-1 problem. If you meet the problem too, try `ed25519` or `ecdsa`. Choose a path and the name for your keys, set a password or not with the tutorial, and then upload your public key to the Git server. You can get a lot of useful information by searching it about how to upload it to GitHub or somewhere else. Now we’ll test it:

```
$ ssh -T git@github.com
Hi <your name>! You've successfully authenticated, but GitHub does not pr
ovide shell access.
```

That’s good. Now we‘ll configure the user name and the email.

```
$ lg2 config —-global user.name “<your name>”
$ lg2 config —-global user.email “<your email>”
```

### Cloning and other operations

You can clone any repositories naturally:

```
$ lg2 clone https://github.com/holzschu/a-shell.git
```

Then you’ll see `a-shell.git` on the current dictionary. On the contrary, for a normal computer with standard `git` command, the dictionary would be named `a-shell`. Actually, all basic commands works well including `lg2 push origin`, but there are still some won’t work, like drawing the commit graph. Enjoy your version controlling trip!

### Does a-Shell support Subversion, CVS or other alternatives?

No. Open an issue if you want.
