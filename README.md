# Readme
## Using TypeScript projects with Visual Studio Code on Mac OSX
#### Prerequsite Applications
- Node and NPM (nodejs.org) - using v4.4.0
- Visual Studio Code (code.visualstudio.com) - using v1.3.1

#### Prerequsite Global NPM packages (npm i -g <PACKAGE_NAME>)
- Typescript (typescriptlang.org) - using v1.8.10
```sh
$ npm i -g typescript
$ tsc -version
```
- Typings (www.npmjs.com/package/typings) - using v1.3.2
```sh
$ npm i -g typings
$ typings -version
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
- Hit the green button to run, red square to stop, set breakpoints in the margin, watch variables, etc.

#### Getting TypeScript and Node to play nicely with each other
- Now let's fix the code to allow us to reference type definitions.  When compiling app.ts, Typescript says nothing about the console object but balks at the the process object.  The process object is specific to Node but we need to add a reference a type definition file before the TypeScript compiler will let this pass without any fuss.
- Type the following to fix the errors (NOTE: I never quite figured out why I needed the "env" variant for node !?!?):
```sh
$ typings install env~node --global --save
```

#### Next steps
- You don't yet have a package.json file.  You don't need one because you have no external dependencies yet.  But should you need to install, for example, lodash, try the following:
```sh
$ npm init -f
$ npm install lodash --save
$ typings install lodash --save
```
- Then you can make reference to lodash from Typescript as so
```ts
/// <reference path="./typings/index.d.ts" />
import * as _ from 'lodash';
```
... to be continued
