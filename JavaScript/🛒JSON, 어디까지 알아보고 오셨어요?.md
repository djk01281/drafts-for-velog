## JSON, 어디까지 알아보고 오셨어요?
![](https://media0.giphy.com/media/T8To8j0yy1PxGCH6Yu/giphy.gif?cid=82a1493bfkavzom1dxn5ey4y5qm8zbdc154zapynz0la2hwg&rid=giphy.gif&ct=g)JSON. 웹개발에 관심이 있다면 많이도 보고 들었을 거다. 정말 어딜가든 보이는 JSON, 그게 도대체 뭘까? 

<br>

## 이름이 어떻게 JSON?

![](https://media2.giphy.com/media/xT9KVn8tApfgnNLpL2/giphy.gif?cid=6c09b95217b37b017d88501a1635e8b93443cec8944b0387&rid=giphy.gif&ct=g)JSON은 **JavaScript Object Notation**의 줄임말이다. Notation, 그니까 **자바스크립트의 Object 타입을 닮은 하나의 형식**이다. 닮았으니까 변환하기도 쉽다. 

<br>

## 형식인데 왜 그렇게 중요하지?

되게 **유용한 형식**이라는 점이 중요하다. 인간이 읽거나 쓰기 쉽다. 기계가 파싱하거나 만들어내기도 쉽다.
쉽게 말해 **정보를 담기 쉽고, 전송하기도 편하다.** 그래서 온갖 곳에 다 쓰인다.

<br>

다음은 **파파고 번역 API**의 응답 예시다.
```
{
    "message": {
        "@type": "response",
        "@service": "naverservice.nmt.proxy",
        "@version": "1.0.0",
        "result": {
            "srcLangType":"ko",
            "tarLangType":"en",
            "translatedText": "tea"
        }
    }
}
```
번역해달라는 **요청**을 보내면 위와같은 **응답**이 돌아온다. 
그렇다. **JSON** 그 녀석이다.

<br>

다음은 **npm**의 **package.json**이다.

```
> npm init --yes
Wrote to /home/monatheoctocat/my_package/package.json:

{
  "name": "my_package",
  "description": "",
  "version": "1.0.0",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "repository": {
    "type": "git",
    "url": "https://github.com/monatheoctocat/my_package.git"
  },
  "keywords": [],
  "author": "",
  "license": "ISC",
  "bugs": {
    "url": "https://github.com/monatheoctocat/my_package/issues"
  },
  "homepage": "https://github.com/monatheoctocat/my_package"
}
```
JSON에 **다양한 정보를 쉽게 담을 수 있고**, JSON으로부터 **정보를 추출해오기도 쉬워** 다양한 곳에 많이 쓰인다.

<br>

## 그래서 진짜 뭔데?
JSON이 뭐냐고? JSON은 **string** 형태로 존재한다. 
그니까 **JavaScript의 Object의 형식을 빌린 string**일 뿐이다. 전송하기 위해서는 string 형태여야 한다.
![](https://64.media.tumblr.com/52c770445274c5da2d05c639a07a935e/tumblr_nfa440YMMU1tq4of6o1_500.gif)(아..알겠다..!)
<br>

## 어떻게 쓰는데?
그럼 이 string을 어디선가 받아왔다고 치자. **이걸로 뭘 할 수 있을까?** 정보가 담겨있는 string이니까 정보에 접근해야 할 거다. **정보에 어떻게 접근할 수 있을까?** ![](https://media0.giphy.com/media/3orif6MDn5Osyps0HC/giphy.gif?cid=82a1493blpbao9gva4tuo3232zol3jptdogei3pqj4lqeo39&rid=giphy.gif&ct=g)매번 `console.log`로 찍어서 눈으로 확인해야 할까?
아니면 string의 문자를 하나하나 돌면서 **key**와 **value**를 구분해내는 코드를 작성할 수도 있을 거다.

다행히 이 string은 **Object과 닮았기 때문에 Object로 변환하기도 쉽다.** 

<br>

## JSON과 JavaScript

JavaScript는 **JSON Object**를 제공한다.
매번 불러서 만들거나 하는 Object는 아니다. 

<br>

JavaScript에서는 랜덤한 수를 만들어내고 싶을 때, 아래처럼 **`Math.random()`**을 쓴다.
```jsx
function getRandomInt(max) {
  return Math.floor(Math.random() * max);
}
```
0부터 max-1까지 중에서 랜덤한 수를 골라주는 함수다.
어쨌든, 이때 쓰는 **Math Object**같은 역할을 하는 게 **JSON Object**이다.

<br>

JSON string을 Object형태로 읽어오고 싶을 때는 JSON Object 안에 **내장된 method**인 **`parse()`**를 실행하면 된다.
```jsx
const json = '{"result":true, "count":42}';
const obj = JSON.parse(json);

console.log(typeof obj);
// output: "object"
```
`json`이라는 string을 만들어서 `JSON.parse()`에 넘겨줬더니 **Object가 되었다! **

<br>

반대로 이미 존재하는 Object를 어딘가에 전송하고 싶다면 string으로 바꿔줘야 한다. 이때는 `JSON.stringfy()`를 실행한다.

```jsx
console.log(JSON.stringify({ x: 5, y: 6 }));
// expected output: "{"x":5,"y":6}"
```
Object가 **string이 되었다!**

<br>

## 호기심 해결!

어디든지 쓰이는 **JSON**에 대해 알아봤다. 생각보다 어렵진 않지만, 웹개발에서 항상 등장하는 개념인만큼, 많이 써서 친숙해지는 것도 중요하다.  

🍰그런 의미에서 **다음** 포스팅에서는 **JSON**과 **API**를 활용한 간단한 **튜토리얼**을 만들어볼까 생각 중이다.🧑🏻‍🍳

<br>

### 참고
[mdn docs](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/JSON)
