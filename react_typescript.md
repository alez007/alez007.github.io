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
npm init
```
For the purpose of this project, we need to create two folders, one called `public` and the other `src`. 
```$bash
mkdir public && mkdir src
```
Folder `public`
will contain static assets like css files and images, plus any html files that are being served to the user, e.g. 
`index.html`. Folder `src` will be our working folder, we will write code here and every file created here will be 
processed in one way or another to generate the project's final state.

At this point, we should make sure we configure our module bundler. Webpack uses `loaders` as helpers to do its job. 
We will install a few of them and I will explain what each of them does:
```
npm install --save-dev webpack webpack-cli typescript webpack-dev-server style-loader css-loader sass-loader ts-loader
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
 
 What else do we want ? We want React and Typescript:
```
npm install --save react react-dom @types/react @types/react-dom 
```

 - `react` is the core React library
 - `react-dom` allows us to hook react into our DOM and contains methods like `render()`
 - types packages are in fact declaration files; Typescript uses those to understand the structure of a given library 
 codebase, in this case the react packages; usually their extensions are '.d.ts' and they also help in not misusing or 
 misunderstanding libraries and IDE auto completion; as a general rule, if you're using a certain npm package and that 
 package doesn't have the declarations already, `@types/package-name` should be the other thing you need
 
 At this point I think we've got all we need. Let's configure our project.
 
 ### Project configuration
 
 I'm going to split this section in 3 parts: Typescript configuration, Webpack configuration and NPM configuration.
 
 #### Typescript configuration
 



