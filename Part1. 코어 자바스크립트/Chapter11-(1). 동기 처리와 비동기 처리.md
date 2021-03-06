# Chapter11-(1). 동기 처리와 비동기 처리

> 자바스크립트는 **<u>비동기 처리</u>**를 위해 **콜백**과 **프로미스**를 사용한다.
> 근데 그 전에✋🏻 **동기와 비동기**에 대해 간단하게 정리하고 가자!



## 🔍동기 처리란 무엇일까?

- 예시로 먼저 이해해보자.

- 일정 시간이 경과한 뒤에 <u>콜백 함수</u> `alarm`을 호출하는 `sleep` 함수를 구현해보자. 그리고 `call` 함수도 추가로 구현해보자.

  > #### 💡복습
  >
  > <u>**콜백 함수**</u> : 함수의 인수로 함수를 전달하고, 필요하다면 인수로 전달한 그 함수를 "나중에 호출(called back)"하는 것!!

  ```js
  // sleep 함수는 일정 시간(delay)이 지난 후에 콜백 함수(alarm)를 호출한다.
  function sleep(alarm, delay) {
    const delayUntil = Date.now() + delay;
  
    // 일정시간이 지날때까지 while 문 반복
    while (Date.now() < delayUntil);
  
    // 일정시간이 지나면 콜백함수 alarm 실행
    alarm();
  }
  
  function alarm() {
    console.log("WAKE UP!!");
  }
  
  function call() {
    console.log("♪ Phone Call ♪");
  }
  
  // sleep 함수가 3초 실행된 이후에 alarm이 울린다.
  sleep(alarm, 3 * 1000);
  
  // call 함수는 sleep 함수 실행 이후, alarm 함수 실행 이후에 실행된다. => ✨동기✨
  call();
  ```

  > ### 출력
  >
  > ```
  > WAKE UP!!
  > ♪ Phone Call ♪
  > ```

  

- 위의 상황을 그림으로 나타내면 다음과 같다.

   ![image](https://user-images.githubusercontent.com/67737432/127824942-d879cb74-0682-4c33-9a5d-661cfde48dbd.png)



> ## 👏🏻즉, 동기 처리란
>
> **현재 실행중인 태스크가 종료할 때까지 다음에 실행될 태스크가 대기하는 방식**을 말한다!
>
> - 자바스크립트 엔진은 한 번에 하나의 태스크만 실행할 수 있는 <u>싱글 스레드 방식</u>이기 때문에
>   **동기 처리** 방식을 사용하게 되면 앞선 태스크가 종료될 때까지 **이후 태스크들이 `블로킹` 되는 단점**이 있다!
> - 물론 태스크들의 실행 순서가 보장된다는 장점도 존재한다.

---



## 🔍그렇다면 비동기 처리에 대해 알아보자!

- 비동기 처리는 동기 처리의 반대 개념으로 이해하면 쉽다.

- 즉, **실행중인 태스크가 종료되지 않은 상태라고 하더라도 대기하지 않고 다음 태스크를 곧바로 실행한다.**

- 위와 똑같은 예제를 이제 비동기 처리로 구현해보자.

  ```js
  function alarm() {
    console.log("WAKE UP!!");
  }
  
  function call() {
    console.log("♪ Phone Call ♪");
  }
  
  // 타이머 함수 setTimeout은 일정 시간이 경과한 이후에 콜백 함수 alarm을 호출한다.
  // 타이머 함수 setTimeout은 다음 태스크를 블로킹하지 않는다.
  setTimeout(alarm, 3 * 1000);
  
  // 3초가 지나지 않았지만 call이 실행된다.
  call();
  ```

  > ### 출력
  >
  > ```
  > ♪ Phone Call ♪
  > WAKE UP!!
  > ```

- 위의 상황을 그림으로 나타내면 다음과 같다.

   ![image](https://user-images.githubusercontent.com/67737432/127826963-fc5f9001-43f8-47ca-9b43-35acf20b3688.png)



> ## 👏🏻즉, 비동기 처리란
>
> **현재 실행 중인 태스크가 종료되지 않은 상태라 해도 다음 태스크를 곧바로 실행하는 방식**을 말한다.
>
> - 동기 처리와는 반대로 태스크의 실행 순서가 보장되지 않는 단점이 있지만, 블로킹이 발생하지 않는다는 장점이 있다.
> - 비동기 처리를 수행하는 비동기 함수는 전통적으로 콜백 함수를 사용해왔다. 
>   그.러.나 콜백 패턴은 여러 측면에서 보았을때 좋지 않은 방법이라고 하는데...🤔 이유는 다음장에서 알아보자!
> - 그래서 ES6에서는 콜백 함수 대신, 비동기 처리를 위한 또 다른 패턴으로 **✨프로미스✨**를 도입했다

