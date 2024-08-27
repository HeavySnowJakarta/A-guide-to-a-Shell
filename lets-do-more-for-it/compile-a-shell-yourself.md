# Compile a-Shell yourself

It's needed when you want to add any native things into a-Shell. FIRST YOU NEED A MAC. Without it you can just do NOTHING.

As I have no Mac, it's almost impossible for me to complete this guide, as well as porting anything to a-Shell natively, including things written in C/C++/Go/Rust/anything else.

What should be discussed on this article is:

+ The architecture of a-Shell, the meanings of the directories.
+ Cross-compile the CLI programs into `aarch64-apple-ios` as libraries and let them be included by a-Shell.
+ Deal with the CPython libraries, and itself.
+ Tips on how to select the CLI programs you want and remove the ones you don't want.

### How to add a command

According to [Holzschu's comment](https://github.com/holzschu/a-shell/issues/151#issuecomment-752739203):

> Hi, these are very good questions. a-Shell does not have any deep dependency to python, python is just treated like one of the many commands available.
> To compile a command, you will need to:
>
> + download the `ios_system.framework` (use the binary release at https://github.com/holzschu/ios_system)
> + compile the command for the iOS architecture: `CC = clang`, `CFLAGS="-arch arm64 -miphoneos-version-min=14.0  -isysroot /Applications/Xcode.app/Contents/Developer/Platforms/iPhoneOS.platform/Developer/SDKs/iPhoneOS14.3.sdk`, `LDFLAGS` is like `CFLAGS`, plus `-F ...Frameworks -framework ios_system`.
> + (once it compiles) make the system create a dynamic library rather than an executable
> + embed that dynamic library inside the executable ("Embed Frameworks", in "Build Phases")
> + add the command to `Resources/commandDictionary.plist`. It's an array of arrays, indexed by the name of the command, with 4 entries for each command:
>   + name of dynamic library
>   +  function to be called inside the library (usually main)
>   +  options (the string that is passed to get_options, can be left empty)
>   +  whether the command operates on files, directories or not (file for a compiler).
> + (once all this is done) start the command and observe what happens, debug any crash.
> + (once it is done) edit the source files for:
>   + output to stdout/stderr should be replaced by output to thread_stout/thread_stderr
>   + reading from stdin should be replaced by reading from thread_stdin
>   + make sure all memory is released when leaving the program
>   + make sure all variables are initialized when we restart the program
>   + (these two are related to a fundamental issue: we never actually exit from a program on iOS, so the system will never reclaim the memory we allocated and reinitialize the variables. It is up to us to do the cleaning).
>   + lines using `std::cout` will have to be replaced by `fprintf(thread_stdout, ...)`, I haven't yet found a way to redirect C++ standard streams.
> + (once all this is done): for AppStore release, convert the dynamic library into a framework (see examples in https://github.com/holzschu/python-aux/)
> + (once all this is done): submit a pull request, if you want the command to become part of the main application. I see your idea about having a third, separate app for a-Shell, targetting rust more than python and TeX.
>
> The first step can be difficult if the package or command insists on guessing the compiler and flags rather than letting you set them.
> If your command is simple and fast (so, not rust or cargo) you can also compile it to webAssembly, which makes things a lot easier.
> If the command uses `fork` and `exec`, you will need to edit the source code a bit more, as `fork()` is a no-op in iOS. `iOS_system` has a fake fork, but we go through both branches in the same thread.

Notice I've never tried things above before.