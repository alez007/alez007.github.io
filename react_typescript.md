---
layout: default
comments: true
---

## React with Typescript from scratch

Whenever I start a new project, I owe to myself to learn something new from it, especially if that project is an easy 
one. 

This time I will describe step by step how to start a react application using Webpack, Babel and Typescript. We're 
going to learn together.

Let's get started!

### First, a bit of history
Javascript was introduced by Netscape Communications in 1995 as a weakly types scripting language, with similar syntax
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
the code and therefore help with debugging and development process. Compile-time type checking is something that is still
very useful in large projects. 

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



