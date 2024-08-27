# Compile a-Shell yourself

It's needed when you want to add any native things into a-Shell. FIRST YOU NEED A MAC. Without it you can just do NOTHING.

As I have no Mac, it's almost impossible for me to complete this guide, as well as porting anything to a-Shell natively, including things written in C/C++/Go/Rust/anything else.

What should be discussed on this article is:

+ The architecture of a-Shell, the meanings of the directories.
+ Cross-compile the CLI programs into `aarch64-apple-ios` as libraries and let them be included by a-Shell.
+ Deal with the CPython libraries, and itself.
+ Tips on how to select the CLI programs you want and remove the ones you don't want.