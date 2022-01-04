# React Quickstart

## Setup application

### Application root
```bash
mkdir <app> && cd <app>
```

### `package.json`
```bash
npm init -y
```
After initialization of the package configuration file, adjust it to the application's individual requirements.
For convenience, it is recommended to add commands to deal with the application's development lifecycle:

```json
"scripts": {
  "build": "webpack",
  "start": "webpack serve",
  "clean": "rm -rf build/*"
},
```

### `src/`
```bash
mkdir src/ && cd src/
touch index.html
touch index.tsx
touch App.tsx
```

#### `src/index.html`
```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, user-scalable=no, initial-scale=1.0, maximum-scale=1.0, minimum-scale=1.0" />
    <title>APP</title>
  </head>
  <body>
    <noscript>JavaScript is required to execute this app</noscript>
    <div id="root"></div>
  </body>
</html>
```

The `<div id=root></div>` is the mounting point for the React application that we are going to create.

#### `src/index.tsx`
```typescript
import React from 'react';
import ReactDOM from 'react-dom';
import App from './App';

ReactDOM.render(<App />, document.getElementById('root'));
```

#### `src/App.tsx`
```typescript
import React from 'react';

const App = () => {
  return <h1>hello world</h1>;
};

export default App;
```

## Webpack

### Installation
```bash
npm install -D webpack webpack-cli webpack-dev-server
npm install -D install path html-webpack-plugin style-loader css-loader sass-loader node-sass file-loader @svgr/webpack
```

### `webpack.config.js`
```bash
touch webpack.config.js
```

After creation of the webpack config, append the following:

```javascript
const path = require('path');
const HtmlWebpackPlugin = require('html-webpack-plugin');

module.exports = {
  entry: path.join(__dirname, 'src', 'index.tsx'),
  output: {
    path: path.join(__dirname, 'build'),
    filename: 'index.bundle.js',
  },
  resolve: {
    extensions: ['.tsx', '.ts', '.js'],
  },
  devServer: {
    static: {
      directory: path.join(__dirname, 'public'),
    },
    compress: true,
    port: 9000,
  },
  mode: process.env.NODE_ENV || 'development',
  module: {
    rules: [
      {
        test: /\.(js|jsx)$/,
        exclude: /node_modules/,
        use: ['babel-loader'],
      },
      {
        test: /\.(ts|tsx)$/,
        exclude: /node_modules/,
        use: ['ts-loader'],
      },
      {
        test: /\.(css|scss)$/,
        use: ['style-loader', 'css-loader', 'sass-loader'],
      },
      {
        test: /\.(jpg|jpeg|png|gif|mp3)$/,
        use: ['file-loader'],
      },
      {
        test: /\.svg$/,
        use: ['@svgr/webpack'],
      }
    ],
  },
  plugins: [
    new HtmlWebpackPlugin({
      template: path.join(__dirname, 'src', 'index.html'),
    }),
  ],
};
```

## Babel
```bash
npm install -D @babel/core @babel/node @babel/preset-env @babel/preset-react @babel/preset-typescript babel-loader
```

### `.babelrc`
```bash
touch .babelrc
```

```json
{
  "presets": [
    "@babel/preset-env",
    "@babel/preset-typescript",
    "@babel/preset-react"
  ]
}
```

## React
```bash
npm install react react-dom @types/react @types/react-dom
```

## TypeScript
```bash
npm install typescript
npm install -D ts-loader
```

### `tsconfig.json`
```bash
touch tsconfig.json
```

```json
{
  "compilerOptions": {
    "target": "es5",
    "lib": [
      "dom",
      "dom.iterable",
      "esnext"
    ],
    "allowJs": true,
    "skipLibCheck": true,
    "esModuleInterop": true,
    "allowSyntheticDefaultImports": true,
    "sourceMap": true,
    "strict": true,
    "forceConsistentCasingInFileNames": true,
    "noFallthroughCasesInSwitch": true,
    "module": "esnext",
    "moduleResolution": "node",
    "resolveJsonModule": true,
    "isolatedModules": true,
    "noEmit": false,
    "jsx": "react-jsx"
  },
  "include": [
    "src"
  ]
}
```

## Application structure
- The application is built under `<app>/build/`. Run `yarn clean` / `npm run clean` to clean up the build.
- Run `npm run build` / `yarn build` to build to application to `<app>/build/`. Webpack determines the build mode
  (development / production) depending on the env variable NODE_ENV, otherwise it will assume that webpack runs in
  development mode. For further information, see `webpack.config.js`.
- Run `npm run start` / `yarn start` to serve webpack in dev mode. The webpack dev server will launch at `localhost:9000`.
