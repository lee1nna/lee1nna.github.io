---
title:  "[React + Webpack] 웹팩 세팅 + 빌드하기"
excerpt: "React에서 웹팩을 세팅하고 빌드해보자!"

categories:
  - React
tags:
  - [React, Webpack]

toc: true
toc_sticky: true

date: 2022-11-09
last_modified_at: 2022-11-09
---
1. 프로젝트 파일 들어가서 `npm init` 실행하면 package.json 파일 생성
2. `npm react react-dom`
3. `npm i -D webpack webpack-cli`
4. `npm i -D babel-loader @babel/core`
5. `npm i -D @babel/preset-env`
6. `npm i -D @babel/preset-react`
7. 프로젝트 폴더에 `webpack.config.js` 파일 추가

```jsx
// webpack.config.js

const path = require('path');

module.exports = {
    mode:'development',
    devtool: 'eval', // 개발일 땐 eval, 프로덕션일 땐 hidden-source-map
    resolve: {
        extensions: ['.jsx', 'js']
    },

    entry: {
        app: './client.jsx'
    },

    module: {
        rules: [{
            test: /\.jsx?$/,
            loader: 'babel-loader',
            options: {
                presets: ['@babel/preset-env', '@babel/preset-react'],
            }
        }],
    },

    output: {
        filename: 'app.js',
        path: path.join(__dirname, 'dist')
    }
}
```

```jsx
// package.json
{
  "name": "gugudan",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "dev": "webpack" // 해당 스크립트 추가
  },
  "author": "leehanna",
  "license": "MIT",
  "dependencies": {
    "react": "^18.2.0",
    "react-dom": "^18.2.0"
  },
  "devDependencies": {
    "@babel/core": "^7.20.2",
    "@babel/preset-env": "^7.20.2",
    "@babel/preset-react": "^7.18.6",
    "babel-loader": "^9.1.0",
    "webpack": "^5.74.0",
    "webpack-cli": "^4.10.0"
  }
}
```

웹팩 세팅과 스크립트 설정이 끝나면 `npm run dev` 를 실행한다.

![웹팩 빌드](https://user-images.githubusercontent.com/71548623/200852044-fb9ae275-eb5e-4361-bc50-260c05dec9fc.png)

프로젝트 폴더에 dist파일이 생성되면 웹팩 빌드가 성공한 것이다 !!
