I use this project as the basis for creating a React project without using `create-react-app`:

## Setup

First, initialize the project in new directory:

```sh
npm init
```

To create a initial directoy structure use:

```sh
mkdir public src
```

It's a good time to initialize repository, and to include a `.gitignore` file excluding `node_modules` and `dist`.

```sh
git init
```

```sh
echo "node_modules"$'\r'"dist" >.gitignore
```

In our `public` directory will handle any static assets, and the most important houses our `index.html `file, wich react will utilize to render the app.

Create a `index.html` inside of the `public` directory and paste the following HTML code:

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="UTF-8" />
    <meta
      name="viewport"
      content="width=device-width, initial-scale=1, shrink-to-fit=no"
    />
    <title>React Starter</title>
  </head>

  <body>
    <div id="root"></div>
    <noscript> You need to enable JavaScript to run this app. </noscript>
    <script src="../dist/bundle.js"></script>
  </body>
</html>
```

## Babel

To install Babel, run the following command:

```sh
npm install --save-dev @babel/core @babel/cli @babel/preset-env @babel/preset-react
```

Now we have to create a `.babelrc` file in the project root. Here, we're telling babel that we're going to use the `env` and `react` presets.

```json
{
  "presets": ["@babel/env", "@babel/preset-react"]
}
```

## Webpack

Now, we need to get and configure Webpack. For this, we install the necessary packages and save as dev depenvencies with the following command:

```sh
npm install --save-dev webpack webpack-cli webpack-dev-server style-loader css-loader babel-loader
```

Create a new file at the root directory called `webpack.config.js`. In this file we're going to exports an object with webpack's configuration.

```js
const path = require('path');
const webpack = require('webpack');

module.exports = {
  entry: './src/index.js',
  mode: 'development',
  module: {
    rules: [
      {
        test: /\.(js|jsx)$/,
        exclude: /(node_modules|bower_components)/,
        loader: 'babel-loader',
        options: { presets: ['@babel/env'] },
      },
      {
        test: /\.css$/,
        use: ['style-loader', 'css-loader'],
      },
    ],
  },
  resolve: { extensions: ['*', '.js', '.jsx'] },
  output: {
    path: path.resolve(__dirname, 'dist/'),
    publicPath: '/dist/',
    filename: 'bundle.js',
  },
  devServer: {
    contentBase: path.join(__dirname, 'public/'),
    port: 3000,
    publicPath: 'http://localhost:3000/dist/',
    hotOnly: true,
  },
  plugins: [new webpack.HotModuleReplacementPlugin()],
};
```

## React

Here, we'll need to get three packages: `react`, `react-dom` and `react-hot-loader`, and save those as a regular dependencies.

```sh
npm install --save react react-dom react-hot-loader
```

We'll need to tell our React app where to hook into the DOM (in our `index.html`). Create inside our `src` directory a file called `index.js`, and fill it with following:

```js
import React from 'react';
import ReactDOM from 'react-dom';
import App from './App.js';
ReactDOM.render(<App />, document.getElementById('root'));
```

Now inside `src` create another file called `App.js` (this file is just a React component) and type the following lines:

```js
import React, { Component } from 'react';
import { hot } from 'react-hot-loader';
import './App.css';

class App extends Component {
  render() {
    return (
      <div className="App">
        <h1> Hello, World! </h1>
      </div>
    );
  }
}

export default hot(module)(App);
```

Now, let's add a simple stylesheet to the `src` directory. We'll create a file called `App.css`, with this content:

```css
.App {
  margin: 1rem;
  font-family: Arial, Helvetica, sans-serif;
}
```

Now you can run the webpack server with:

```
webpack serve --mode development
```

You should add this in script section inside the `package.json` file.
