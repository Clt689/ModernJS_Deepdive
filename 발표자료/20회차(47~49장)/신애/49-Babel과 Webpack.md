# `49.1` Babel

- IE 같은 구형 브라우저에서 지원하지 않는 기능을 저사양으로 트랜스파일링.

  ```javascript
  // ES6의 화살표 함수, ES7의 지수 연산자
  [1, 2, 3].map((n) => n ** n);
  ```

  ```javascript
  // ES5 사양으로 트랜스파일링
  'use strict';

  [1, 2, 3].map(function (n) {
    return Math.pow(n, n);
  });
  ```

## `49.1` Babel 설치

### Babel 프리셋 설치와 `Babel.config.json` 설정

> - `@babel/preset-env`: ES6 이상의 코드를 ES5 이하로 트랜스파일링.
> - `@babel/preset-flow` : Flow 문법을 ES5 이하로 트랜스파일링.
> - `@babel/preset-react`: JSX 문법을 React.createElement로 변환.
> - `@babel/preset-typescript`: TypeScript 코드를 ES5 이하로 트랜스파일링.

<br/>

### Babel을 사용하여 ES6 이상의 코드를 ES5 이하로 트랜스파일링한 pacage.json

```json
{
  "name": "esnext-project",
  "version": "1.0.0",
  "scripts": {
    "build": "babel src/js -w -d dist/js"
  },
  "devDependencies": {
    "@babel/cli": "^7.10.3",
    "@babel/core": "^7.10.3",
    "@babel/preset-env": "^7.10.3"
  }
}
```

<br/>

## `49.2` Webpack

> - `Webpack`은 의존관계에 있는 JS, CSS, 이미지 등의 리소스들을 하나(또는 여러 개)의 파일로 번들링하는 모듈 번들러.
> - `Webpack`을 사용하면 의존 모듈이 하나의 파일로 번들링 되므로 별도의 모듈 로더가 필요 없다.
> - `Webpack`과 `Babel`을 이용하여 ES6 이상의 코드를 ES5 이하로 트랜스파일링하고 번들링할 수 있다.

### Babel-loader 설치

```json
// Webpack을 사용하여 ES6 이상의 코드를 ES5 이하로 트랜스파일링한 pacage.json
{
  "name": "esnext-project",
  "version": "1.0.0",
  "scripts": {
    "build": "babel src/js -w -d dist/js"
  },
  "devDependencies": {
    "@babel/cli": "^7.10.3",
    "@babel/core": "^7.10.3",
    "@babel/plugin-proposal-class-properties": "^7.10.1",
    "@babel/preset-env": "^7.10.3",
    "webpack": "^4.43.0",
    "webpack-cli": "^3.3.12"
  }
}
```

<br/>

### Webpack.config.js 설정

```javascript
const path = require('path');

module.exports = {
  // entry file
  // https://webpack.js.org/configuration/entry-context/#entry
  entry: './src/js/main.js',
  // 번들링된 js 파일의 이름(filename)과 저장될 경로(path)를 지정
  // https://webpack.js.org/configuration/output/#outputpath
  // https://webpack.js.org/configuration/output/#outputfilename
  output: {
    path: path.resolve(__dirname, 'dist'),
    filename: 'js/bundle.js',
  },
  // https://webpack.js.org/configuration/module
  module: {
    rules: [
      {
        test: /\.js$/,
        include: [path.resolve(__dirname, 'src/js')],
        exclude: /node_modules/,
        use: {
          loader: 'babel-loader',
          options: {
            presets: ['@babel/preset-env'],
            plugins: ['@babel/plugin-proposal-class-properties'],
          },
        },
      },
    ],
  },
  devtool: 'source-map',
  // https://webpack.js.org/configuration/mode
  mode: 'development',
};
```

  <br/>

### Babel-polyfill 설치

- Polyfill이 필요한 코드

  ```javascript
  // src/js/main.js
  import { pi, power, Foo } from './lib';

  console.log(pi);
  console.log(power(pi, pi));

  const f = new Foo();
  console.log(f.foo());
  console.log(f.bar());

  // polyfill이 필요한 코드
  console.log(
    new Promise((resolve, reject) => {
      setTimeout(() => resolve(1), 100);
    })
  );

  // polyfill이 필요한 코드
  console.log(Object.assign({}, { x: 1 }, { y: 2 }));

  // polyfill이 필요한 코드
  console.log(Array.from([1, 2, 3], (v) => v + v));
  ```

- 트랜스파일링과 번들링이 실행되어 polyfill된 코드

  ```javascript
  ...
  // 190 line
  console.log(new Promise(function (resolve, reject) {
    setTimeout(function () {
      return resolve(1);
    }, 100);
  })); // polyfill이 필요한 코드

  console.log(Object.assign({}, {
    x: 1
  }, {
    y: 2
  })); // polyfill이 필요한 코드

  console.log(Array.from([1, 2, 3], function (v) {
    return v + v;
  }));
  ...
  ```

## 실제 운영환경에서도 사용

- @babel-polyfill은 개발 환경에서만 사용하는 것이 아니라 실제 운영 환경에서도 사용.
- 개발용 의존성(dependencies)로 설치하는 `--save-dev`옵션을 지정하지 않는다.

  ### ES6의 import를 사용하는 경우 진입점의 선두에 폴리필을 로드

  ```javascript
  // src/js/main.js
  import "@babel/polyfill";
  import { pi, power, Foo } from './lib';
  ...
  ```

  ### Webpack을 사용하는 경우 Webpack.config.js 파일의 entry 배열에 폴리필을 로드

  ```javascript
  const path = require('path');

  module.exports = {
    // entry file
    // https://webpack.js.org/configuration/entry-context/#entry
    entry: ['@babel/polyfill', './src/js/main.js'],
  ...
  ```
