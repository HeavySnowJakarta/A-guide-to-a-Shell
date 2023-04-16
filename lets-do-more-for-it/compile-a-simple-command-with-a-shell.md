# Compile a simple command with a-Shell

Everyone reading this part of the guide book must be not satisfied by the functions a-Shell already has, and at this chapter we will talk about various possibilities of what this App can do.

This article focus on compiling a small file or project written in C/C++ with a-Shell’s own tool chain. Due to Apple’s limitation, they can only be compiled to WebAssembly instead of native codes, so don’t dream for `emacs` or `fish`!

### Meet clang, clang++ and wasm

Let’s start from compiling a single program file. Here are two examples:

```c
// test.c
#include<stdio.h>

int main(){
    printf("Hello, world!");
    return 0;
}
```

```cpp
// test.cpp
#include<iostream>
using namespace std;

int main(){
    cout << "Hello, world!";
    return 0;
}
```

To compile, we use `clang` and `clang++` respectively.

```
$ clang test.c -o testc
$ clang++ test.cpp -o testcpp.wasm
```
