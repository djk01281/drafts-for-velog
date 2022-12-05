

firebase는 hosting 외에도 다양한 백엔드 기능을 제공한다. 
이번에는 흔히 볼 수 있는 **Sign in with Google** 기능을 추가해보자. 

이 포스팅은 **시리즈**의 4번째 글로 [**1편**](https://velog.io/@djk01281/Firebase%EB%A1%9C-%EB%B0%B1%EC%97%94%EB%93%9C-%EB%A7%8C%EB%93%A4%EA%B8%B0-1-%ED%94%84%EB%A1%9C%EC%A0%9D%ED%8A%B8-%EC%8B%9C%EC%9E%91%ED%95%98%EA%B8%B0), [**2편**](https://velog.io/@djk01281/Firebase로-백엔드-만들기-2-코드-작성과-webpack-설치), [**3편**](https://velog.io/@djk01281/Firebase로-백엔드-만들기-3Firebase-설치와-Deploy)을 놓쳤다면 **꼭** 보고 오자!

## 프로젝트에서 Google Auth 활성화하기

![](https://velog.velcdn.com/images/djk01281/post/a264dcc3-015f-49c8-bce9-badd25599ef8/image.png)Firebase 콘솔의 **Authentication** 메뉴로 가준다. 

<br>

![](https://velog.velcdn.com/images/djk01281/post/2ac0a2dc-62a2-47f7-92de-6739f3f55c17/image.png)2번째 탭의 **Sign-in method**에서 제공업체 중 **Google**을 선택해 사용할 수 있다.

<br>

## 프로젝트에 Google Auth 추가하기

Google Auth를 import해와서 쓸 수 있다. 
<br>

```jsx
import { getAuth, GoogleAuthProvider, signInWithPopup } from "firebase/auth";
```

**`index.js`**의 맨 위에 위와 같이 추가해준다.  이제 **getAuth**, **GoogleAuthProvider**, **signInWithPopup**을 쓸 수 있다.

<br>

```jsx
const auth = getAuth();
```

firebase의 기능들은 **`get(…)`** 형식의 이름을 갖는 모듈을 제공한다. 불러와서 `auth`라는 변수에 저장해준다. 

<br>

## 버튼과 onclick 완성하기

```html
<button class="signInBtn">Sign in with Google</button>
```

우선 `index.html` 파일에 버튼을 만들어주자. 

<br>

```jsx
const signInBtn = document.getElementById("signInBtn");
```

`index.js`에서는 버튼을 불러온 후

<br>

```jsx
signInBtn.onclick = function () {
  const provider = new GoogleAuthProvider();
  provider.addScope("profile");
  provider.addScope("email");

  signInWithPopup(auth, provider)
    .then((result) => {
      const user = result.user;
      const outputElement = document.createElement("h1");

      // outputDiv.textContent = Object.keys(user);
      //providerId,proactiveRefresh,reloadUserInfo,reloadListener,uid,auth,stsTokenManager,accessToken,displayName,email,emailVerified,phoneNumber,photoURL,isAnonymous,tenantId,providerData,metadata

      outputElement.textContent = `Welcome, ${user.displayName}`;
      document.body.appendChild(outputElement);
    })
    .catch((error) => {
      const errorMessage = error.message;
      const outputElement = document.createElement("h1");
      outputElement.textContent = `Error: ${errorMessage}`;
      document.body.appendChild(outputDiv);
    });
};
```

**클릭 이벤트 시 실행**될 로직을 위와 같이 만들어준다. 

복잡해보일 수 있지만, Google이란 **Provider**를 통해 **`signInWithPopup`**을 실행시키고, 성공하면 result로부터 **사용자의 이름**을, 실패하면 **에러 메시지**를 보여주는 것 뿐이다. 

주석처리된 부분은 `result.user` 오브젝트가 어떤 이름의 key들을 갖고 있는지 몰라서 화면에 찍어본 흔적이다. `displayName`이 있으니 성공하면 **이름**과 함께 **환영 메시지**를 보여주자. 
<br>

## index.js 전체 코드

```jsx
import { initializeApp } from "firebase/app";
import {
  getAuth,
  GoogleAuthProvider,
  getRedirectResult,
  signInWithRedirect,
  onAuthStateChanged,
  signInWithPopup,
} from "firebase/auth";
// For Firebase JS SDK v7.20.0 and later, measurementId is optional
const firebaseConfig = {
  apiKey: "AIzaSyATCD9cxXSJ2RSUwd9WSDHTDMV7FuSaCgo",
  authDomain: "fir-demo-48710.firebaseapp.com",
  projectId: "fir-demo-48710",
  storageBucket: "fir-demo-48710.appspot.com",
  messagingSenderId: "49933278590",
  appId: "1:49933278590:web:bb643d3e5d7b2077cd672f",
  measurementId: "G-HFF9Z86BJM",
};

initializeApp(firebaseConfig);
const auth = getAuth();

const signInBtn = document.getElementById("signInBtn");
signInBtn.onclick = function () {
  const provider = new GoogleAuthProvider();
  provider.addScope("profile");
  provider.addScope("email");

  signInWithPopup(auth, provider)
    .then((result) => {
      const user = result.user;
      const outputElement = document.createElement("h1");
      // outputDiv.textContent = Object.keys(user);
      //providerId,proactiveRefresh,reloadUserInfo,reloadListener,uid,auth,stsTokenManager,accessToken,displayName,email,emailVerified,phoneNumber,photoURL,isAnonymous,tenantId,providerData,metadata
      outputElement.textContent = `Welcome, ${user.displayName}`;
      document.body.appendChild(outputElement);
    })
    .catch((error) => {
      const errorMessage = error.message;
      const outputElement = document.createElement("h1");
      outputElement.textContent = `Error: ${errorMessage}`;
      document.body.appendChild(outputDiv);
    });
};
```
전체 코드는 위와 같다.

## build and deploy

```bash
npm run build
```

코드에 변화가 생겼으니 **webpack**으로 다시 빌드시켜주자.
<br>
 

```bash
firebase deploy
```

**배포**시킨다. (만약 firebase라는 걸 찾을 수 없다는 내용의 **에러**가 나오면, 이전 글로 돌아가서 firebase tools 설치부터 firebase login, firebase init까지 실행시켜주자.)

<br>

![](https://velog.velcdn.com/images/djk01281/post/05ee5cb0-4bb5-4c51-b968-a04b277c1e91/image.png)배포된 사이트에서 **버튼**을 눌러 로그인해보자.

<br>

![](https://velog.velcdn.com/images/djk01281/post/90dfbb53-832b-4898-bf52-890cec306f41/image.png)잘 나오는 걸 볼 수 있다!

## Summary

Firebase 프로젝트 시작부터, 배포 그리고 Google Auth 기능 추가까지 해봤다. 
이 외에도 Firebase에는 Storage 등 백엔드를 구성하기 위한 다양한 기능이 더 많이 있다. 하지만 **처음 시작해보려고 하는 사람들을 위한 가이드**인 이 시리즈는 여기서 **마무리** 지으려고 한다. 

☺️처음 작성한 포스트였는데, 알게 된 내용을 공유하는 게 나름 의미가 있고 재밌네요. 앞으로 쓰게 될 다른 글도 많이 읽어주세요👏🏻
