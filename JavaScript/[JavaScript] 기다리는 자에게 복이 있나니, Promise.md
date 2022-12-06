[**저번 글**] (https://velog.io/@djk01281/Synchronous한-콜백함수도-있을까) 요약:

Callback function은 Synchronous할 수도, **Asynchronous**할 수도 있는데 후자의 경우가 문제가 된다.

Asynchronous한 callback function을 순차적으로 쓰고 싶으면 **nested** callback을 쓰면 된다.

하지만 이 방법은 **Callback Hell(콜백 지옥**)을 만들 수 있다.

<br>

## Promise, 뭐하는 거야?

다음과 같이 할 수 있게 해준다.

```jsx
const a = promiseFunction()

a.then(()=>console.log("Do Something"));
```

`promiseFunction`은 **promise**를 return하는 어떤 함수다.

<br>

그니까 **asyncrhonous한 함수가 promise를 return하게 만들면**, `.then` 뒤에 그 함수가 끝나면 실행할 코드를 넣을 수 있다. 순차적으로 실행할 수 있게 됐다!

<br>

순차적으로 실행해야하는 코드가 **많으면** 어떨까? Callback지옥같은 일이 또 생기는 건 아닐까..?

```jsx
const a = promiseFunction()

a.then(()=>console.log("first"))
.then(()=>console.log("second"))
.then(()=>console.log("third"))
.then(()=>console.log("fourth"))
```

**훨씬 깔끔해졌다!**

<br>

## 그래서 어떻게 만드는데?

“*asyncrhonous한 함수가 **promise를 return하게** 만들면”* 이라고 했는데, 어떻게 그렇게 만들 수 있을까?

2단계로 나눌 수 있다.
1. Promise를 만든다.
2. 함수가 Promise를 return하게 만든다.

일단 **첫 번째 단계**부터 해결하자.

```jsx
new Promise((resolve, reject) => {
//asynchronous한 무언가를 한다.
})
.then(
//그 다음에 무언가를 한다.
)
```

**Promise**키워드 앞에 **new**를 붙여서 만들면 된다!

근데 그 옆에 있는`resolve`와 `reject`는 뭐하는 아이들일까?

<br>

## promise 실행 과정

![](https://www.freecodecamp.org/news/content/images/2020/06/Ekran-Resmi-2020-06-06-12.21.27.png)

promise가 resolve될 경우에 아까 봤던 **.then()**을, reject될 경우에는 **.catch()**를 실행시켜준다.

*이게 뭔 소릴까?*

**그럼 언제 resolve되고 언제 reject된다는 걸까?** 

답은 **우리가 정해줄 때**다.

<br>

코드로 나타내면 다음과 같다.

```jsx
new Promise((resolve, reject)=>{
	//async한 무언가

	if(//잘 해결되었다고 믿을만한 조건){
		resolve(//return하고 싶은 것)
	}
	else{
		reject(//return하고 싶은 것)
	}
})
.then(//resolve에서 return한 값으로 무언가를 한다.)
.catch(//reject에서 return한 값으로 무언가를 한다.)

```

new Promise로 만든 이 함수도 무언가를 return하고 싶어할 것이다. 그걸 `resolve`나 `reject`안에 적어준다.

`.then`이나 `.catch`에서 이 값을 받아서 그 다음에 실행하고 싶은 코드를 실행해주면 된다.

<br>

**2번째 단계**였던, 우리의 함수가 Promise를 return하게 만드는 건 간단하다.

```jsx
function ourFunction(arg1, arg2){
    const workDone = someAsyncWork(arg1, arg2); //이 부분이 asynchronous한 작업이라고 가정하자.
    
    return new Promise((resolve, reject)=>{
        if(잘해결됐다면){resolve(workDone);}
        else{reject('무언가 잘못됐어!')}
    })
}

```
비동기 처리를 하고 Promise를 만들어서 return하면 된다!



## 이게 끝이야?

promise를 더 편하게 쓸 수 있게 해주는 기능들도 있다. 

그 중 하나는 resolve가 되든, reject가 되든 실행되는 `finally`다. 공통된 부분을 담아둘 수 있어 코드 반복을 피할 수 있다.

```jsx
new Promise((resolve, reject) => { 
//resolve, reject
 }))
.then(() => { console.log("success") })
.catch(() => { console.log("fail") })
.finally(res => { console.log("finally") });
```

<br>

또, **여러 promise를 동시에** 쓰고 싶을 때도 있을 거다. 

asynchronous한 코드를 여러 개 작동시키는데, **전부 다 끝나는 시점에 무언가를 하고 싶다면** 

```jsx
Promise.all([promise1, promise2]).then(function(results) {
	// Both promises resolved
})
.catch(function(error) {
	// One or more promises was rejected
});
```

배열에 넣어서 `Promise.all`로 거대한 promise를 만들어주자!

## 다음에는?

**async**와 **await**를 이용해 비동기처리를 할 수도 있다. 

다음 글에서는 그에 대해 더 알아보자.🍰

**참고:**

[https://davidwalsh.name/promises](https://davidwalsh.name/promises)

[https://www.freecodecamp.org/news/javascript-es6-promises-for-beginners-resolve-reject-and-chaining-explained/](https://www.freecodecamp.org/news/javascript-es6-promises-for-beginners-resolve-reject-and-chaining-explained/)
