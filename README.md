# setup-typescript-nodejs-project
In this guide, we walk through the process of creating a TypeScript project from scratch with cold-reloading, and scripts for building, development, and production environments.


## Initial Setup
Let's create a folder for us to work in.

<code> mkdir typescript-starter</code>

<code> cd typescript-starter </code>


Next, we'll setup the project package.json and add the dependencies.

## Setup Node.js package.json
Using the -y flag when creating a package.json will simply approve all the defaults.

npm init -y

## Add TypeScript as a dev dependency
This probably doesn't come as a surprise ;)

<code> npm install typescript --save-dev </code>

After we install typescript, we get access to the command line TypeScript compiler through the tsc command. More on that below.

## Install ambient Node.js types for TypeScript

TypeScript has Implicit, Explicit, and Ambient types. Ambient types are types that get added to the global execution scope. Since we're using Node, it would be good if we could get type safety and auto-completion on the Node apis like file, path, process, etc. That's what installing the DefinitelyTyped type definition for Node will do.

<code>npm install @types/node --save-dev</code>

## Create a tsconfig.json.
The tsconfig.json is where we define the TypeScript compiler options. We can create a tsconfig with several options set.

<code> npx tsc --init --rootDir src --outDir build \
--esModuleInterop --resolveJsonModule --lib es6 \
--module commonjs --allowJs true --noImplicitAny true </code>

<ul>
<li>rootDir: This is where TypeScript looks for our code. We've configured it to look in the src/ folder. That's where we'll write our TypeScript.</li>
  <li>outDir: Where TypeScript puts our compiled code. We want it to go to a build/ folder.</li>
<li>esModuleInterop: If you were in the JavaScript space over the past couple of years, you might have recognized that modules systems had gotten a little bit out of control (AMD, SystemJS, ES Modules, etc). For a topic that requires a much longer discussion, if we're using commonjs as our module system (for Node apps, you should be), then we need this to be set to true.</li>
  <li>resolveJsonModule: If we use JSON in this project, this option allows TypeScript to use it.</li>
<li>lib: This option adds ambient types to our project, allowing us to rely on features from different Ecmascript versions, testing libraries, and even the browser DOM api. We'd like to utilize some es6 language features. This all gets compiled down to es5.</li>
  <li>module: commonjs is the standard Node module system in 2019. Let's use that.</li>
<li>allowJs: If you're converting an old JavaScript project to TypeScript, this option will allow you to include .js files among .ts ones.</li>
<li>noImplicitAny: In TypeScript files, don't allow a type to be unexplicitly specified. Every type needs to either have a specific type or be explicitly declared any. No implicit anys.</li>
  </ul>
At this point, you should have a tsconfig.json that looks like this:

<code>
  
{
  "compilerOptions": {
    /* Basic Options */
    // "incremental": true,                   /* Enable incremental compilation */
    "target": "es5",                          /* Specify ECMAScript target version: 'ES3' (default), 'ES5', 'ES2015', 'ES2016', 'ES2017', 'ES2018', 'ES2019' or 'ESNEXT'. */
    "module": "commonjs",                     /* Specify module code generation: 'none', 'commonjs', 'amd', 'system', 'umd', 'es2015', or 'ESNext'. */
    "lib": ["es6"],                     /* Specify library files to be included in the compilation. */
    "allowJs": true,                          /* Allow javascript files to be compiled. */
    // "checkJs": true,                       /* Report errors in .js files. */
    // "jsx": "preserve",                     /* Specify JSX code generation: 'preserve', 'react-native', or 'react'. */
    // "declaration": true,                   /* Generates corresponding '.d.ts' file. */
    // "declarationMap": true,                /* Generates a sourcemap for each corresponding '.d.ts' file. */
    // "sourceMap": true,                     /* Generates corresponding '.map' file. */
    // "outFile": "./",                       /* Concatenate and emit output to single file. */
    "outDir": "build",                          /* Redirect output structure to the directory. */
    "rootDir": "src",                         /* Specify the root directory of input files. Use to control the output directory structure with --outDir. */
    // "composite": true,                     /* Enable project compilation */
    // "tsBuildInfoFile": "./",               /* Specify file to store incremental compilation information */
    // "removeComments": true,                /* Do not emit comments to output. */
    // "noEmit": true,                        /* Do not emit outputs. */
    // "importHelpers": true,                 /* Import emit helpers from 'tslib'. */
    // "downlevelIteration": true,            /* Provide full support for iterables in 'for-of', spread, and destructuring when targeting 'ES5' or 'ES3'. */
    // "isolatedModules": true,               /* Transpile each file as a separate module (similar to 'ts.transpileModule'). */

    /* Strict Type-Checking Options */
    "strict": true,                           /* Enable all strict type-checking options. */
    "noImplicitAny": true,                    /* Raise error on expressions and declarations with an implied 'any' type. */
    // "strictNullChecks": true,              /* Enable strict null checks. */
    // "strictFunctionTypes": true,           /* Enable strict checking of function types. */
    // "strictBindCallApply": true,           /* Enable strict 'bind', 'call', and 'apply' methods on functions. */
    // "strictPropertyInitialization": true,  /* Enable strict checking of property initialization in classes. */
    // "noImplicitThis": true,                /* Raise error on 'this' expressions with an implied 'any' type. */
    // "alwaysStrict": true,                  /* Parse in strict mode and emit "use strict" for each source file. */

    /* Additional Checks */
    // "noUnusedLocals": true,                /* Report errors on unused locals. */
    // "noUnusedParameters": true,            /* Report errors on unused parameters. */
    // "noImplicitReturns": true,             /* Report error when not all code paths in function return a value. */
    // "noFallthroughCasesInSwitch": true,    /* Report errors for fallthrough cases in switch statement. */

    /* Module Resolution Options */
    // "moduleResolution": "node",            /* Specify module resolution strategy: 'node' (Node.js) or 'classic' (TypeScript pre-1.6). */
    // "baseUrl": "./",                       /* Base directory to resolve non-absolute module names. */
    // "paths": {},                           /* A series of entries which re-map imports to lookup locations relative to the 'baseUrl'. */
    // "rootDirs": [],                        /* List of root folders whose combined content represents the structure of the project at runtime. */
    // "typeRoots": [],                       /* List of folders to include type definitions from. */
    // "types": [],                           /* Type declaration files to be included in compilation. */
    // "allowSyntheticDefaultImports": true,  /* Allow default imports from modules with no default export. This does not affect code emit, just typechecking. */
    "esModuleInterop": true,                  /* Enables emit interoperability between CommonJS and ES Modules via creation of namespace objects for all imports. Implies 'allowSyntheticDefaultImports'. */
    // "preserveSymlinks": true,              /* Do not resolve the real path of symlinks. */
    // "allowUmdGlobalAccess": true,          /* Allow accessing UMD globals from modules. */

    /* Source Map Options */
    // "sourceRoot": "",                      /* Specify the location where debugger should locate TypeScript files instead of source locations. */
    // "mapRoot": "",                         /* Specify the location where debugger should locate map files instead of generated locations. */
    // "inlineSourceMap": true,               /* Emit a single file with source maps instead of having a separate file. */
    // "inlineSources": true,                 /* Emit the source alongside the sourcemaps within a single file; requires '--inlineSourceMap' or '--sourceMap' to be set. */

    /* Experimental Options */
    // "experimentalDecorators": true,        /* Enables experimental support for ES7 decorators. */
    // "emitDecoratorMetadata": true,         /* Enables experimental support for emitting type metadata for decorators. */

    /* Advanced Options */
    "resolveJsonModule": true                 /* Include modules imported with '.json' extension */
  }
}
  
  </code>
  
