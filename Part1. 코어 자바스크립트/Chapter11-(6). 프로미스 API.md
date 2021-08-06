# Chapter11-(6). 프로미스 API

> Promise는 5가지 정적 메서드를 제공한다!

## 🔍1. Promise.all 

- `Promise.all`은 **여러개의 비동기 처리를 병렬적으로 처리**할 때 유용하게 사용된다!

  - **<u>병렬적</u>**으로 처리한다는 말은 쉽게 말해서... **`순차적`의 반대, 즉 '동시에 처리'한다는 말**로 이해하면 쉽다!!

  > ### 💡예시
  > 아래 코드는 여러개의 비동기 처리를 **'순차적'으로 처리하는 코드**이다. 
  > 이 코드를 **어떻게 병렬적으로 처리**하도록 바꿀 수 있을까? (아래 그림을 구현해보려고 한다.)
  >
  > |                      순차적(바꾸기 전)                       |                       병렬적(바꾼 후)                        |
  > | :----------------------------------------------------------: | :----------------------------------------------------------: |
  > | ![image](https://user-images.githubusercontent.com/67737432/128542770-758298f4-3e81-4fd3-b96c-9a9b6fab0b4a.png) | ![image](https://user-images.githubusercontent.com/67737432/128542799-d0bd282a-4814-4735-a9a5-63b5a8a18b9a.png) |

  ```js
  // 1초 뒤에 resolve(1)을 수행하는 프로미스
  const request1 = function () {
    return new Promise((resolve, reject) => setTimeout(() => resolve(1), 1000));
  };
  
  // 2초 뒤에 resolve(2)을 수행하는 프로미스
  const request2 = function () {
    return new Promise((resolve, reject) => setTimeout(() => resolve(2), 2000));
  };
  
  // 3초 뒤에 resolve(3)을 수행하는 프로미스
  const request3 = function () {
    return new Promise((resolve, reject) => setTimeout(() => resolve(3), 3000));
  };
  
  // 3개의 request를 순차적으로 처리한다.
  const result = [];
  request1()
    .then(function (data) {
      result.push(data);
      return request2();
    })
    .then(function (data) {
      result.push(data);
      return request3();
    })
    .then(function (data) {
      result.push(data);
      console.log(result);//✨6초 뒤에 [1, 2, 3]이 출력된다✨
    });
  ```
  
  > 🌼순차적으로 실행한 결과
  >
  > ![image](https://user-images.githubusercontent.com/67737432/128541935-a74375fd-de8d-44da-934e-1b019465b5ef.png)
  > `6초`가 걸렸다! (1초 + 2초 + 3초)
  >
> - 그런데 이렇게 순차적으로 처리하는 것 말고 **"병렬적" 으로 처리하고 싶다면**? → `Promise.all`

  

  ### 💡Promise.all로 병렬적으로 처리하기

  - `Promise.all` 메소드는 인수로 배열 등의 이터러블을 전달받는다!

  - 그리고 **<u>전달받은 프로미스가 모두 `fulfilled`(성공) 상태가 되면 모든 처리 결과를 배열에 저장해 새로운 프로미스를 반환</u>**한다.

  - 코드로 이해해보자👏

    ```js
    // 아까와 동일한 request1, request2, request3을 사용했다고 하자.
    // 3개의 비동기 처리를 병렬로 처리한다.
    Promise.all([request1(), request2(), request3()])
    .then(console.log); // ✨[1, 2, 3]이 3초 뒤에 출력됨✨
    ```

  >  🌼병렬적으로 실행한 결과
  >
  >    -  ![image](https://user-images.githubusercontent.com/67737432/128544969-0a71c967-f124-44b6-bf9b-ee47c37c05b8.png)

- 중요한 점은, **`Promise.all`에 전달되는 프라미스 중 하나라도 거부되면, **
  **`Promise.all`이 반환하는 프라미스는 에러와 함께 바로 거부된다는 점**이다!

  ```js
  Promise.all([
    new Promise((resolve, reject) => setTimeout(() => reject(new Error("에러 발생!")), 10000)),
    new Promise((resolve, reject) => setTimeout(() => resolve(2), 20000)),
    new Promise((resolve, reject) => setTimeout(() => resolve(3), 30000)),
  ]).catch(console.log); // Error: 에러 발생!
  ```

  - ✋🏻그러나 **프로미스에는 `취소`라는 개념이 없어서 에러를 출력해도 마지막 프로미스까지의 호출은 그대로** 일어난다.
    **그러나 그 결과는 무시된다.**
  - 즉, 위의 경우에서는 `20초`에 `Error: 에러 발생!`이 출력되지만, 실제로 **모든 호출이 끝나는 시간은 그대로 `30초`**이다. 
  -     ![image](https://user-images.githubusercontent.com/67737432/128547595-999f0598-b13c-456d-8886-6106d0267fdf.png)

---

## 🔍Promise.allSettled

> ES11(ECMA Script 2020)에 도입된 내용으로, settled의 뜻을 미리 알고 가면 쉽다.
>
> 📌`settled` = **<u>비동기 처리가 수행된 상태</u>**를 의미한다. 즉, `fulfilled`/`rejected` 중 한가지 상태이다.

- **`Promise.allSettled` = 모든 프로미스가 `settled` 상태가 될 때 처리 결과를 배열로 반환하는 메서드**

- 아까 `Promise.all`에서 사용했던 예시를 다시 출력해보면...

  ```js
  Promise.allSettled([
    new Promise((resolve, reject) => setTimeout(() => reject(new Error("에러 발생!")), 1000)),
    new Promise((resolve, reject) => setTimeout(() => resolve(2), 2000)),
    new Promise((resolve, reject) => setTimeout(() => resolve(3), 3000)),
  ]).then(console.log);
  ```

  > ### 출력
  >
  > - 3초 뒤에 결과가 출력된다
  >
  > ```
  > [
  >   { status: 'rejected', reason: Error: 에러 발생! at <anonymous>:3:60 },
  >   { status: 'fulfilled', value: 2 },
  >   { status: 'fulfilled', value: 3 }
  > ]
  > ```

- 즉, 모든 프로미스가 settled 상태가 될때까지 기다렸다가, 3초뒤에 결과를 출력한다.

  - 응답이 성공한 경우 – `{status:"fulfilled", value:result}`

  - 에러가 발생한 경우 – `{status:"rejected", reason:error}`

- ⭐프라미스가 앞에서 하나라도 거절되면 전체를 거절(결과를 무시하는)하는 **`Promise.all`과는 차이가 있는 문법**이다!!

---

## 🔍Promise.race, Promise.resolve & Promise.reject

> 나머지는 간단한것들!

### 1. Promise.race🏁🏎️

`Promise.all`과 비슷하지만, 가장 먼저 처리되는 프로미스의 결과(또는 에러)를 반환한다.

```js
Promise.race([
  new Promise((resolve, reject) => setTimeout(() => reject(new Error("에러 발생!")), 2000)),
  new Promise((resolve, reject) => setTimeout(() => resolve(3), 3000)),
  new Promise((resolve, reject) => setTimeout(() => resolve(1), 1000)),
]).then(console.log); // 1 
//(3번째 프로미스가 가장 빨리 처리되었다.)
```

### 2. Promise.resolve

인수로 전달받은 값을 `resolve`하는 프로미스를 생성한다.

```js
const resolvedPromise = Promise.resolve([1, 2, 3]);
resolvedPromise.then(console.log);	// [1, 2, 3]
// resolvedPromise는 fulfilled 상태이므로 then은 [1, 2, 3]을 출력한다.
```

-   ![image](https://user-images.githubusercontent.com/67737432/128550263-9940f527-631b-45e7-90ef-b7012e91b8c7.png)

### 3. Promise.reject

인수로 전달받은 값을 `reject`하는 프로미스를 생성한다.

```js
const rejectedPromise = Promise.reject(new Error('Error!'));
rejectedPromise.catch(console.log);	// Error: Error!
```

-   ![image](https://user-images.githubusercontent.com/67737432/128550478-8f04a35c-857c-44c2-bee0-32cc793420dc.png)

---

