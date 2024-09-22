# Submit new packages

Sometimes you want to release your projects to the `pkg` repository so that everyone can use it. So far the most packages are distributed in WebAssembly, but actually almost everything running well with a-Shell can be released(except native binary codes). This article focus on making a project ready for being released at the `pkg` repository.

### The structure of a package

The offical repository is stored at [https://github.com/holzschu/a-Shell-commands](https://github.com/holzschu/a-Shell-commands). See the repository for it's file structure:

```plain
.
├──man
├──packages
├──uninstall
├──list
├──pkg
└──README.md
```

Among them,

+ `man/man[0-9]`: The man files, stored at the corresponding directories.
+ `packages`: The install scripts.
+ `uninstall`: The uninstall scripts.
+ `list`: A list of available packages, one for per line.
+ `pkg`: The script to manage the pankages.


Therefore, to prepare a package, three parts are needed: the install script, the man page and the uninstall script. Let’s see an example `expand`.

Here is its install script:

```sh
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

```sh
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

Now we have clarified: an install script is needed for `pkg install` to execute while an uninstall script is needed as well, except it is for `pkg uninstall`. Usually the man page is also needed although it isn't required because it isn't always necessary. The install script is stored at the repository and it indicates where the other parts of the package are stored at.

### An example

`checkbashisms` is a part of [`devscripts`](https://salsa.debian.org/debian/devscripts), which can check if the scripts starting with `#!/usr/bin/sh` are compatible with Dash (which had better be just POSIX-compatible). To add it as a a-Shell package, get the shell source file from the source site first. Here we'll use the tag `v2.24.1` as example:

```perl
#!/usr/bin/perl

# This script is essentially copied from /usr/share/lintian/checks/scripts,
# which is:
#   Copyright (C) 1998 Richard Braakman
#   Copyright (C) 2002 Josip Rodin
# This version is
#   Copyright (C) 2003 Julian Gilbey
#
# This program is free software; you can redistribute it and/or modify
# it under the terms of the GNU General Public License as published by
# the Free Software Foundation; either version 2 of the License, or
# (at your option) any later version.
# ...

use strict;
use warnings;
use Getopt::Long qw(:config bundling permute no_getopt_compat);
use File::Temp   qw/tempfile/;
...
```

That file is all we need. As a Perl script, it just runs with a-Shell and no compiling is needed. Import the file to a-Shell and we'll see it working, so it's the time to commit it to the `pkg` repository. All we have to do are:

+ Fork [a-Shell-commands](https://github.com/holzschu/a-Shell-commands) (REMEMBER TO CREATE A NEW BRANCH FOR YOUR PACKAGE!).
+ Release the executable file with GitHub Release, where the install script will download the file from that link.
+ Write your scripts to install and uninstall, and put the man page file there.
+ Add the info to `list` (and perhaps `README.md`).
+ Open a PR as the scripts will work as long as the changes are merged.

From the Debian's repository we can get the man file `checkbashisms.1`, which ends with a single number `.1`. Release it together with the Perl script above. The man file will be put into the corresponding `man` directory according to its number, so our man page here will be stored at the directory `man/man1`.

Now let's write the script to install (after creating the release, [checkbashisms's release](https://github.com/HeavySnowJakarta/a-Shell-commands/releases/tag/checkbashisms-v2.24.1) as example here).

```sh
#!/bin/sh
# install script for checkbashisms

packagename=${0##*/}

# download command:
curl -L https://github.com/HeavySnowJakarta/a-Shell-commands/releases/download/checkbashisms-v2.24.1/checkbashisms -o ~/Documents/bin/$packagename --create-dirs --silent
chmod +x ~/Documents/bin/$packagename
# download man page
curl -L https://raw.githubusercontent.com/holzschu/a-Shell-commands/master/man/man1/$packagename.1 -o ~/Library/man/man1/$packagename.1 --create-dirs --silent
# refresh man database
mandocdb ~/Library/man
# download uninstall information:
curl -L https://raw.githubusercontent.com/holzschu/a-Shell-commands/master/uninstall/$packagename -o ~/Documents/.pkg/$packagename --create-dirs --silent
```

There are two assumptions here:

* The executable file is available from GitHub Release. You either let `holzschu` to release it, or fork it to your own account and then release it there (we choose the latter here).
* The script will be availabe ONCE it's merged into the original repository as the script is trying to get the files from it. It won't work if the files can not be found from the original repository.

The uninstall script will be much more simple——it just deletes the files related to the program.

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

Put the scripts and the man pages to the repository by the file structure of it:

```
a-shell-commands
├─man
│ ├─man1
│ │ ├─others
│ │ └─checkbashisms.1 # the man page
│ └─man6
├─packages
│ ├─others
│ └─checkbashisms # the install script
├─uninstall
│ ├─others
│ └─checkbashisms # the uninstall script
├─list
├─pkg
└─README.md
```

Then add the word `checkbashisms` into the file `list`:

```diff
...
base64
cal
+checkbashisms
col
colrm
...
```

Also the file `README.md` (as it's under GPL):

```diff
+[checkbashisms](https://salsa.debian.org/debian/devscripts)
```

Then open a PR. That's all you have to do.