# Before the hard part

Everyone reading this chapter of the guide book must be unsatisfied by the functions a-Shell already has, and at this chapter we will talk about various possibilities of what this App can do.

Attention that this road is filled with big challenges. Sometimes you have to learn a number of concepts or search on the Internet again and again just to deal with a confusing error. If you are ready now, start your hacking trip!

### How do programs work?

This is one of the most important problems of this topic. All programs working on a-Shell are divided into two kinds:

* Native binary codes, for `ios` and `arm64`. They work just like what normal Apps work, and theoretically they can do anything. To compile these kinds of codes, you need a Mac computer with Xcode installed, and to distribute it, you must resign the App with an developer account. Updates must be pushed to the App Store and all users have no ability to add new functions or remove them.
* WebAssembly codes, called by `wasm`. They can do simpler works and you can use any computer to compile it (even a-Shell itself). They can be added or removed by users and a simple tool `pkg` is provided to manage `wasm` packages.

Consider do you want to compile your codes to native codes or WebAssembly codes, which is decided by whether the project is complex and whether it’ll be welcomed by the majority of the users.

### The file system of a-Shell

Another important problem. According to the limitations of Apple, most paths are not available for non-jailbroken devices. So a-Shell uses a different file structure. In short:

* `~/Library/` just acts like `/usr`.
* `~/Documents` just acts like `/home/your_account`, commonly known as `~` for a normal computer.

You may have noticed a directory `~/Library/usr/` also exists. It's for special needs of `clang`.

### What would be talked about at this chapter?

This part is in progress and not finally determined.

* To compile a project in C/C++ to WebAssembly files with a-Shell’s own tool chain
* To cross-compile a project with a real computer environment’s tool chain to WebAssembly files, with `make`, `cmake` or other tools
* To submit new packages to a-Shell’s extension repository
* To add new commands to a-Shell itself and compile the project
* Go and WebAssembly
* The possibilities of Rust
* Node.js? JSCompiler?
* Some interesting ideas
