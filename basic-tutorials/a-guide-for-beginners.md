# A guide for beginners

You may have already found `a-Shell` on the App Store. `a-Shell` is a terminal emulator for iOS/iPadOS, which allows you to run various Unix commands, from importing the `python.rich` module to managing `vim` plugins. You can use `ffmpeg`, `python`, `lua`, `tex`, `perl`, `clang`, `wasm`, `jsc`, etc, and edit text using `vim` or nano-like `pico`. You can even run `JupyterLab`; also, `code-server` may be researched in the future.

### What you can do

#### Basic commands and net commands

As is expected, basic commands like `ls`, `cd` and `cp` are available of course. Many important net commands have been provided as well.

```sh
$ ping google.com -c 4
PING google.com (198.18.0.85): 56 data bytes
64 bytes from 198.18.0.85: icmp_seq=0 ttl=64 time=0.406 ms
64 bytes from 198.18.0.85: icmp_seq=1 ttl=64 time=0.454 ms
64 bytes from 198.18.0.85: icmp_seq=2 ttl=64 time=0.532 ms
64 bytes from 198.18.0.85: icmp_seq=3 ttl=64 time=0.396 ms
--- google.com ping statistics ---
4 packets transmitted, 4 packets received, 0.0% packet loss
round-trip min/avg/max/stddev = 0.396/0.447/0.532/0.054 ms

$ nslookup
> apple.com
Server:         198.18.0.2
Address:        198.18.0.2#53
Non-authoritative answer:
Name:   apple.com
Address: 198.18.0.37
```

`man` command is also provided, so you can read the manuals of basic commands easily.

<figure><img src=".gitbook/assets/68F0411E-EB70-4EEA-8E82-3119770F7787.jpeg" alt=""><figcaption><p>Manual of make</p></figcaption></figure>

#### Get more packages

A tool called `pkg` can be used to install some extra commands. You can use `pkg install` to get more commands:

```sh
$ pkg install zip
```

Use `pkg list` to list all packages already-installed and `pkg search <package name>` to search if a package is available. To see all available packages, use `pkg search`. To remove a package, use `pkg remove <package name>`.

The variable `$PKG_SERVER` defines the address to get packages. If the variable is not set, the default repository [https://github.c\
om/holzschu/a-Shell-commands](https://github.com/holzschu/a-Shell-commands) would be used. You can set the repository you use by setting the variable:

```sh
$ export PKG_SERVER=https://github.com/holzschu/a-Shell-commands 
```

If you can’t get or search for any package, there may be something wrong with `$PKG_SERVER`. Try to unset it to switch to the default repository:

```sh
$ unsetenv PKG_SERVER
```

#### Edit text files

So far three text editors are provided: `vim`, `pico` and `ed`.

Vim users may be happy to see Vim plugins just work, but plugin managers like `vim-plug` have many problems. Therefore, it’s suggested to use Vim 8’s built-in package manager. See [configure-your-vim.md](basic-tutorials/configure-your-vim.md "mention") for details.

<figure><img src=".gitbook/assets/89BA884C-9395-4E53-9284-97E69E3CE2A9.jpeg" alt=""><figcaption><p>Vim interface</p></figcaption></figure>

If you are not used to Vim and looking for a simpler text editor, `pico` will suit your needs. GNU Nano under GPL can’t be included in a-Shell due to FSF’s policy, so `pico` is included to provide a similar experience.

<figure><img src=".gitbook/assets/D884DB64-276A-46D6-8ED6-789FBD167C1C.jpeg" alt=""><figcaption><p>Pico interface</p></figcaption></figure>

A toy, `ed`, is also included. `ed` is a line editor, which allows one to input editing commands line by line. For the example below, `r`, `,p`, `1`, `2`, `3`, `4` and `q` are commands inside, and others are the outputs by `ed`.

```
$ ed
r test.cpp
131
,p
#include<iostream>
using namespace std;
int main(){
        cout << "Hello, world!" << endl;
        int a;
        cin >> a;
        cout << a;
        return 0;
}
1
#include<iostream>
2
using namespace std;
3
4
int main(){
q
$
```

There are also Web based editors available: Ace, CodeMirror and Monaco. Use `pkg` to get them installed.

#### Running codes

See [Running codes](./running-codes.md) for tips on running Python, Lua and Perl with a-Shell.

#### Remote SSH/SFTP

SSH connecting is available. Just use `ssh`, `scp` and `sftp` as you’ve got used to. Use `ssh-keygen` to generate SSH keys. `mosh` and `sshd` are not supported yet.

#### C/C++ and WebAssembly

Thanks to WebAssembly, it’s not impossible to compile projects written in C/C++. With `clang`, we can compile C codes into WebAssembly, and with `wasm`, we can run them easily. In fact, almost all commands installed by `pkg` are distributed in WebAssembly.
