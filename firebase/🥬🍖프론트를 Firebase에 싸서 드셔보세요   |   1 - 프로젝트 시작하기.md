## Motivation

프론트만 만들어오다가 **BaaS**(Backend as a service)를 이용해, 백엔드를 구축하고 싶었다. 지금의 **Firebase**는 8이 아닌 9 버전으로, 바뀐 부분이 많아 배우면서 어려움이 있었다. 삽질하면서 배운 **Firebase 사용법을 깔끔하게 정리**하고 싶었다. 

<br>

## Firebase 8 vs 9

Version 8에서 9이 되면서, Firebase를 이용하는 방식에 변화가 생겼다. 기존에는 Firebase Object를 만들어서 그안의 method를 실행했다. 

반면, 이제는 **webpack**같은 **module bundler**로 필요한 부분만 `import`해와서 사용할 수 있다. 

```jsx
import { initializeApp } from "firebase/app";

initializeApp();
```

<br>

## Overview

전체 과정을 요약하면 다음과 같다.

1. **Firebase** 프로젝트를 생성한다. 
2. **NPM**으로 **Webpack**을 설치 후 설정해준다. 
3. **NPM**으로 **Firebase**를 설치한다. **`node_modules`** 폴더에 저장된다. 
4. Firebase를 사용하고 싶은 js 파일에서 원하는 부분을 **`import`**해온다. 
5. **Webpack**이 bundling할 때, 필요한 dependencies를 적절히 대체해서 하나의 **output** 파일로 만들어준다.
6. **Firebase-tools**를 이용해 사이트에 **deploy**한다. 🔥

<br>

## Firebase 프로젝트 생성

[https://firebase.google.com](https://firebase.google.com/)

![](https://velog.velcdn.com/images/djk01281/post/1281eac3-0bdd-464f-9d56-7bc2d1327a43/image.png)Firebase 홈페이지에 들어가 **Get Started** 버튼을 클릭한다.   

<br>![](https://velog.velcdn.com/images/djk01281/post/d7797353-7eb3-4195-b4e7-6b30e47103eb/image.png)**+ 프로젝트 추가**를 누른다.

<br>![](https://velog.velcdn.com/images/djk01281/post/8bca95ca-d446-4f26-a103-1495729a67e2/image.png)프로젝트 **이름을 지정**해준다. 

<br>![](https://velog.velcdn.com/images/djk01281/post/66426454-3081-4938-a051-ea8458ab29a6/image.png)지금은 필요가 없으니 Google 애널리틱스 사용은 꺼준 후 **프로젝트 만들기**를 클릭한다. 

<br>![](https://velog.velcdn.com/images/djk01281/post/bd0f0182-f9b3-4e65-866f-62c0a86c2e81/image.png)프로젝트가 준비되었다. 

<br>![](https://velog.velcdn.com/images/djk01281/post/184f1a75-331a-4aff-9693-9ed314b76619/image.png)프로젝트 이름(**myProject**) 밑에 **</>버튼**을 클릭해서 **앱을 추가**해준다. 

<br>![](https://velog.velcdn.com/images/djk01281/post/d62f3441-7abd-4fdd-8ecd-c1a909ae179f/image.png)**앱 닉네임**과 **호스팅 설정**을 해준다. 그냥 **체크**해주면 된다.

<br>![](https://velog.velcdn.com/images/djk01281/post/71dc33c1-e376-47dc-b718-a998288289d1/image.png)어차피 나중에 설정해줄 내용이니 **다음**을 눌러 우선은 쭉 넘어가면 된다. 

<br>![](https://velog.velcdn.com/images/djk01281/post/b614048a-7061-416d-8bb4-dad2e62c1754/image.png) 마찬가지로 **넘어가준다**. 


<br>![](https://velog.velcdn.com/images/djk01281/post/918c194d-b7d1-4b95-8918-07ae4a68dca0/image.png)**콘솔로 이동**을 누르면 웹 앱을 추가할 수 있다. 

**🔥이제 [다음 단계](https://velog.io/@djk01281/Firebase로-백엔드-만들기-2-코드-작성과-webpack-설치)에서는 간단한 프론트 코드를 만들고 webpack을 설치하자.**
