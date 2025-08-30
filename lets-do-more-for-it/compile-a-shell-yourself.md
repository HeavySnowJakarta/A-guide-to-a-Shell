# Compile a-Shell yourself

It's needed when you want to add any native things into a-Shell. FIRST YOU NEED A MAC. Without it you can just do NOTHING.

What should be discussed on this article is:

* The architecture of a-Shell, the meanings of the directories.
* Cross-compile the CLI programs into `aarch64-apple-ios` as libraries and let them be included by a-Shell.
* Deal with the CPython libraries, and itself.
* Tips on how to select the CLI programs you want and remove the ones you don't want.

### How to add a command

#### General view

According to [Holzschu's comment](https://github.com/holzschu/a-shell/issues/151#issuecomment-752739203):

> Hi, these are very good questions. a-Shell does not have any deep dependency to python, python is just treated like one of the many commands available. To compile a command, you will need to:
>
> * download the `ios_system.framework` (use the binary release at https://github.com/holzschu/ios\_system)
> * compile the command for the iOS architecture: `CC = clang`, `CFLAGS="-arch arm64 -miphoneos-version-min=14.0 -isysroot /Applications/Xcode.app/Contents/Developer/Platforms/iPhoneOS.platform/Developer/SDKs/iPhoneOS14.3.sdk`, `LDFLAGS` is like `CFLAGS`, plus `-F ...Frameworks -framework ios_system`.
> * (once it compiles) make the system create a dynamic library rather than an executable
> * embed that dynamic library inside the executable ("Embed Frameworks", in "Build Phases")
> * add the command to `Resources/commandDictionary.plist`. It's an array of arrays, indexed by the name of the command, with 4 entries for each command:
>   * name of dynamic library
>   * function to be called inside the library (usually main)
>   * options (the string that is passed to get\_options, can be left empty)
>   * whether the command operates on files, directories or not (file for a compiler).
> * (once all this is done) start the command and observe what happens, debug any crash.
> * (once it is done) edit the source files for:
>   * output to stdout/stderr should be replaced by output to thread\_stout/thread\_stderr
>   * reading from stdin should be replaced by reading from thread\_stdin
>   * make sure all memory is released when leaving the program
>   * make sure all variables are initialized when we restart the program
>   * (these two are related to a fundamental issue: we never actually exit from a program on iOS, so the system will never reclaim the memory we allocated and reinitialize the variables. It is up to us to do the cleaning).
>   * lines using `std::cout` will have to be replaced by `fprintf(thread_stdout, ...)`, I haven't yet found a way to redirect C++ standard streams.
> * (once all this is done): for AppStore release, convert the dynamic library into a framework (see examples in https://github.com/holzschu/python-aux/)
> * (once all this is done): submit a pull request, if you want the command to become part of the main application. I see your idea about having a third, separate app for a-Shell, targetting rust more than python and TeX.
>
> The first step can be difficult if the package or command insists on guessing the compiler and flags rather than letting you set them. If your command is simple and fast (so, not rust or cargo) you can also compile it to webAssembly, which makes things a lot easier. If the command uses `fork` and `exec`, you will need to edit the source code a bit more, as `fork()` is a no-op in iOS. `iOS_system` has a fake fork, but we go through both branches in the same thread.

#### Configure the environment

Generally it's recommended to install Xcode from App Store to gain the full abilities to build an iOS application. Set up command-line tools:

```sh
sudo sh -c 'xcode-select -s /Applications/Xcode.app/Contents/Developer && xcodebuild -runFirstLaunch
```

Notice if you've installed Xcode into another location, clarify it by replacing `/Application/Xcode.app` with where it is.

Then read and agree with the Xcode licenses:

```sh
sudo xcodebuild -license
```

Download the SDK support for iOS platform and iOS simulator runtimes.

```
xcodebuild -downloadPlatform iOS
```

If you find you must run x86 programs with an Apple silicon Mac, you may have to install Rosetta:

```sh
sudo softwareupdate --install-rosetta --agree-to-license
```

Also installing CocoaPods by [following the official guides](https://guides.cocoapods.org/using/getting-started.html#installation) may helps manage dependencies.

Now you should be able to see you can build iOS applications like this:

```
% xcrun --sdk iphoneos --show-sdk-path
/Applications/Xcode.app/Contents/Developer/Platforms/iPhoneOS.platform/Developer/SDKs/iPhoneOS18.5.sdk
```

#### Get the XcFramework of ios\_system

Follow [the README](https://github.com/holzschu/ios_system?tab=readme-ov-file#installation) of `ios_system`, you can either download the binaries from [the releases](https://github.com/holzschu/ios_system/releases), or build it yourself. Check out the directory of the Git repository and **fetch the submodules**:

```sh
git clone https://github.com/holzschu/ios_system.git
cd ios_system
git submodule update --init --recursive
```

Build all XcFrameworks:

```sh
swift run --package-path xcfs build
```

When downloading dependencies and building work fine, you'll find the frameworks at the `.build` directory. Unzip the needed `.zip` files like `ios_system.xcframework.zip` to where you can find them. For example, when you put the extracted files as following to `~/ios_system.xcframework`:

```
Info.plist
ios-arm64_x86_64-maccatalyst
ios-arm64
ios-arm64_x86_64-simulator
```

You may set the `-f` argument like `-f ~/ios_system.xcframework/ios-arm64`, which will be mentioned later.

#### Configure the flags

First get where your iOS SDK is stored at:

```
% xcrun --sdk iphoneos --show-sdk-path
/Applications/Xcode.app/Contents/Developer/Platforms/iPhoneOS.platform/Developer/SDKs/iPhoneOS18.5.sdk
```

Then set the flags of Clang:

* `CC = clang`
* `CFLAGS="-arch arm64 -miphoneos-version-min=14.0 -isysroot <the path to your iOS SDK>"`
* `LDFLAGS="-arch arm64 -miphoneos-version-min=14.0 -isysroot <the path to your iOS SDK> -F <the path mentioned above> -framework ios_system"`

#### Compile Zig

Zig is known for how easy it's to cross-compile. When compile a command to a-Shell with Zig, you may pay attention by setting the flags like this:

`zig build-lib <the entry point> -dynamic -OReleaseFast -target aarch64-ios --sysroot <the path to your iOS SDK> -lc -framework ios_system (and/or other XcFrameworks provided by ios_system) -F <the path of ios_system XcFramework mentioned above> -DreleaseSafe`

You may also want to include XcFrameworks provided by iOS SDK like `Foundation` or `JavaScriptCore`. Add the flags before `-DreleaseSafe`:

`-framework Foundation -framework JavaScriptCore -F <the path to your iOS SDK>/System/Library/Frameworks`

{% hint style="info" %}
To be continued...
{% endhint %}
