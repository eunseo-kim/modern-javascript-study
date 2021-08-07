# Chapter11-(9). async와 await

> `async`, `await`을 사용하면 비동기 프로그래밍을 쉽게 구현할 수 있다!

## 🔍먼저 async 함수에 대해 알아보자

- 다음과 같은 코드가 있다고 하자. 특별할게 없는 일반적인 함수 코드이다!

  ```js
  function hello() { return "Hello" };
  console.log(hello());	// Hello
  ```

- 그런데 여기에 `async`라는 키워드를 함수 앞에 추가하면?

  ```js
  async function hello() { return "Hello" };
  console.log(hello());	// Promise { 'Hello' } ✨프로미스를 반환한다!✨
  ```

> ### 👏🏻따라서 async 함수는,
>
> 암묵적으로 반환값을 resolve하는 **<u>프로미스를 반환</u>**한다!

---

## 🔍await 키워드에 대해서도 알아보자!

- `await` 키워드는 프로미스가 `settled` 상태(비동기 처리가 수행된 `fulfilled/rejected` 상태)가 될 때 까지 <u>**대기**</u>하다가,
  **settled 상태가 되면 프로미스가 resolve한 처리 결과를 반환**한다!

- 따라서 `await` 키워드는 **<u>반드시 프로미스 앞에서 사용</u>**해야 된다!

- 무슨 말인지 코드로 이해해보자 :)

  ```js
  async function foo() {
    const a = await new Promise((resolve) => setTimeout(() => resolve(1), 1000));
    const b = await new Promise((resolve) => setTimeout(() => resolve(2), 2000));
    const c = await new Promise((resolve) => setTimeout(() => resolve(3), 3000));
  
    console.log([a, b, c]);
  }
  
  foo();	// 6초 후에 [1, 2, 3] 출력됨
  ```

  > 만약 `await` 키워드를 사용하지 않는다면..?
  >
  > ```js
  > async function foo() {
  >   const a = new Promise((resolve) => setTimeout(() => resolve(1), 1000));
  >   const b = new Promise((resolve) => setTimeout(() => resolve(2), 2000));
  >   const c = new Promise((resolve) => setTimeout(() => resolve(3), 3000));
  > 
  >   console.log([a, b, c]);
  > }
  > 
  > foo();	// [ Promise { <pending> }, Promise { <pending> }, Promise { <pending> } ] 출력됨
  > ```
  >
  > - 여기서 `pending`은 맨 처음에 설명했듯이, 비동기 처리가 되기 전 상태(**보류 중 상태**)이다!
  >   즉, await 키워드를 사용하지 않으면 **프로미스의 상태가 settled가 될때까지 기다려주지 않는다!**



- `await`은 어떻게 보면 `promise.then`과 같은 역할을 하지만, 
  `then`보다 좀 더 가독성 좋고 쓰기 쉽게 프로미스를 처리할 수 있는 키워드이다👏🏻👏🏻



> ### await은 async function과 함께🤝🏻
>
> - `await` 만 따로 사용할 수 있을까?
>
> ```js
> function foo() {	// 일반적인 함수 안에서 await 키워드를 사용해보았다.
>   const a = await new Promise((resolve) => setTimeout(() => resolve(1), 1000));
>   const b = await new Promise((resolve) => setTimeout(() => resolve(2), 2000));
>   const c = await new Promise((resolve) => setTimeout(() => resolve(3), 3000));
> 
>   console.log([a, b, c]);
> }
> 
> foo();	// SyntaxError: await is only valid in async function
> ```
>
> - `await`은 async function 안에서만 사용가능하다는 Syntax error이 발생한다.
> - 그런데 `await` 혼자 사용하고 싶은 경우도 분명 있을텐데... 아래 상황을 보자
>
> ```js
> // 최상위 레벨 코드에서 await을 사용해야 한다. 최상위 레벨 코드란, 감싸고 있는 블록이 없는 상태!
> let response = await fetch('/article/promise-chaining/user.json');	
> // 서버로부터 응답을 받으면 response에 응답을 저장하는 코드
> ```
>
> - 이런 경우에는 **익명 `async` 함수로 코드를 감싸**주면 된다!
>
> ```js
> (async () => {
>   let response = await fetch("/article/promise-chaining/user.json");
> })();
> ```



---

## 🔍async/await 에러 핸들링

> 에러 처리 간단하게만 설명하자면..
>
> #### 💡try…catch…finally
>
> ```js
> try {
>    ... 에러 발생할 가능성이 있는 코드 실행 ...
> } catch(error) {
>    ... 에러 핸들링 ...
> } finally {
>    ... 에러와 상관 없이 항상 실행 ...
> }
> ```
>
> #### 💡throw 문
>
> 에러를 발생시키려면 단순히 에러 객체를 생성하면 되는게 아니라, 
> `try` 코드 블록에서 `throw`문으로 에러 객체를 **던져야** 한다!
>
> ```js
> try {
>   // new Error("wrong");    // 만들기만 하면 catch에서 잡을 수 없다.
>   throw new Error("wrong"); // throw로 에러 객체를 '던져야' 잡을 수 있다!
> } catch (e) {
>   console.log(e);           // Error: wrong
> }
> ```



#### 📌await Promise~가 거부된 경우

- 그렇다면... 만약 `await Promise~`를 실행했는데, 프로미스가 거부(rejected)되면 어떻게 될까? 

  ```js
  async function f() {
    await Promise.reject(new Error("에러 발생!"));
  }
  ```

- 이 경우, 마치 `throw` 문으로 에러를 던진 것과 같은 코드가 실행된다!

  ```js
  // 위의 코드와 결과가 동일하다.
  async function f() {
    throw new Error("에러 발생!");
  }
  ```



#### 📌await이 던진 에러 잡기

- await이 던진 에러는, `try...catch`를 사용해서 잡을 수 있다!
  왜냐하면 await에서 프로미스가 거부된 경우, 자동으로 `throw` 한 것 같은 결과를 주기 때문이다!

  ```js
  async function f() {
  
    try {
      let response = await fetch('http://유효하지-않은-주소');
    } catch(err) {
      alert(err); // TypeError: failed to fetch
    }
  }
  
  f();
  ```

- 그렇다면 만약.. async 함수 안에서  `try...catch`로 에러를 잡지 못한 경우에는?????

  ```js
  async function f() {
    let response = await fetch('http://유효하지-않은-url');
  }
  ```

- **만약 async 함수 내에서 catch 문을 사용해서 에러 처리를 하지 않으면,**
  **async 함수는 발생한 에러를 reject 하는 프로미스를 반환하게 된다!**

  즉, 이는 `.catch` 후속 처리 메서드를 사용해서 에러를 처리해줄 수 있다.

  ```js
  async function f() {
    let response = await fetch("http://유효하지-않은-url");
  }
  
  f().catch((error) => console.log("error!:", error)); // error!: TypeError: Failed to fetch
  ```

  

