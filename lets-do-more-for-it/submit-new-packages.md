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

The install script downloads the binary file (scripts sometimes) to `~/Documents/bin/` or `~/Library/bin/` that is stored at `$PATH`, then gets the man page and refresh the man database,and downloads the uninstall script finally. The binary file(s) can be stored at a repository (anywhere in fact) and for a single command it can be parsed with `curl`. Here’s it’s uninstall script:

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

The uninstall script just clears everything about the package: the binary file, and the man page. For a comlexer project, remember to remove any app data by your package.

Now we are clarified: an install script is needed for `pkg install` to execute while an uninstall script is as well but for `pkg uninstall`. Usually the man page is also needed although it can be not included sometimes. The install script is stored at the repository and it indicates where the other parts of the package are stored at.

### An example

`cowsay` is an old and famous project written in Perl. To have it installed, we may clone the GitHub mirror repository first:

```
$ lg2 clone https://github.com/schacon/cowsay
```

We can see there is an `install.sh` on the repository. Actually it does not work with a-Shell and `dash` and it’s not essential.
