# Readme

## Using TypeScript projects with Visual Studio Code on Mac OSX
#### Prerequsite Installs
- Node and NPM (nodejs.org)
- Visual Studio Code (code.visualstudio.com) - using v1.3.1 
- Typescript (typescriptlang.org)
```sh
$ npm i -g typescript
$ tsc -version
$ tsc -help
```
#### Creating a new project
- Launch Visual Studio in a new empty directory
```sh
$ mkdir myfirst
$ cd myfirst
$ code .
```
- Inside the VS Code IDE, create a new file named .gitignore and edit as follows
```
js/
node_modules/
typings/
```
- Inside the VS Code IDE, create a new file named app.ts and edit as follows
```ts
/// <reference path="./typings/index.d.ts" />
class Greeter {
    greeting: string;
    constructor(message: string) {
        this.greeting = message;
    }
    greet() {
        return "Hello, " + this.greeting;
    }
}
let greeter = new Greeter("world");
console.log("Process #" + process.pid + " says " + greeter.greet());
```
- Open command pallette in VS Code by typing ``<cmd>+<shift>+P`` and type the word ``task``
- Choose ``Run Build Task`` (or ``<cmd>+<shift>+B``)
- No task is yet configured, so click the ``Configure Task Runner`` button then select ``TypeScript - tsconfig.json`` to create a new file named ``.vscode\tasks.json`` then change it read as follows:
```js
{
    "version": "0.1.0",
    "command": "tsc",
    "isShellCommand": true,
    "args": ["--target", "ES5",
             "--outDir", "js",
             "--sourceMap",
             "--watch",
             "app.ts"],
    "showOutput": "silent",
    "problemMatcher": "$tsc"
}  
```
- Open the Output window by typing ``<cmd>+<shift>+U``
- Now repeat the steps used earlier to invoke the ``Run Build Task`` command
- The files ``js/app.js`` and ``js/app.js.map`` will be created but you'll receive the following errors too, which we'll fix later:
```
app.ts(1,1): error TS6053: File 'typings/index.d.ts' not found.
app.ts(12,27): error TS2304: Cannot find name 'process'.
9:39:57 PM - Compilation complete. Watching for file changes.
```
- In VS Code, switch to DEBUG mode using the circular bug icon in the left-hand sidebar, then click the cog icon at the top of the debug panel to configure ``launch.json``.  This file will automatically open itself.
- Within the configuration denoted by ``"name": "Launch"``, ensure the following settings are adjusted as follows:
```
"program": "${workspaceRoot}/app.ts",
"sourceMaps": true,
"outDir": "${workspaceRoot}/js"

```
- Hit the green button to run, red square to stop, set breakpoints in the margin, watch variables ... the usual.
- Now let's fix the code to allow us to reference type definitions.  Note that app.ts knows about the console object but not the process object.  The process object is specific to Node but we need to reference a type definition file before the TypeScript compiler will let this pass without any fuss.
- 
