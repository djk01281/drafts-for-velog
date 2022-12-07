## Callback이 뭔가요?

*A callback function is a function passed into another function as an argument, which is then invoked inside the outer function to complete some kind of routine or action.* [MDN](https://developer.mozilla.org/en-US/docs/Glossary/Callback_function)

**그냥** 다른 function에 argument로 들어가서, 그 function에 의해 실행되면 콜백 함수다!

<br>

## Asynchronous한 함수는 실제로 어떻게 작동할까?

[https://youtu.be/8aGhZQkoFbQ](https://youtu.be/8aGhZQkoFbQ)

![](https://miro.medium.com/max/1400/1*iHhUyO4DliDwa6x_cO5E3A.gif)

쉽게 설명하자면 다음과 같다. (일단 오른쪽의 주황색 WebAPI는 무시하자.)

1. Stack에서 어떤 Function `Parent`가 실행이 되었는데 그 안에 **Async**한 어떤 Function `Child`가 있다고 하자. 
2. 실행되는 순간, `Child`는 일단 Callback Queue로 가게 된다. **Async하기 때문에 바로 실행될 수 없다.** `foo`는 그 부분을 빼고 다 실행했다면 바로 return한다. 즉, Stack에서 Pop된다. 
3. 나중에 **Stack 내부가** 다 Pop되서 **비게 되면** Callback Queue에서부터 하나씩 Stack에 와서 실행된다. 실행되서 Pop되면, Callback Queue의 다음 Function이 와서 또 실행되고 Pop된다.

2번에서 Async하기 때문에 바로 실행될 수 없다고 했는데 왜그럴까? 
바로 사용자가 있는 **브라우저 환경**이기 때문이다. 만약 전부 Synchronous하다면 오래 걸리는 Task가 진행 중일 때는 아무것도 할 수 없다. 브라우저가 Render되지도 않고, 클릭을 할 수도 없다.  

<br>

## Callback 함수는 다 asynchronous할까?

![https://media.giphy.com/media/mEV42F38lur6PbfapW/giphy.gif](https://media.giphy.com/media/mEV42F38lur6PbfapW/giphy.gif)

이상하다. 분명 Callback함수는 **비동기(Asynchronous)** 개념과 같이 소개되는 경우가 많다. 마치 모든 Callback Function이 Asynchronous한 것처럼. 근데 위의 정의에 따르면 꼭 그럴 것 같지는 않는데.. **어떻게 된 걸까?**

<br>

## Asynchronous한 Callback 함수

**A함수**와 **B함수**가 있다.

A함수에 B함수를 argument로 넘겨주고, A함수는 B함수를 실행하는 코드를 가지고 있다.

이 경우, **B함수가 Callback함수다**.

Asynchronous하게 실행되는 경우는 다음과 같다.

![https://res.cloudinary.com/practicaldev/image/fetch/s--5e204O-Y--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_880/https://dev-to-uploads.s3.amazonaws.com/i/rjqf7w2vmlxxz5ez0dbz.png](https://res.cloudinary.com/practicaldev/image/fetch/s--5e204O-Y--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_880/https://dev-to-uploads.s3.amazonaws.com/i/rjqf7w2vmlxxz5ez0dbz.png)

1. B함수를 argument로 가진 A함수가 실행된다.
2. A함수는 B함수를 **asynchronous 큐**에 넣어주고 바로 return한다.
3. 나중에 B함수가 실행되고, return한다.

`addEventListener`같은 경우가 대표적인 예시고, 어느 정도 익숙한 상황이다.

<br>

## Synchronous한 Callback 함수

Callback함수가 synchronous하려면 어떤 식으로 작동해야 할까?

![https://res.cloudinary.com/practicaldev/image/fetch/s--dqZaqp09--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_880/https://dev-to-uploads.s3.amazonaws.com/i/hyaxexxqnkl9ymxrjlh4.png](https://res.cloudinary.com/practicaldev/image/fetch/s--dqZaqp09--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_880/https://dev-to-uploads.s3.amazonaws.com/i/hyaxexxqnkl9ymxrjlh4.png)

1. B함수를 argument로 가진 A함수가 실행된다.
2. B함수가 실행되고, return한다.
3. A함수가 return한다.

대표적인 예시가 `forEach` 함수다.

```
const array1 = ['a', 'b', 'c'];

const someFunction = function(element){
console.log(element)
}

array1.forEach(element => someFunction(element));

// expected output: "a"
// expected output: "b"
// expected output: "c"

```

Array의 `forEach` method는 함수를 받아서, 배열의 원소 각각에 대해 실행한다. forEach 자체가 return하기 전에, argument로 받은 함수에 대한 call이 return하므로, **synchronous한 callback함수**다.

<br>

## 그래서..

Callback함수는 그냥 다른 함수에 argument로 들어가서 그 함수에 의해 불리는 function이다..!

Return하는 시점에 따라서 synchronous할 수도, asynchronous할 수도 있다.

**다만 asynchronous하게 작동할 수도 있다는 점 자체가 특이하기 때문에, 주목해야 하는 함수다.**

![https://media.tenor.com/BgPesLLHCBYAAAAd/kung-fu-conan-obrien.gif](https://media.tenor.com/BgPesLLHCBYAAAAd/kung-fu-conan-obrien.gif)

*<center>뭔 짓을 할지 모른다.</center>*

<br>

## Asynchronous한게 왜 문제가 될까?

일단 Asyncrhonous하게 작동하는 함수가 있다면, 대부분은 그럴만한 이유가 있다.

자바스크립트는 **Single-Thread**로 동작하기 때문에, 모든 게 순서대로(synchronously) 작동한다면 오래 걸리는 함수를 뒷부분의 모든 함수들이 계속 기다려야 한다.

따라서 Asynchronous하게 작동한다는 건 되게 좋은 점이다. 문제는 **콜백함수를 이용할 때는** 매우 헷갈린다는 데 있다. 즉, **흐름을 제어하기가 어렵다.**

<br>

## 헷갈린다.

Aynchronous한 함수는 **언제 끝날지 모른다.** 그런데 우리는 인간이기 때문에 순차적으로 사고해서 코드를 작성한다. 그러다보면 문제가 생길 수 있다.

예시가 와닿지 않을까봐 실제 겪었던 문제를 가져와봤다. 다음은 [카드게임](notion://www.notion.so/djk0128/%5B%3Chttps://github.com/djk01281/react-memory-card%3E%5D(%3Chttps://github.com/djk01281/react-memory-card%3E))을 만들면서 작성했던 React 코드의 일부다.

```
...

setScore(score + 1);

if (score > totalScore) {
      setTotalScore(score);
}

...

```

score를 1 증가시킨 후, 최고 점수보다 높다면 최고 점수를 score로 업데이트한다.

문제는 `setScore`나 `setTotalScore`가 **asynchronous**한 함수라는 점이다.

실제로 실행시켜보면 if문이 먼저 실행되기 때문에, 원하는대로 작동하지 않는다. (여기서는 React의 useEffect()를 사용해서 문제를 해결했다.)

즉, **순차적으로 실행되지 않는 함수를 이용해 순차적인 작업을 하고 싶을 때 문제가 된다.**

<br>

## 어떻게 해결할까?

**끝나는 걸 기다렸다가** 그 다음에 실행시키고 싶은 코드를 실행하면 된다.

Callback Function에서는 **callback 안에서 또 다른 callback을 부르는 방법**이 있다.

```
firstFunction(args, function() {
	//무언가를 한다(1)
  secondFunction(args, function() {
		//무언가를 한다(2)
    thirdFunction(args, function() {
			//무언가를 한다(3)
    })
  })
})

```

이렇게 하면  firstFunction, secondFunction, thirdFunction 순서로 실행된다. 첫 번째 함수가 두 번째 함수를 부르고, 두 번째 함수가 세 번째 함수를 부르기 때문이다.

하지만 이렇게 작성하는 방식의 문제점은 **코드가 매우 지저분해질 수 있다**는 점이다.

<br>

## 콜백 지옥, Callback Hell

![https://res.cloudinary.com/practicaldev/image/fetch/s--c0aEZX7m--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_880/https://dev-to-uploads.s3.amazonaws.com/uploads/articles/b8euo2n7twvgh3dbuatd.jpeg](https://res.cloudinary.com/practicaldev/image/fetch/s--c0aEZX7m--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_880/https://dev-to-uploads.s3.amazonaws.com/uploads/articles/b8euo2n7twvgh3dbuatd.jpeg)

*”콜백 지옥(Callback Hell)“*

Asynchronous한 코드를 순차적인 논리로 구성하고 싶기 때문에 **nested callback**을 쓰면서 생긴 문제였다. 코드가 매우 지저분해졌다. 

코드가 지저분한 게 다가 아니다. 한 function을 다른 function에 넣는 과정이 반복되다보니, **Error Handling**이 어렵다. 한 껍질에서 문제가 생기면, 두 번째 껍질에서는 도달할 수 없다. 많은 코드가 중첩되어 있는 상황에서 생길 수 있는 모든 경우의 에러를 처리하기는 쉽지 않다. 

지금까지 본 콜백 함수의 단점을 요약하면 다음과 같다. 

<br>

*흐름을 제어하기가 어렵다: 알아보기 어렵고, 에러 처리에도 문제가 있다.* 

<br>

## 주도권을 뺏겨버린 함수

사실 더 큰 문제가 남아있다. 함수를 만들어서, 콜백 함수로 사용하면서도 (즉, API같은 다른 Function에 넘겨주면서도) **믿을 수 없다(Trust Issues)**는 게 문제다. 

이 문제들은 모두 **Inversion Of Control** 때문에 발생한다. 즉, 함수를 넘겨주기 때문에 **주도권이 더 이상 그 함수에게 있지 않기 때문에** 문제가 된다. 

이렇게만 말하면 와닿지 않을 수 있으니 다음 경우를 살펴보자.

<br>

## 너무 일찍 호출되는 경우

Callback Function을 너무 빨리 호출버린다는 게 무슨 뜻일까? **Asynchronous한 줄 알았던 Function에 넘겨줬는데 알고보니 Synchronous한 경우다.**

다음과 같은 코드가 있다.

```jsx
let a = 0; //A

function printA(result){ //B
	console.log(a); 
}

syncOrAsync(printA); //C
a++; //D
```

synchronous할 지 asynchronous할지 모르는 `syncOrAsync` 함수가 있다. 여기에 `printA`라는 `a`를 출력하는 함수를 Callback function으로 넘겨준다. 

syncOrAsync가 **sync라면** A → C → B → D 순으로 0이 출력된다. 

반대로, **async라면** A → C → D → B 순으로 1이 출력된다.

우리가 함수를 Callback Function으로 어떤 API에 넘겨줄 때, **그 API가 synchronous하게 작동할 지, asyncrhonous하게 작동할 지 알 수 없다.** 

<br>

## 너무 일찍 호출되는 경우 - 해결법

Callback Function을 이용해서도 해결법은 있다. 
받는 함수가 sync할지, async할지 모른다면 넘겨주는 함수를 **async하게 만들어서** 넘겨주면 된다. 일반적으로는 `setTimeout`을 이용해서 0초를 기다리게 하는 부분을 추가한다. 즉, **무조건 async로 만든다.** 

 

```jsx
function printA(result){ 
	setTimeout(function(){console.log(a)}, 0);
}
```

이제 무조건 1이 출력된다.

<br>

## 이쯤이면 그냥 쓰지 않는게..

너무 늦게 호출되거나 호출되지 않는 경우, 또 에러 핸들링이 잘 안되는 등의 문제도 있다. 이것들에는 복잡하지만 어느 정도 해결할 수 있는 방법이 있다. 
다만 Callback Function이 API 내부에서 여러 번 실행되서 생기는 문제나, Callback Function에 파라미터를 제대로 넣어주지 않고 실행해버리는 경우는 Callback Function만으로는 해결할 수 없다.

 확실한 건 **문제가 많다는 점**이다. 여태까지 살펴 본 Callback Function의 문제들을 요약하면 다음과 같다.

<br>

*”흐름을 제어하기가 어렵다: 알아보기 어렵고, 에러 처리에도 문제가 있다”.*

*“뿐만 아니라, 함수의 주도권이 넘어가기 때문에 무슨 짓을 할 지 모른다.”*


![](https://media2.giphy.com/media/U5adYAzbjEVjHDr1Af/giphy.gif?cid=ecf05e477x8yknymhilens8o7hg0ywrdm9l6evts01r4w67h&rid=giphy.gif&ct=g)

이런 문제들을 해결하기 위해서 자바스크립트에서는 **Promise**라는 훨씬 좋은 기능을 제공한다. 이에 대해선 **[다음 글](https://velog.io/@djk01281/Promise-%EB%AD%98-%EC%95%BD%EC%86%8D%ED%95%9C%EB%8B%A4%EB%8A%94-%EA%B1%B0%EC%95%BC)**에서 알아보자.

<br>

<br>

**참고**[https://github.com/maxogden/art-of-node#callbacks](https://github.com/maxogden/art-of-node#callbacks)

[https://dev.to/marek/are-callbacks-always-asynchronous-bah](https://dev.to/marek/are-callbacks-always-asynchronous-bah)

[https://developer.mozilla.org/en-US/docs/Glossary/Callback_function](https://developer.mozilla.org/en-US/docs/Glossary/Callback_function)

[https://github.com/getify/You-Dont-Know-JS/tree/1st-ed](https://github.com/getify/You-Dont-Know-JS/tree/1st-ed)
