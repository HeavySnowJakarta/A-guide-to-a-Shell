# Running codes

a-Shell supports the following coding languages:

+ Python (with `pip`)
+ Lua
+ Perl
+ JavaScript (without `npm`)
+ C/C++ (with WebAssembly)
+ Fortran (using `f2c`)

For JavaScript, see [Run JavaScript](../lets-do-more-for-it/run-javascript.md). For C/C++, see [Compile a simple command](../lets-do-more-for-it/compile-a-simple-command-with-a-shell.md).

### Python 3

you can run Python easily.

```sh
$ python
>>> print (“Hello, world!”)
Hello, world!
```

You can also install modules using `pip`. So far `clang` can not deal with Python modules properly, so they must be written in pure Python.

```sh
$ pip install requests
Defaulting to user installation because normal site-packages is not writeable
Collecting selenium
  Downloading selenium-4.8.3-py3-none-any.whl (6.5 MB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 6.5/6.5 MB 2.9 MB/s eta 0:00:00
Requirement already satisfied: urllib3[socks]~=1.26 in /private/var/containers/Bundle/Application/C3889491-0CAD-4C9D-8B01-39773D35A63A/a-Shell.app/Library/lib/python3.11/site-packages (from selenium) (1.26.13)
Collecting trio~=0.17
  Downloading trio-0.22.0-py3-none-any.whl (384 kB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 384.9/384.9 kB 4.8 MB/s eta 0:00:00
Collecting trio-websocket~=0.9
  Downloading trio_websocket-0.10.2-py3-none-any.whl (17 kB)
Requirement already satisfied: certifi>=2021.10.8 in /private/var/containers/Bundle/Application/C3889491-0
CAD-4C9D-8B01-39773D35A63A/a-Shell.app/Library/lib/python3.11/site-packages (from selenium) (2022.9.24)
Requirement already satisfied: attrs>=19.2.0 in /private/var/containers/Bundle/Application/C3889491-0CAD-4
C9D-8B01-39773D35A63A/a-Shell.app/Library/lib/python3.11/site-packages (from trio~=0.17->selenium) (22.1.0
)
Collecting sortedcontainers
  Downloading sortedcontainers-2.4.0-py2.py3-none-any.whl (29 kB)
Collecting async-generator>=1.9
  Downloading async_generator-1.10-py3-none-any.whl (18 kB)
Requirement already satisfied: idna in /private/var/containers/Bundle/Application/C3889491-0CAD-4C9D-8B01-
39773D35A63A/a-Shell.app/Library/lib/python3.11/site-packages (from trio~=0.17->selenium) (3.4)
Collecting outcome
  Downloading outcome-1.2.0-py2.py3-none-any.whl (9.7 kB)
Requirement already satisfied: sniffio in /private/var/containers/Bundle/Application/C3889491-0CAD-4C9D-8B
01-39773D35A63A/a-Shell.app/Library/lib/python3.11/site-packages (from trio~=0.17->selenium) (1.3.0)
Collecting exceptiongroup
  Downloading exceptiongroup-1.1.1-py3-none-any.whl (14 kB)
Collecting wsproto>=0.14
  Downloading wsproto-1.2.0-py3-none-any.whl (24 kB)
Collecting PySocks!=1.5.7,<2.0,>=1.5.6
  Downloading PySocks-1.7.1-py3-none-any.whl (16 kB)
Collecting h11<1,>=0.9.0
  Downloading h11-0.14.0-py3-none-any.whl (58 kB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 58.3/58.3 kB 2.2 MB/s eta 0:00:00
Installing collected packages: sortedcontainers, PySocks, outcome, h11, exceptiongroup, async-generator, w
sproto, trio, trio-websocket, selenium
Successfully installed PySocks-1.7.1 async-generator-1.10 exceptiongroup-1.1.1 h11-0.14.0 outcome-1.2.0 se
lenium-4.8.3 sortedcontainers-2.4.0 trio-0.22.0 trio-websocket-0.10.2 wsproto-1.2.0
```

### Lua and Perl

Here are examples about Lua and Perl.

```sh
$ lua
Lua 5.4.4  Copyright (C) 1994-2022 Lua.org, PUC-Rio
> print ("Hello, world!")
Hello, world!
```

Perl also works.

```
$ perl test.pl
Hello, world!
```

### Fortran

With `f2c`, Fortran source files can be converted into C/C++.

```sh
pkg install f2c
```

{% hint style="info" %}
TODO: The specific steps to use `f2c` to deal with Fortran programs.
{% endhint %}