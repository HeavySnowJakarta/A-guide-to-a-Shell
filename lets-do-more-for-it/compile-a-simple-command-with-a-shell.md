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

### Build a project

With `make`, we can build projects with a-Shell itself. Let’s try a small project written in C now.
