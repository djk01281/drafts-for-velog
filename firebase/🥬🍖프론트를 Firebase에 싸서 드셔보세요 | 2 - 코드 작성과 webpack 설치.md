
[**1편**](https://velog.io/@djk01281/Firebase%EB%A1%9C-%EB%B0%B1%EC%97%94%EB%93%9C-%EB%A7%8C%EB%93%A4%EA%B8%B0-1-%ED%94%84%EB%A1%9C%EC%A0%9D%ED%8A%B8-%EC%8B%9C%EC%9E%91%ED%95%98%EA%B8%B0)을 놓쳤다면 **꼭** 보고 오자!

## 기본 파일들 작성

원하는 곳에 프로젝트 폴더를 만든 후 **`dist/index.html`**, **`src/index.js`**를 하나씩 만들어준다. 

![](https://velog.velcdn.com/images/djk01281/post/268636aa-88c1-4e64-83e5-9c455cc9f87d/image.png)파일 구조는 다음과 같다.
```
.
├── dist          
│   └── index.html       
├── src
│   └── index.js          
└── README.md
```

<br>

`index.js`는 나중에 수정하고, 일단 **`index.html`**에 들어가 다음과 같이 작성해주자.

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta http-equiv="X-UA-Compatible" content="IE=edge" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Firebase-9-Demo</title>
    <script src="bundle.js" defer></script>
  </head>
  <body>
    <h1>Firebase🔥<h1>
  </body>
</html>
```

**`<script>`**태그 안의 `bundle.js`라는 파일은 나중에 설정할 webpack이 생성해준다.
특정 주소로 들어갔을 때, 이 html 페이지를 보는게 우리의 **목표**다.

<br>

## npm 초기화
Node.js가 설치되어 있단 가정 하에 npm command를 쓸 수 있다. 
<br>
```bash
npm init
```

실행하면, `package.json`가 생성된다.

앞으로 `npm run build`로 webpack을 실행할 예정이므로 **`package.json`을 수정**해줘야 한다.

**`scripts`**라고 되어 있는 부분에 **`“build”: “webpack”`**을 추가해주자.

```json
"scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "build": "webpack"
  },
```

<br>

## Webpack 설치

```bash
npm i webpack webpack-cli -D
```

`-D` 옵션: Development Dependency에 추가

윗 코드를 실행해 **webpack**을 설치한다. Firebase 라이브러리 안에서 우리가 import해서 쓰는 모듈만 output file에 번들링 해주는 역할을 할 거다. 

<br>

## Webpack.config.js

설치 후, 프로젝트의 **root** folder에 **`webpack.config.js`**란 파일을 만든다. 

안에는 다음과 같이 적어준다.

```jsx
const path = require("path");

module.exports = {
  mode: "development",
  entry: "./src/index.js",
  output: {
    path: path.resolve(__dirname, "dist"),
    filename: "bundle.js",
  },
  watch: true,
};
```

자세한 설명은 생략하겠지만, `entry`의 파일을 통해 연결된 모든 파일들을 `ouput`에 따라 우리가 HTML에 추가했던 **`bundle.js`**를 만들어준다는 뜻이다. npm, webpack 등에 대해서는 또 정리를 해서 올릴 생각이다.

참고로 `watch: true`는 webpack이 지켜보고 있다가 파일에 변경이 생기면 다시 번들링해준다는 뜻이다.

**🔥[다음](https://velog.io/@djk01281/Firebase로-백엔드-만들기-3Firebase-설치와-Deploy)에는 firebase를 설치하고 deploy까지 해보자!**
