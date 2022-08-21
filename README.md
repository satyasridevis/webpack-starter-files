# webpack-starter-files
Webpack starter file from Brad Traversy. Learnt how to configure webpack for packages and plugins
Web Pack 5

Webpack setup
Webpack Dev Server
HTML Webpack Plugin
Sass Compiling
Babel Transpiling
Asset Resource Loaders
Analyzer Plugin
Source Maps

## Webpack Setup

- create a new directory "**Webpack-starter**"
- create two directory or folders - **src** and **dist**
- src/index.js
- dist/index.html
- open live server
- create generatejoke.js file in **src** folder
- generateJoke.js
- 

```bash
function generateJoke()
{
console.log("checking");
}

export default generateJoke;
```

- import file in index.js and
- import generateJoke from “./generateJoke.js”;
- generateJoke();
- It Won’t inport
- script build: “webpack —mode production”

```bash
npm init -y
npm i -D webpack webpack-cli
npm run build
```

- creates a **main.js**
- create a file called '**webpack.config.js**'

```jsx
const path = require("path");
    
    module.exports = {
    
    mode: "development",
    
    entry: {
    
    bundle: path.resolve(__dirname, "src/index.js"),
    
    },
    
    output: {
    
    path: path.resolve(__dirname, "dist"),
    
    filename: "[name].js",
    
    },
    
    };
```

1. in **package.json**

```jsx
delete --mode production
script: 
{ 
build :"webpack"
}

```

- delete main.js and run

```bash
npm run build
```

- generates a **bundle.js** file and change in script src in index.html

```jsx
<script src="bundle.js"></script>
```

## Sass Compiling

```bash

npm i -D sass style-loader css-loader sass-loader
```

- In src directory, create a **styles folder** and create **main.scss**
- copy code from webpack-starter github repository for main.scss
1. add **main.scss** in index.js file like

```jsx
 import './styles/main.scss'
```

- add configuration in **webpack.config.file**

```jsx
module :{
rules:[
      {

       test: /\.scss$/,
       use: ['style-loader','css-loader','sass-loader']
       }
]}
```

again run and then

## HTML Webpack Plugin

```bash
npm run build
npm i -D html-webpack-plugin

```

In webpack-config.js add 

```jsx
const HtmlWebpackPlugin = require('html-webpack-plugin')

plugins: [

new HtmlWebpackPlugin({

title: "Webpack App",

filename: "index.html",

template: "src/template.html",

}),

],
```

- delete dist folder and  npm run build
- we cant change index.html in dist folder so create a **template.html** inside src folder

Insert Code in template.html

```html
<title><%= htmlWebpackPlugin.options.title %></title>

<div class="container">
      <img alt="" id="laughImg" />
      <h3>Don't laugh Challenge</h3>
      <div id="joke" class="joke"></div>
      <button id="jokeBtn" class="btn">Get Another Joke</button>
    </div>
```

```bash
npm run build
```

## Caching

So we're using webpack to bundle our modular application which yields a deployable `/dist` directory. Once the contents of `/dist` have been deployed to a server, clients (typically browsers) will hit that server to grab the site and its assets. The last step can be time consuming, which is why browsers use a technique called [caching](https://en.wikipedia.org/wiki/Cache_(computing))
This allows sites to load faster with less unnecessary network traffic. However, it can also cause headaches when you need new code to be picked up.

```jsx
output: {
     
     filename: '[name].[contenthash].js',
     path: path.resolve(__dirname, 'dist'),
     clean: true,
}
```

then delete dist folder and again run

```bash
npm run build 
```

## Webpack Server

```bash
npm i -D webpack-dev-server
```

In Webpack.config.file

```jsx
devServer: {

static: {

directory: path.resolve(__dirname, "dist"),

},

port: 3000,
de
open: true,

hot: true,

compress: true,

historyApiFallback: true,

},
```

in package.json file add in scripts

```jsx
scripts: {

"build": "webpack",
"dev": "webpack serve"
}
```

```bash
npm run dev
```

if i add some code in index.js

again run npm run build, two bundle file will be created 

to keep it clean 

in webpack.config,js, add

```jsx
output:
{ clean: true}
//(avoid duplicate bundle.js)
```

delete dist folder and again run

```jsx
npm run dev
```

## Source Mapping

**devtool**

`string = 'eval'` `false`

Choose a style of [source mapping](http://blog.teamtreehouse.com/introduction-source-maps) to enhance the debugging process. These values can affect build and rebuild speed dramatically.

A source map provides a way of mapping code within a compressed file back to it’s original position in a source file

```jsx
devtool: 'source-map'
```

## Babel Transpiling

```bash
npm i -D babel-loader @babel/core @babel/preset-env
```

```jsx
modules:{
rules:[
{
   test: /\.js$/,
   exclude: /node_modules/,
   use: {
   loader: "babel-loader",
   options: {
    presets: ["@babel/preset-env"],
          },
        },
      },]
}
```

```bash
npm run build
```

## Asset Resource loaders

- create a asset folder in src and copy laughing.svg from repo

```jsx
import laughing from "./assets/laughing.svg"; 
// add in index.js
```

```jsx
{
test: /.(png|svg|jpg|jpeg|gif)$/i,
type: "asset/resource",
},
// add in output 
assetModuleFilename: "[name][ext]"

```

- A new laughing.svg image file created in dist folder
- In Template.html img tag with id attribute

```html
<img alt="" id="laughImg" />
```

In index.js file

```jsx
const laughImg = document.getElementById("laughImg");
laughImg.src = laughing;
```

## API Integration

In generateJoke.js

```jsx
import axios from "axios";

function generateJoke() {
  const config = {
    headers: {
      Accept: "application/json",
    },
  };
  axios.get("https://icanhazdadjoke.com", config).then((res) => {
    document.getElementById("joke").innerHTML = res.data.joke;
  });
}
```
