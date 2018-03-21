# Create React App with Webpack 4 from Scratch
Port of This Tut: https://medium.freecodecamp.org/part-1-react-app-from-scratch-using-webpack-4-562b1d231e75


mkdir react_boiler
cd react_boiler
npm init
npm i webpack webpack-cli -D
mkdir src
touch ./src/index.js
echo "console.log(\"Hello World.\")" >> ./src/index.js
package.json```diff
{
  "name": "react_boiler",
  "version": "1.0.0",
  "description": "React with Webpack using Babel to transpile",
  "main": "index.js",
  "scripts": {
-    "test": "echo \"Error: no test specified\" && exit 1"
+    "test": "echo \"Error: no test specified\" && exit 1",
+    "start": "webpack --mode development",
+    "build": "webpack --mode production"
  },
  "author": "Baz-Co",
  "license": "MIT",
  "devDependencies": {
    "webpack": "^4.2.0",
    "webpack-cli": "^2.0.12"
  }
}
```
npm i react react-dom -S
npm i babel-core babel-loader babel-preset-env babel-preset-react -D
touch webpack.config.js```js
module.exports = {
  module: {
    rules: [
      {
        test: /\.js$/,
        exclude: /node_modules/,
        use: {
          loader: "babel-loader"
        }
      }
    ]
  }
};
```
touch .babelrc```
{
  "presets": ["env", "react"]
}
```
index.js```diff
- console.log("Hello World.")
+ import React from "react";
+ import ReactDOM from "react-dom";
+ 
+ const Index = () => {
+   return <div>Hello React!</div>;
+ };
+ 
+ ReactDOM.render(<Index />, document.getElementById("index"));
```
touch ./src/index.html```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta http-equiv="X-UA-Compatible" content="ie=edge">
  <title>React and Webpack4</title>
</head>
<body>
  <section id="index"></section>
</body>
</html>
```
npm i html-webpack-plugin -D
webpack.config.js```diff
+ const HtmlWebPackPlugin = require("html-webpack-plugin");
+ 
+ const htmlPlugin = new HtmlWebPackPlugin({
+   template: "./src/index.html",
+   filename: "./index.html"
+ });
+ 
module.exports = {
  module: {
    rules: [
      {
        test: /\.js$/,
        exclude: /node_modules/,
        use: {
          loader: "babel-loader"
        }
      }
    ]
-  }
+  },
+  plugins: [htmlPlugin]
}
```
npm i webpack-dev-server -D
package.json```diff
{
  "name": "react_boiler",
  "version": "1.0.0",
  "description": "React with Webpack using Babel to transpile",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
-    "start": "webpack --mode development",
+    "start": "webpack-dev-server --mode development --open --hot",
    "build": "webpack --mode production"
  },
  "author": "Baz-Co",
  "license": "MIT",
  "devDependencies": {
    "babel-core": "^6.26.0",
    "babel-loader": "^7.1.4",
    "babel-preset-env": "^1.6.1",
    "babel-preset-react": "^6.24.1",
    "html-webpack-plugin": "^3.0.7",
    "webpack": "^4.2.0",
    "webpack-cli": "^2.0.12"
  },
  "dependencies": {
    "react": "^16.2.0",
    "react-dom": "^16.2.0"
  }
}
```
npm i css-loader style-loader -D
webpack.config.js```diff
const HtmlWebPackPlugin = require("html-webpack-plugin")

- const htmlPlugin = new HtmlWebPackPlugin({
+ const htmlWebpackPlugin = new HtmlWebPackPlugin({
  template: "./src/index.html",
  filename: "./index.html"
})

module.exports = {
  module: {
    rules: [
      {
        test: /\.js$/,
        exclude: /node_modules/,
        use: {
          loader: "babel-loader"
        }
-      }
+      },
+      {
+        test: /\.css$/,
+        use: ["style-loader", "css-loader"]
+      }
    ]
  },
-  plugins: [htmlPlugin]
+  plugins: [htmlWebpackPlugin]
}
```
webpack.config.js```diff
const HtmlWebPackPlugin = require("html-webpack-plugin");

const htmlWebpackPlugin = new HtmlWebPackPlugin({
  template: "./src/index.html",
  filename: "./index.html"
});

module.exports = {
  module: {
    rules: [
      {
        test: /\.js$/,
        exclude: /node_modules/,
        use: {
          loader: "babel-loader"
        }
      },
      {
        test: /\.css$/,
-        use: ["style-loader", "css-loader"]
+        use: [
+          {
+            loader: "style-loader"
+          },
+          {
+            loader: "css-loader",
+            options: {
+              modules: true,
+              importLoaders: 1,
+              localIdentName: "[name]_[local]_[hash:base64]",
+              sourceMap: true,
+              minimize: true
+            }
+          }
+        ]
      }
    ]
  },
  plugins: [htmlWebpackPlugin]
};
```
touch index.css
echo "* { color: red }" >> index.css
index.js```diff
import React from "react";
import ReactDOM from "react-dom";
+
+ import './index.css'

const Index = () => {
  return <div>Hello React!</div>;
};

ReactDOM.render(<Index />, document.getElementById("index"));
```
mkdir ./src/Components
mkdir ./src/Components/App
touch ./src/Components/App/App.js```js
import React, { Component } from 'react'

class App extends Component {
  render() {
    return <h1>Hello There</h1>
  }
}

export default App
```
index.js```diff

import React from "react"
import ReactDOM from "react-dom"

import './index.css'
+ 
+ import App from "./Components/App/App"
+ 
- const Index = () => {
-   return <div>Hello React!</div>;
- };
- 
- ReactDOM.render(<Index />, document.getElementById("index"));
+ ReactDOM.render(<App />, document.getElementById("index"))


```
