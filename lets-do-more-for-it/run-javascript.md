# Run JavaScript

We have a lot to talk about JavaScript. As we know traditionally JavaScript runs both server side (with NodeJS, Deno or Bun) and client side (with a browser). With a-Shell JavaScript also can run, both as a "server" (with `jsc` below) and as a "browser" (with a-Shell's built-in browser view). So the cases may be a bit more complex.

### `jsc`

`jsc` is known as "JSCompiler" (although actually not a real compiler), which is based on Apple's JavaScriptCore, and provides a server-like JS environment. For example:

```js
// hello.js
console.log("Hello, world!")
```

run it with the `jsc` command:

```sh
jsc hello.js
```

and you'll see the output:

```
Hello, world!
```

There is a `jsc` object available with `jsc`, which allows the user to operate file system APIs. A quick view:

```ts
// Read a file at `filePath` and returns it contents (with UTF-8).
jsc.readFile(filePath: string): string
// Read a file at `filePath` and returns it contents encoded with Base64.
jsc.readFileBase64(filePath: string): string
// Write a string to a file (with UTF-8).
jsc.writeFile(filePath: string, content: string): Result
// Write a string encoded with Base64 to a file (with UTF-8).
jsc.writeFileBase64(filePath: string, content: string): Result
// Return a list of files at `filderPath`.
jsc.listFiles(folderPath: string): string[]
// See if there's a file at the path and it's not a folder.
jsc.isFile(filePath: string): boolean
// See if there's a folder at the path and it's not a file.
jsc.isDirectory(folderPath: string): boolean
// `mkdir`.
jsc.makeFolder(folderPath: string): Result
// `rm` or `del`.
jsc.deleteFile(filePath: string): Result
// `mv` from `pathA` to `pathB`.
jsc.move(pathA: string, pathB: string): Result
// `cp` from `pathA` to `pathB`.
jsc.copy(pathA: string, pathB: string): Result
// Run a system command and get its return value (0 for example).
jsc.system(command: string): Result
```

### `internalbrowser`

`internalbrowser` provides a browser-like environment. For example:

```
internalbrowser https://google.com
```

Swipe from left to the right to go back. When there's no web pages back, it'll lead you to the precious terminal.