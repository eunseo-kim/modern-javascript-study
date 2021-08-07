# Chapter11-(3). 프로미스(promise)

## 🔍프로미스 만들기

- 프로미스 객체는  `Promise` 라는 표준 빌트인 객체를 통해 생성할 수 있다.

- `Promise` 생성자 함수는 비동기 처리를 수행할 **콜백 함수**를 인수로 전달받는데, 
  이 **콜백 함수**는 **`resolve`와 `reject`** 라는 함수를 또 인수로 전달받는다!

- 즉, 지금까지의 내용으로 프로미스 객체를 만들어보면 다음과 같다👏🏻👏🏻

  ```js
  let promise = new Promise(function(resolve, reject) {...})
  ```

---

## 🔍resolve, reject?

> 🤔그렇다면 이 `resolve`, `reject`에는 어떤 함수를 전달하면 될까?
> 정답은, **우리가 신경쓰지 않아도 된다!**이다.
>
> `resolve`, `reject`는 **자바스크립트에서 자체적으로 제공하는 함수**이다.
> 그렇다면 자바스크립트에서 왜 `resolve`, `reject`를 자체적으로 제공하는지 알아보자.

#### 📌먼저, **<u>프로미스의 3가지 상태 정보</u>**에 대해 알아야 한다!

| 프로미스의 상태 정보 |                 의미                  |          상태 변경 조건          |
| :------------------: | :-----------------------------------: | :------------------------------: |
|      `pending`       | 비동기 처리가 아직 수행되지 않은 상태 | 프로미스가 생성된 직후 기본 상태 |
|     `fulfilled`      | 비동기 처리가 수행된 상태 **(성공)**  |     **`resolve` 함수 호출**      |
|      `rejected`      | 비동기 처리가 수행된 상태 **(실패)**  |      **`reject` 함수 호출**      |

- 즉, 생성된 직후의 프로미스는 기본적으로 `pending` 상태이지만

- 비동기 처리가 수행된 이후 **성공**했다면
  - `resolve` 함수를 호출해 프로미스를 `fulfilled` 상태로 변경하고
- 비동기 처리가 수행됐지만 **실패**했다면
  - `reject` 함수를 호출해 프로미스를 `rejected` 상태로 변경한다.

 ![image](https://user-images.githubusercontent.com/67737432/128383270-02f242b8-2da5-437b-bb36-389510109006.png)

> ### 🙂즉,
>
> `resolve`, `reject` 함수를 호출함으로써 **프로미스의 상태를 결정**할 수 있다!

---

## 🔍 resolve(value), reject(error)

-  `resolve(value)`, `reject(error)` 의 동작에 대해 좀 더 자세히 알아보자면..

-  `resolve(value)`, `reject(error)`는 프로미스의 비동기 처리 상태(`state`)와 더불어 
  비동기 처리 결과(위의 그림에서 `result`) 정보도 갖는다. 

- 이를 실제로 개발자 도구로 출력해보면...

  - `resolve` 출력

    ![image](https://user-images.githubusercontent.com/67737432/128386688-cd14600c-58b0-41e6-ae86-bb8f935daba7.png)

  ---

  - `reject` 출력

    ![image](https://user-images.githubusercontent.com/67737432/128388141-cfb25d4d-60b5-4b08-b34f-d6af471b9464.png)



---

## 🔍프로미스 후속 처리 메소드 : then, catch, finally

> 프로미스의 비동기 처리 상태가 변화하면 이에 따른 **후속 처리**를 해줘야 된다!
>
> 예를 들어서, 비동기 처리에 성공하여 `fulfilled` 상태가 되면 처리 결과를 가지고 무언가 해야 되고,
> 비동기 처리에 실패하여 `rejected` 상태가 되면 에러를 가지고 에러 처리를 해야 된다
>
> 이를 위해 **프로미스는 후속 처리 메서드 `then`, `catch`, `finally`** 를 제공한다

### 💡`then(성공했을 때 실행되는 함수, 실패했을 때 실행되는 함수)`

- 성공했을 때 실행되는 함수의 인수 → 프로미스의 비동기 처리 결과(`result`)

- 실패했을 때 실행되는 함수의 인수 → 프로미스의 에러(`error`)

- 예시를 보면...

  ```js
  // fulfilled
  new Promise((resolve, reject) => resolve("OK")).then(
  	result => console.log(result),
      error => console.log(error)
  ); // OK
  ```

  ```js
  // rejected
  new Promise((resolve, reject) => reject(new Error("error!"))).then(
  	result => console.log(result),
      error => console.log(error)
  ); // Error: error! ...
  ```



### 💡`catch(실패했을 때 실행되는 함수)`

- 에러가 발생한 경우만 다루고 싶다면 `.catch`를 사용하면 된다!

- 사실상 `then(undefined, onRejected)`와 동일하게 동작하므로 위의 예시와 거의 비슷하다🤗

  ```js
  new Promise((resolve, reject) => reject(new Error("error!"))).catch((error) => console.log(error));
  // Error: error!
  ```



### 💡`finally(프로미스의 상태와 상관없이 무조건 수행하는 함수)`

- `fulfilled`, `rejected`와 상관없이 무조건 실행된다!

  ```js
  // fulfilled
  new Promise((resolve, reject) => resolve("OK"))
  	.finally(() => console.log("finally..."))
      .then(result => console.log(result)
  );
  // finally...
  // OK
  ```

  ```js
  new Promise((resolve, reject) => reject(new Error("error!!")))
      .finally(() => console.log("finally..."))
      .then((result) => console.log(result)
  );
  // finally...
  // Error: error!!
  ```




> ### then, catch, finally의 반환값
>
> 은 언제나 **프로미스**이다!



---

## 🔍🤷🏻‍♂️그래서 프로미스로 비동기를 어떻게 처리한다는 것일까?🤷🏻‍♀️

### 💡앞에서 콜백으로 처리했던 loadScript를 다시 가져와보자...🤔

- 콜백으로 작성한 `loadScript`

	```js
	function loadScript(src, callback) {
	    let script = document.createElement("script");
	    script.src = src;

	    script.onload = () => callback(null, script);
	    script.onerror = () => callback(new Error("error!"), script);

	    document.head.append(script);
	}


- 프로미스로 바꾸기

  ```js
  function loadScript(src) {
      return new Promise(function(resolve, reject) {
          let script = document.createElement("script");
    		script.src = src;
          
          script.onload = () => resolve(script);
          script.onerror = () => reject(new Error("error!"));
          
          document.head.append(script);
      })
  }
  
  let promise = loadScript("../my/script.js");
  
  // 프로미스의 상태에 따라 다르게 동작하는 .then을 이용하니까 훨씬 자연스럽다!
  promise.then(
  	script => console.log(`script`를 불러왔습니다.),
      error => console.log(error)
  );
  
  // .then을 얼마든지 추가하여 원하는 또다른 핸들러를 사용할 수 있다!
  promise.then(
  	script => alert("성~공!"),
  );
  ```

  > 잘 동작한다.
  >
  > ![image](https://user-images.githubusercontent.com/67737432/128393219-005ce192-cfbe-44eb-8221-acde0f0a6440.png)

