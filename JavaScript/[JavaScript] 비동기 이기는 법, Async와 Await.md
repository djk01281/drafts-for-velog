![](https://velog.velcdn.com/images/djk01281/post/30805e18-ccfb-4466-8c0d-07ecf6224224/image.gif) 
**자바스크립트 시리즈:**

[**[JavaScript] 🛒JSON, 어디까지 알아보고 오셨어요?**](https://velog.io/@djk01281/JSON-어디까지-알아보고-오셨어요-ptpanx2n)

[**[JavaScript] 동기적인(Synchronous) 콜백(callback)함수도 있을까?**](https://velog.io/@djk01281/Synchronous한-콜백함수도-있을까)

[**[JavaScript] 기다리는 자에게 복이 있나니, Promise**](https://velog.io/@djk01281/Promise-뭘-약속한다는-거야)

[**[JavaScript] 비동기 이기는 법, Async와 Await.**](https://velog.io/@djk01281/JavaScript-Async와-Await)


## Async, Await를 간단하게 말하면

Async와 Await는 **Promise**의 Syntatic Sugar다. 즉, Promise를 **더 쉽게 사용할 수 있게** 해준다. Async와 Await가 하는 일은 간단하다. 

**Async**는 단순히 **Promise를 Return**해준다. 

**Await**는 Promise가 Resolve될 때까지 **기다려 준다**. Fulfilled되면 **값을 return**해주고, Rejected되면 **Error를 Throw**해준다.  

<br>

## Promise가 아닌 함수

async으로 지정된 함수 안에서 **Promise가 아닌** asynchronous한 함수를 쓰려면 어떻게 해야할까? 

await는 Promise를 기다려준다. 따라서 그 함수를 이용해 **Promise를 만들어주면** 된다. 그 후 앞에 async를 붙여준다.

<br>

## Async, Await를 사용하지 못하는 곳

async/await를 사용하지 못하는 곳:

```jsx
async function f() {
  let response = await fetch('http://no-such-url');
}

// f() becomes a rejected promise
f().catch(alert); // TypeError: failed to fetch // (*)
```

`async` 키워드가 붙은 함수도 함수이기 때문에 **밖에서** 불리게 된다. 근데 밖은 `async` 함수 내부가 아니기 때문에 **await를 쓸 수 없다.**

<br>

*그럼 어떻게 해야할까?*

<br>

`async` 함수는 결국 **Promise**를 return하기 때문에, 일반적인 Promise처럼도 사용할 수 있다. 즉, 함수의 밖에서는 `.then` 또는 `.catch`로 접근할 수 있다.

따라서 **밖에서** `.catch`로 error를 잡아낼 수 있다. 마치 Promise **.then() chaining**의 마지막 줄에서 `.catch`로 error를 다 잡아내는 것처럼.

<br>

참고로 **promise.all**과도 쓸 수 있다.

```jsx
// wait for the array of results
let results = await Promise.all([
fetch(url1),
fetch(url2),
...
]);
```

<br>

## Async, Await를 쓰는 이유
일단 기본적으로 **보기 더 편하다.** 하지만 그 외에도 다른 이유가 많다.
가장 큰 차이는 Promise를 return하는 함수 앞에 await를 붙이면, **Promise를 return하지 않는다는 점**이다. 

await은 fulfilled되면 value를 돌려주고, rejected되면 error를 thrown한다. 즉, **.then과 .catch를 이용해서 access할 필요 없다.** 다음 라인에 그냥 적으면 되니까. 

이 특성 때문에 크게 **두 가지** 장점을 갖게 된다.

### 하나, Try, Catch를 다시 쓸 수 있다.

error도 thrown하기 때문에 top level에 try/catch를 쓰면 error를 삼켜버릴 문제도 없다. 마치 마지막 줄에 .catch를 쓰는 것과 비슷하다. (물론 함수 밖에서 .catch를 쓰게 되면 그게 마지막 줄이 되겠지만.)

```jsx
async function myFunction() {
  try {
    await somethingThatReturnsAPromise();
  } catch (err) { 
    console.log(err); // oh noes, we got an error
  }
}
```

### 둘, 반복문과 같이 쓸 수 있다.

**.then chaining**을 쓰면 이전 단계의 .then과 물리적으로(코드가 공간적으로) 끊기면 안되기 때문에 **for문이나 while문을 사용할 수 없었다**. 따라서 Promise만 썼을 때는 2가지 중 하나를 택해야 했다.

힘들어도 **.then**으로 전부 이어주거나, 너무 많다면 함수를 **Recursive**하게 만들거나.

*둘 다 직관적이지는 못하다.*

<br>

하지만 **async, await**를 사용하면 .then chaining을 쓸 필요가 없다. 그 줄이 다 끝날 때까지 기다려주기 때문에, 그냥 다음 줄 혹은 다음 반복 때 실행하면 된다. 따라서, **for문, while문에 넣어 쓸 수 있다.**

```jsx
let docs = [{}, {}, {}];

for (let doc of docs) {
  await db.post(doc);
}
```

<br>

## 모든 function에 async를 붙일 필요는 없다

**잘못된 패턴:** 

1. Async한 function someFunction을 어떤  parentFunction 안에서 쓰고 싶음. 
2. `const result = await someFunction`을 하고 싶음. 
3. parentFunction도 `async`로 만듬.

**옳은 패턴:**

1. Async한 function someFunction을 어떤  parentFunction 안에서 쓰고 싶음. 
2. someFunction은 Promise를 리턴한다는 걸 기억
3. parentFunction 안에서 `someFunction.then()`을 사용

*.then()을 사용하자.* 

<br>

## 정리

1. Async, Await는 Promise를 더 쉽고 직관적으로 쓸 수 있게 만들어주는 Syntatic Sugar이다. 그냥 일반적인 코드를 작성하는 것과 가장 이질감이 없다. 그러면서도 Promise처럼 .then으로 접근하는 것 또한 가능하다. 
2. Promise만 썼을 때는 안되던 Try, Catch로 다시 에러 핸들링을 할 수도 있고, 반복문을 쓸 수도 있다.


<br>

**자바스크립트 시리즈:**

[**[JavaScript] 🛒JSON, 어디까지 알아보고 오셨어요?**](https://velog.io/@djk01281/JSON-어디까지-알아보고-오셨어요-ptpanx2n)

[**[JavaScript] 동기적인(Synchronous) 콜백(callback)함수도 있을까?**](https://velog.io/@djk01281/Synchronous한-콜백함수도-있을까)

[**[JavaScript] 기다리는 자에게 복이 있나니, Promise**](https://velog.io/@djk01281/Promise-뭘-약속한다는-거야)

[**[JavaScript] 비동기 이기는 법, Async와 Await.**](https://velog.io/@djk01281/JavaScript-Async와-Await)

<br>

**참고:**

[https://javascript.info/async-await](https://javascript.info/async-await)
[https://codeburst.io/javascript-es-2017-learn-async-await-by-example-48acc58bad65](https://codeburst.io/javascript-es-2017-learn-async-await-by-example-48acc58bad65)
[https://pouchdb.com/2015/03/05/taming-the-async-beast-with-es7.html](https://pouchdb.com/2015/03/05/taming-the-async-beast-with-es7.html)
