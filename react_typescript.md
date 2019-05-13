---
layout: default
comments: true
---

## React with Typescript from scratch

Whenever I start a new project, I owe to myself to learn something new from it, especially if that project is an easy 
one. 

This time I will describe step by step how to start a react application using Webpack and Typescript. We're 
going to learn together.

Let's get started!

### First, a bit of history
Javascript was introduced by Netscape Communications in 1995 as a weakly typed scripting language, with similar syntax
with Java. Reason behind this decision was the addition of more features to the already existing HTML, something that'll 
make it more reactive. Its purpose was intended to complement HTML and satisfy some immediate needs like easiness of use
by web designers.

In 1996 Netscape Communications submitted JavaScript to 
[ECMA International](https://en.wikipedia.org/wiki/Ecma_International){:target="_blank"} to create a standard specification
for browsers to adopt and this led to ECMAScript as a language specification one year later.

Fast forward 20 years and big browsers now support version 5 of ECMAScript, ES5. Although ES continues to improve (ES9 
in 2019), browsers are not always that fast in adopting new features and usually the aim of developers is to make sure 
they end up having ES5 code when the project is done. 

### Why TypeScript ?
TypeScript aims to be a strongly typed JavaScript. Its main aim is to instruct mature IDE's to flag common errors in 
the code and therefore help with debugging and development process. You can see the introductory video from 2012 
[here](https://channel9.msdn.com/posts/Anders-Hejlsberg-Introducing-TypeScript){:target="_blank"}. Compile-time type 
checking is something that is still very useful in large projects. 

### What is Webpack ?
[Webpack](https://webpack.js.org/){:target="_blank"} is a 'static module bundler for modern JavaScript applications'. 
Very simply put, it does two things: 
 1. it builds a dependency map based on an entry point, in this case `index.js`
 2. based on that, it packs the necessary code in one bundle, in this case `bundle.js`

### Project initialisation
Make sure you've got [npm](https://www.npmjs.com/get-npm){:target="_blank"} installed, then go ahead and create a 
folder and inside it run:
```$bash
npm init && mkdir src
```
This will initialise our npm project and also create a folder `src` that will contain all our source code. 
Folder `src` will be our working folder, we will write code here and every file created here will be 
processed in one way or another to generate the project's final state.

At this point, we should make sure we configure our module bundler. Webpack uses `loaders` as helpers to do its job. 
We will install a few of them and I will explain what each of them does:
```
npm install --save-dev webpack webpack-cli typescript webpack-dev-server style-loader css-loader sass-loader ts-loader html-loader clean-webpack-plugin html-webpack-plugin
```
- `webpack` is the Webpack core library
- `webpack-cli` is the cli executable for Webpack
- `typescript` is the Typescript core library
- [webpack-dev-server](https://webpack.js.org/configuration/dev-server/){:target="_blank"} is going to be used in our 
 local development, it fires up a local web server with hot reload that automatically refreshes the page once we modify 
 something in our code
- `style-loader css-loader sass-loader` are style loaders, the first one knows how to inject <style> tags in the DOM,
 second one allows us to import css files in the code like we do with any other JS file, the third one is like the second 
 one but with sass / scss files
- `ts-loader` allows Webpack to understand and map / pack .ts or .tsx files written with Typescript
- `html-loader` allows Webpack to manipulate .html files
- `clean-webpack-plugin` it's a Webpack plugin that will allow Webpack to clean the build folder before any subsequent build
- `html-webpack-plugin` is a nice Webpack plugin that will clone out index.html file from ./src folder and put it inside
the distribution folder after each build
 
I think we've got all we need for now. Let's configure our project.
 
### Project configuration
 
I'm going to split this section in 3 parts: Typescript configuration, Webpack configuration and NPM configuration.
 
#### Typescript configuration
Typescript reads its configuration from a file called `tsconfig.json`. Below is a simple config, enough to start a project,
but if you're curious you can see all options [here](https://www.typescriptlang.org/docs/handbook/compiler-options.html){:target="_blank"}.
```json
{
  "compilerOptions": {
    "outDir": "./dist/",
    "module": "es6",
    "target": "es5",
    "jsx": "react",
    "strict": true,
    "allowSyntheticDefaultImports": true
  },
  "include": [
    "./src/**/*"
  ],
  "exclude": [
    "./node_modules"
  ]
}
```
In big lines, the configuration file tells TypeScript the following: 
* everything that gets compiled, needs to be saved in 'outDir'
* expect modules to be written using ES6
* target compilation output to be compatible with ES5
* expect react templating system in .tsx files ([find out more](https://www.typescriptlang.org/docs/handbook/jsx.html){:target="_blank"})
* enable all strict type checking options in the code
* compile all that it finds in ./src folder
* exclude ./node_modules from compilation

#### Webpack configuration
Webpack configuration file is `webpack.config.js`, let's create this file and add the following code:
```javascript
module.exports =(env) => {
 return require(`./webpack.${env}.js`)
}
```
What the above code does is read the --env cli parameter from webpack and require the necessary files based on it.
For example, if we run `webpack --env dev` then it'll include `webpack.dev.js` configuration file. Reason we do this is,
based on the environment we run the project on, we will have different bundling configuration files that will instruct 
webpack to do different things.

Now, you might think that some things from one config file, might also be needed into the other. How do we respect the
DRY principle in this case ? Well, for this, we will use a package called `webpack-merge`. Let's install it first:
```
npm install --save-dev webpack-merge
```
Now, we will create a file called `webpack.common.js` which will include common configuration options that will be applied
throughout all other environment configurations, then we'll make sure we merge this file inside every environment
configuration file.

Here's what we have in `webpack.common.js`:
```javascript
const path = require("path");
const CleanWebpackPlugin = require('clean-webpack-plugin');
const HtmlWebpackPlugin = require('html-webpack-plugin');

module.exports = {
    entry: "./src/index.tsx",
    module: {
        rules: [
            {
                test: /\.(ts|tsx)$/,
                use: 'ts-loader',
                exclude: /(node_modules)/
            },
            {
                test:/\.(s*)css$/,
                use:['style-loader','css-loader', 'sass-loader']
            },
            {
                test: /\.html$/,
                use: 'html-loader'
            }
        ]
    },
    resolve: { extensions: [".tsx", ".ts", ".js", ".jsx"] },
    output: {
        path: path.resolve(__dirname, "dist/"),
        publicPath: "/",
        filename: "bundle.js"
    },
    plugins: [
        new CleanWebpackPlugin(),
        new HtmlWebpackPlugin({
            filename: 'index.html',
            template: 'src/index.html'
        })        
    ]
};
```
In big lines, it does the following:
* reads `index.tsx` as an entry script to build the dependency map
* uses `ts-loader` for transpiling (some kind of a fancy word for source-to-source compiling) all ts and tsx files
* uses `style-loader, css-loader, sass-loader` for css, scss imports
* uses `html-loade` for html files manipulation
* outputs the bundle in `/dist/bundle.js` file that will be included in our `src/index.html`

In `webpack.dev.js` we merge the common configuration and add some other configurations needed only for DEV:
```javascript
const merge = require('webpack-merge');
const common = require('./webpack.common.js');
const webpack = require("webpack");
const path = require("path");

module.exports = merge(common, {
    mode: "development",
    devServer: {
        contentBase: path.join(__dirname, "dist/"),
        port: 3000,
        publicPath: "http://localhost:3000/",
        hot: true
    },
    plugins: [
        new webpack.HotModuleReplacementPlugin()
    ]
});
```
In addition to what common config does, this one only configures the webpack-dev-server to find `index.html` inside
`dist` folder and hot reload every change it detects in the code files.

The `webpack.production.js` configuration file looks like this:
```javascript
const merge = require('webpack-merge');
const common = require('./webpack.common.js');

module.exports = merge(common, {
    mode: "production"
});
```
Ok, I think we're done with Webpack, let's move on to the last bit, NPM configuration.

#### NPM Configuration
The hard part is over, we only need to create a few scripts in package.json so we can easily start a webpack dev server,
build a dev environment and a production one. Under the "scripts" key in your package.json, let's add the following lines:
```json
"scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "start": "webpack-dev-server --mode development --env dev",
    "build:dev": "webpack --env dev",
    "build:prod": "webpack --env production"
}
```
And there we have it, three commands to do the following:
* `npm start` will start our working dev server that will be served on http://localhost:3000
* `npm run build:dev` will build a fully working development project inside `dist` folder
* `bpm run build:prod` will build a fully working production code inside the `dist` folder 

### Did I mention React ? 
And we still haven't written any piece of code, have we ? Let's install React:
```
npm install --save react react-dom @types/react @types/react-dom 
```
- `react` is the core React library
- `react-dom` allows us to hook react into our DOM and contains methods like `render()`
- `@types` packages are in fact declaration files; Typescript uses those to understand the structure of a given library 
 codebase, in this case the react packages; usually their extensions are '.d.ts' and they also help in not misusing or 
 misunderstanding libraries and IDE auto completion; as a general rule, if you're using a certain npm package and that 
 package doesn't have the declarations already, `@types/package-name` should be the other thing you need

We will work in the `src` folder, therefore let's create a simple application there:
* under `src/index.html`, let's add this code:

```html
<!DOCTYPE html>
<html>

<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no">
  <title>React Starter</title>
</head>

<body>
<div id="root"></div>
<noscript>
  You need to enable JavaScript to run this app.
</noscript>
</body>
```
* under `src/index.tsx`, let's add:

```typescript
import React from "react";
import ReactDOM from "react-dom";

import App from "./App";
ReactDOM.render(<App />, document.getElementById("root"));
```
* under `src/App.tsx`, let's add:

```typescript
import React, { Component} from "react";

class App extends Component{
    render(){
        return(
            <div className="App">
                <h1> Yo! </h1>
            </div>
        );
    }
}

export default App;
```

Fire it up with `npm start`, go to `http://localhost:3000` and if you don't see your app on your page then you're lying 
to me :stuck_out_tongue_winking_eye:.

### Conclusions
Ok, I know, this has been a long one, but hey, at least now we're one step closer towards understanding all that magic
behind projects like create-react-app and the like. Now, we could improve this, of course. For one, we miss uglifier for 
our js code, we haven't used minifiers for our css code, but I will let you check it out in [Webpack Guides](https://webpack.js.org/guides){:target="_blank"}.

There is though one aspect that we should discuss, it's the elephant called [Babel](https://babeljs.io/){:target="_blank"}.  
