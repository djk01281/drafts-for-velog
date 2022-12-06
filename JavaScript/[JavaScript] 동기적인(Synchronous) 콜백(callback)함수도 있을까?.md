## Callback이 뭔가요?

_A callback function is a function passed into another function as an argument, which is then invoked inside the outer function to complete some kind of routine or action._ [MDN](https://developer.mozilla.org/en-US/docs/Glossary/Callback_function)

**그냥** 다른 function에 argument로 들어가서, 그 function에 의해 실행되면 콜백 함수다! 

<br>

## Callback 함수는 다 asynchronous할까?

![](https://media.giphy.com/media/mEV42F38lur6PbfapW/giphy.gif)

이상하다. 분명 Callback함수는 **비동기(Asynchronous)** 개념과 같이 소개되는 경우가 많다. 마치 모든 Callback Function이 Asynchronous한 것처럼. 근데 위의 정의에 따르면 꼭 그럴 것 같지는 않는데.. **어떻게 된 걸까?**

<br>

## Asynchronous한 Callback 함수

**A함수**와 **B함수**가 있다. 

A함수에 B함수를 argument로 넘겨주고, A함수는 B함수를 실행하는 코드를 가지고 있다. 

이 경우, **B함수가 Callback함수다**.  

Asynchronous하게 실행되는 경우는 다음과 같다. 

![](https://res.cloudinary.com/practicaldev/image/fetch/s--5e204O-Y--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_880/https://dev-to-uploads.s3.amazonaws.com/i/rjqf7w2vmlxxz5ez0dbz.png)

 

1. B함수를 argument로 가진 A함수가 실행된다.
2. A함수는 B함수를 **asynchronous 큐**에 넣어주고 바로 return한다. 
3. 나중에 B함수가 실행되고, return한다. 

`addEventListener`같은 경우가 대표적인 예시고, 어느 정도 익숙한 상황이다.

<br>

## Synchronous한 Callback 함수

Callback함수가 synchronous하려면 어떤 식으로 작동해야 할까?

![](https://res.cloudinary.com/practicaldev/image/fetch/s--dqZaqp09--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_880/https://dev-to-uploads.s3.amazonaws.com/i/hyaxexxqnkl9ymxrjlh4.png)

1. B함수를 argument로 가진 A함수가 실행된다.
2. B함수가 실행되고, return한다.
3. A함수가 return한다. 

대표적인 예시가 `forEach` 함수다. 

```jsx
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

## 결론

Callback함수는 그냥 다른 함수에 argument로 들어가서 그 함수에 의해 불리는 function이다..!

Return하는 시점에 따라서 synchronous할 수도, asynchronous할 수도 있다.

**다만 asynchronous하게 작동할 수도 있다는 점 자체가 특이하기 때문에, 주목해야 하는 함수다.**

![](https://media.tenor.com/BgPesLLHCBYAAAAd/kung-fu-conan-obrien.gif)

_<center>뭔 짓을 할지 모른다.</center>_

<br>

## Asynchronous한게 왜 문제가 될까?

일단 Asyncrhonous하게 작동하는 함수가 있다면, 대부분은 그럴만한 이유가 있다. 

자바스크립트는 **Single-Thread**로 동작하기 때문에, 모든 게 순서대로(synchronously) 작동한다면 오래 걸리는 함수를 뒷부분의 모든 함수들이 계속 기다려야 한다.

따라서 Asynchronous하게 작동한다는 건 되게 좋은 점이다. **문제는 헷갈린다는 데 있다**.

<br>

## 왜 헷갈릴까?

Aynchronous한 함수는 **언제 끝날지 모른다.** 그런데 우리는 인간이기 때문에 순차적으로 사고해서 코드를 작성한다. 그러다보면 문제가 생길 수 있다. 

예시가 와닿지 않을까봐 실제 겪었던 문제를 가져와봤다. 다음은 [카드게임]([https://github.com/djk01281/react-memory-card](https://github.com/djk01281/react-memory-card))을 만들면서 작성했던 React 코드의 일부다.

```jsx
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

asynchronous한 함수가 **끝나는 걸 기다렸다가** 그 다음에 실행시키고 싶은 코드를 실행하면 된다. 

일단은 **callback함수 안에서 또 다른 callback을 부르는 방법**이 있다.

```jsx
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

![](https://res.cloudinary.com/practicaldev/image/fetch/s--c0aEZX7m--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_880/https://dev-to-uploads.s3.amazonaws.com/uploads/articles/b8euo2n7twvgh3dbuatd.jpeg)

이렇게 지저분해진 코드를 **콜백 지옥(Callback Hell)**이라고 한다. 

Asynchronous한 코드를 순차적인 논리로 구성하고 싶기 때문에 **nested callback**을 쓰면서 생긴 문제였다.

이걸 해결하기 위해서 자바스크립트에서는 **promise**와 **async/await**라는 기능을 제공한다. 이에 대해선 다음에 알아보자. [**다음 글**](https://velog.io/@djk01281/Promise-뭘-약속한다는-거야) 

**참고** 
[https://github.com/maxogden/art-of-node#callbacks](https://github.com/maxogden/art-of-node#callbacks)

[https://dev.to/marek/are-callbacks-always-asynchronous-bah](https://dev.to/marek/are-callbacks-always-asynchronous-bah)

[https://developer.mozilla.org/en-US/docs/Glossary/Callback_function](https://developer.mozilla.org/en-US/docs/Glossary/Callback_function)