### We can go ahead and clean the commented out stuff that we don't need. Our tsconfig.json should look like this:

{
  "compilerOptions": {
    "target": "es5",                          
    "module": "commonjs",                    
    "lib": ["es6"],                     
    "allowJs": true,
    "outDir": "build",                          
    "rootDir": "src",
    "strict": true,         
    "noImplicitAny": true,
    "esModuleInterop": true,
    "resolveJsonModule": true
  }
}
We're set to run our first TypeScript file.

## Create the src folder and create our first TypeScript file
<code>mkdir src
touch src/index.ts</code>
And let's write some code.

<code>console.log('Hello world!')</code>

## Compiling our TypeScript
To compile our code, we'll need to run the tsc command using npx, the Node package executer. tsc will read the tsconfig.json in the current directory, and apply the configuration against the TypeScript compiler to generate the compiled JavaScript code.

<code>npx tsc</code>
## Our compiled code
Check out build/index.js, we've compiled our first TypeScript file.

<code>"use strict";
console.log('Hello world!');</code>

## Useful configurations & scripts
#### Cold reloading
Cold reloading is nice for local development. In order to do this, we'll need to rely on a couple more packages: ts-node for running TypeScript code directly without having to wait for it be compiled, and nodemon, to watch for changes to our code and automatically restart when a file is changed.

<code>npm install --save-dev ts-node nodemon</code>
Add a nodemon.json config.

<code>{
  "watch": ["src"],
  "ext": ".ts,.js",
  "ignore": [],
  "exec": "ts-node ./src/index.ts"
}</code>

And then to run the project, all we have to do is run nodemon. Let's add a script for that.

<code>"start:dev": "nodemon"</code>
By running npm run start:dev, nodemon will start our app using ts-node ./src/index.ts, watching for changes to .ts and .js files from within /src.

### Creating production builds
In order to clean and compile the project for production, we can add a build script.

Install rimraf, a cross-platform tool that acts like the rm -rf command (just obliterates whatever you tell it to).

<code>npm install --save-dev rimraf</code>

And then, add this to your package.json.

<code>"build": "rimraf ./build && tsc"</code>

Now, when we run npm run build, rimraf will remove our old build folder before the TypeScript compiler emits new code to dist.

### Production startup script
In order to start the app in production, all we need to do is run the build command first, and then execute the compiled JavaScript at build/index.js.

The startup script looks like this.

<code>"start": "npm run build && node build/index.js"</code>

I told you it was simple! Now, off you go. Create great things, my friend.

### Scripts overview

<code>npm run start:dev</code>
Starts the application in development using nodemon and ts-node to do cold reloading.

<code>npm run build</code>
Builds the app at build, cleaning the folder first.

<code>npm run start</code>
Starts the app in production by first building the project with npm run build, and then executing the compiled JavaScript at build/index.js.
