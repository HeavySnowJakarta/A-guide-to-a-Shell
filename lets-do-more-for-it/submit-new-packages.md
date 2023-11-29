# Submit new packages

Sometimes you want to release your projects to the `pkg` repository so that everyone can use it. So far the most packages are distributed in WebAssembly, but actually almost everything running well with a-Shell can be released(except native binary codes). This article focus on making a project ready for being released at the `pkg` repository.

### The structure of a package

The offical repository is stored at [https://github.com/holzschu/a-Shell-commands](https://github.com/holzschu/a-Shell-commands). To prepare a package, three parts are needed: the install script, the man page and the uninstall script. Let’s see an example `expand`.

Here is its install script:

```bash
#! /bin/sh
# Default install file for packages:

packagename=${0##*/}

# download command:
curl -L https://github.com/holzschu/a-Shell-commands/releases/download/0.1/$packagename -o ~/Documents/bin/$packagename --create-dirs --silent
chmod +x ~/Documents/bin/$packagename
# download man page
curl -L https://raw.githubusercontent.com/holzschu/a-Shell-commands/master/man/man1/$packagename.1 -o ~/Library/man/man1/$packagename.1 --create-dirs --silent
# refresh man database
mandocdb ~/Library/man
# download uninstall information:
curl -L https://raw.githubusercontent.com/holzschu/a-Shell-commands/master/uninstall/$packagename -o ~/Documents/.pkg/$packagename --create-dirs --silent
```

The install script downloads the binary file (scripts sometimes) to `~/Documents/bin/` or `~/Library/bin/` that is stored at `$PATH`, then gets the man page, refreshes the man database,and finally downloads the uninstall script. The binary file(s) can be stored at a repository (anywhere in fact) and for a single command it can be parsed with `curl`. Here’s it’s uninstall script:

```bash
#! /bin/sh

# Default uninstall file for packages:
packagename=${0##*/}

# remove command
rm ~/Documents/bin/$packagename
# remove man page
rm ~/Library/man/man1/$packagename.1
# refresh man database
mandocdb ~/Library/man
```

The uninstall script just clears everything about the package: the binary file, and the man page. For a more comlex project, remember to remove any app data by your package.

Now we have clarified: an install script is needed for `pkg install` to execute while an uninstall script is needed as well, except it is for `pkg uninstall`. Usually the man page is also needed although it isn't required because it isn't always necissary. The install script is stored at the repository and it indicates where the other parts of the package are stored at.

### An example

{% hint style="info" %}
This part is still on progress. A problem has to be clarified: How to release the directory `cows`?
{% endhint %}

`cowsay` is an old and famous project written in Perl. To have it installed, we may clone the GitHub mirror repository first:

```
$ lg2 clone https://github.com/schacon/cowsay
```

We can see there is an `install.sh` on the repository. Actually it does not work with a-Shell and `dash` while it’s not essential. We’ll rewrite the installation script for it.

`install.sh` searches for if Perl exists on the computer which is not necessary for a-Shell so this part will not be used. According to the file `MANIFEST` of the repository, the project has the following files:

```
ChangeLog		Changes to recent versions.
INSTALL			Instructions for installing cowsay.
LICENSE			The license for use and redistribution of cowsay.
MANIFEST		This file.
README			Read this first.  Really.
Wrap.pm.diff		Diff for Text/Wrap.pm.
cows/*			Support files used by cowsay.
cowsay			Main cowsay executable.
cowsay.1		Main cowsay manual page.
install.sh		cowsay installation script.
pgp_public_key.txt	Verify the signature file with this key.
```

Among them, three files are important:

* `cowsay`, a script written in Perl, the most important executable file
* `cows/*`, a directory storing the source files used by `cowsay`
* `cowsay.1`, the man page (`man1`) of `cowsay`

The file ending with a single number is the man page here. We just put it into the corresponding `man` directory according to its number. For example, `*.1` files can be stored at the directory `man/man1`.

These files will be copied to where they should be on the computer, `/usr/local/bin` for `cowsay` and `/usr/local/share` for `cows/*`. as what `install.sh` tells us. According to the unique file system of a-Shell, `/usr` will be replaced into `~/Library`. For `cowsay.1`, we'll copy it to `~/Library/man/man1/$packagename.1` just like other packages. Thus, our `install.sh` may seems like this (not considering `cows/*`):

```sh
#!/bin/sh
# install script for cowsay

packagename=${0##*/}

# download command:
curl -L <somewhere> -o ~/Documents/bin/$packagename --create-dirs --silent
chmod +x ~/Document/bin/$packagename
# download man page
curl -L https://raw.githubusercontent.com/holzschu/a-Shell-commands/master/man/man1/$packagename.1 -o ~/Library/man/man1/$packagename.1 --create-dirs --silent
# refresh man database
mandocdb ~/Library/man
# download uninstall information
curl -L https://raw.githubusercontent.com/holzschu/a-Shell-commands/master/uninstall/$packagename -o ~/Documents/.pkg/$packagename --create-dirs --silent
```

There are two assumptions here:

* The executable file is available from GitHub Release. You either let `holzschu` to release it, or fork it to your own account and then release it there.
* The script will be availabe ONCE it's merged into the original repository as the script is trying to get the files from it. It won't work if the files can not be found from the original repository.

Now let's consider `cow/*`. It can be packed into a `tar` archive and be released. Now our script looks like this:

```
```

The uninstall script will be much more simple——it just deletes the files related to the program.

```bash
#! /bin/sh

# Default uninstall file for packages:
packagename=${0##*/}

# remove command
rm ~/Documents/bin/$packagename
# remove "cows"
rm -r ~/Library/local/share/cows
# remove man page
rm ~/Library/man/man1/$packagename.1
# refresh man database
mandocdb ~/Library/man
```

Put the scripts and the man pages to the repository by the file structure of it:

```
a-shell-commands
├─man
│ ├─man1
│ │ ├─others
│ │ └─cowsay.1 # the man page
│ └─man6
├─packages
│ ├─others
│ └─cowsay # the install script
├─uninstall
│ ├─others
│ └─cowsay # the uninstall script
├─list
├─pkg
└─README.md
```

Then add the word `cowsay` into the file `list`:

```
...
column
comm
cowsay
csplit
cut
...
```

Release the excutable file `cowsay` and the directory `cows`, commit your changes, synchronize it to the remote server, and open a Pull Request finally. That's all you need to do to submit a new package.
