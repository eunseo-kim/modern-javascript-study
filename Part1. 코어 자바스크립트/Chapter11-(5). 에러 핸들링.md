# Chapter11-(5). 에러 핸들링

> 비동기 처리를 위한 **콜백 패턴**은 에러 처리가 곤란하다고 했었다🤦🏻‍♀️
> 그러나 **프로미스는 에러를 문제 없이 처리**할 수 있다!

## 🔍프로미스의 에러 처리

- 프로미스는 후속 처리 메서드 `then`, `catch`를 통해 에러를 처리할 수 있다!

1. **`then`의 두번째 콜백 함수로 에러 처리하기**

   ```js
   new Promise((resolve, reject) => {
     throw new Error("에러 발생!");
   }).then(
     (result) => console.log("성공!", result),
     (error) => console.log("에러!", error)
   ); // 에러! Error: 에러 발생!
   ```

2. **`catch`로 에러 처리하기**

   ```js
   new Promise((resolve, reject) => {
     throw new Error("에러 발생!");
   }).catch(
       (error) => console.log("에러!", error)
   ); // Error: 에러 발생!
   ```

> ### 🤔그렇다면 이 경우는? 
>
> ```js
> new Promise((resolve, reject) => {
>   resolve("성공~!");
> }).then(
>   (result) => console.xxx("성공!", result),		// console.xxx는 에러를 발생시킨다.
>   (error) => console.log("에러!", error)		// 위의 에러가 과연 여기에서 잡힐까????????????!!!!
> );
> ```
>
> 정답은  **`then`의 두번째 콜백함수는 첫번째 콜백 함수에서 발생한 에러를 캐치하지 못한다!** 이다.



### 💡따라서...

- 위의 코드는 이렇게 고칠 수 있다🤗

  ```js
  new Promise((resolve, reject) => {
    resolve("성공~!");
  })
    .then((result) => console.xxx("성공!", result))
    .catch((error) => console.log("에러!", error)); // 에러! TypeError: console.xxx is not a function
  ```

- `catch` 메서드를 모든 `then` 메서드를 호출한 이후에 호출하면 
  <u>비동기 처리에서 발생한 에러 + `then` 메서드 내부에서 발생한 에러</u>까지 모두 캐치할 수 있다!
- 그리고 `then` 메서드의 두 번째 콜백 함수에서 에러를 처리하는 것보다 **`catch` 메서드를 사용하는 것이 가독성이 좋고 명확하다~**
- **에러 처리는 `then` 보다 `catch` 에서 하는 것을 권장한다!** *(출처 - 모던 Javascript Deep Dive)* 

---



