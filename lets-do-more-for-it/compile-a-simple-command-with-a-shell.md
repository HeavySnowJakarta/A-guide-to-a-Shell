# Compile a simple command with a-Shell

This article focus on compiling a small file or project written in C/C++ with a-Shell’s own tool chain. Due to Apple’s limitation, they can only be compiled to WebAssembly instead of native codes, so don’t dream for `emacs` or `fish`!

### Meet clang, clang++ and wasm

Let’s start from compiling a single program file. Here are two examples:

```c
// test.c
#include<stdio.h>

int main(){
    printf("Hello, world!\n");
    return 0;
}
```

```cpp
// test.cpp
#include<iostream>
using namespace std;

int main(){
    cout << "Hello, world!" << endl;
    return 0;
}
```

To compile, we use `clang` and `clang++` respectively. We use `-o` to set the name of the target file, and they can end with `.wasm` or not.

```
$ clang test.c -o testc
$ clang++ test.cpp -o testcpp.wasm
```

Then run the compiled files with `wasm`. You can either call `wasm` to run it or execute it directly like binary codes.

```
$ ./testc
Hello, world!
$ wasm testcpp.wasm
Hello, world!
```

You may receive a message `wasm: Error:` sometimes. Try to close all the open windows and retry.

### Meet make

For big projects, it’ll be a difficult work to input commands to compile all files line by line. Usually, `make` is used to do it automatically. `make` seeks for the makefile of the project and do as it directs when it works. You may have known a famous way to compile and install a project from source codes:

```
$ ./configure
$ make
$ make install
```

For the example above, `./configure` generates the makefile according to your platform, `make` compiles the project according to the makefile, and `make install` installs it to the computer. However, sometimes `./configure` really doesn’t know the difference of a-Shell and WebAssembly so that it may do the wrong work and mess it up.

Now let’s see a simple project: `unrar`. It’s simple enough that there is no script like `configure` to generate makefiles. First of all get the whole source code, and here are parts of the short makefile:

```makefile
# Linux using GCC
CXX=c++
CXXFLAGS=-O2 -Wno-logical-op-parentheses -Wno-switch -Wno-dangling-else
LIBFLAGS=-fPIC
DEFINES=-D_FILE_OFFSET_BITS=64 -D_LARGEFILE_SOURCE -DRAR_SMP
STRIP=strip
AR=ar
LDFLAGS=-pthread
DESTDIR=/usr
```

```makefile
COMPILE=$(CXX) $(CPPFLAGS) $(CXXFLAGS) $(DEFINES)
LINK=$(CXX)

WHAT=UNRAR

UNRAR_OBJ=filestr.o recvol.o rs.o scantree.o qopen.o
LIB_OBJ=filestr.o scantree.o dll.o qopen.o

OBJECTS=rar.o strlist.o strfn.o pathfn.o smallfn.o global.o file.o filefn.o filcreat.o \
	archive.o arcread.o unicode.o system.o crypt.o crc.o rawread.o encname.o \
	resource.o match.o timefn.o rdwrfn.o consio.o options.o errhnd.o rarvm.o secpassword.o \
	rijndael.o getbits.o sha1.o sha256.o blake2s.o hash.o extinfo.o extract.o volume.o \
	list.o find.o unpack.o headers.o threadpool.o rs16.o cmddata.o ui.o

.cpp.o:
	$(COMPILE) -D$(WHAT) -c $<
```

Now we’ll revise the makefile to let it suit our tool chain. Before that, we need to know:

* `CXX` means the compiler being used. It’ll be `clang++` for a-Shell.
* `CXXFLAGS` and `DEFINES` means the options of the compiler.
* `-O2` means O2 optimization, working with `clang++`.
* `-Wno-logical-op-parentheses -Wno-switch -Wno-dangling-else` will be not needed.
* `-D_FILE_OFFSET_BITS=64 -D_LARGEFILE_SOURCE -DRAR_SMP` is for 32-bit systems to deal with large files. For a 64-bit system, it‘s useless but harmless.
* `/usr` won’t be accessible on a-Shell. Actually `~/Library` acts as `/usr` so we’ll use it instead.
