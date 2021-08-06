# Chapter11-(2). 비동기 처리를 위한 콜백 패턴

## 🔍콜백으로 비동기 처리하기

- 스크립트나 모듈을 로딩하는 것은 비동기 동작이다. 
  먼저 다음과 같이 스크립트를 읽어올 수 있는 함수를 만들어보자

  ```js
  function loadScript(src) {
    let script = document.createElement('script');
    script.src = src;
    document.head.append(script);
  }
  
  // 해당 경로에 위치한 스크립트를 불러오고 실행함
  loadScript('/my/script.js');
  ```

  > 🙂 참고로... 이런 결과를 가져오는 코드이다.
  >
  >     ![image](https://user-images.githubusercontent.com/67737432/128356631-0a1e2d47-27ba-4eda-a81c-27703058e2ca.png)

- 이렇게 읽어온 `my/script.js` 에 대하여, `script.js`를 다음과 같이 정의해보았다.

  ```js
  // script.js는 다음과 같다.
  function newFunction() {
    alert("Hi~!");
  }
  ```

- 그렇다면 이제 다시 처음 코드로 돌아가서, `newFunction`을 호출해보자!

  ```js
  function loadScript(src) {
    let script = document.createElement('script');
    script.src = src;
    document.head.append(script);
  }
  
  loadScript('/my/script.js');
  newFunction();	// script.js에 있는 함수를 호출할 수 있을까?
  ```

- 결과는...😨

     ![image](https://user-images.githubusercontent.com/67737432/128356617-a73ab9fd-aa46-4393-b833-7b887701e78e.png)

---

### 😮 왜 제대로 실행이 되지 않을까?

- 바로 맨 처음에 말했듯이, **스크립트나 모듈을 로딩하는 것은 비동기 동작**이기 때문이다!

- 비동기 동작은 **현재 실행 중인 태스크가 종료되지 않은 상태라 해도 다음 태스크를 곧바로 실행하는 방식**이라고 했었다.

- 따라서, `loadScript`가 완전히 종료되지 않아 아직 스크립트를 읽어오지 않은 상태임에도,
  곧바로 `newFunction()`을 호출했기 때문에 `is not defined` 에러가 뜨는 것이다.

- 즉, 그림으로 나타내보면...

  ![image](https://user-images.githubusercontent.com/67737432/128357707-a2675dc1-a4c7-4a82-9109-b2ba745b434c.png)

---

### 💡콜백으로 에러 고치기

- 콜백은 **나중에 호출할 함수**를 의미한다고 했다! 
- 따라서 위의 문제는, **스크립트의 로딩이 끝나는 시점에 콜백 함수를 실행하고,** 
  **콜백 함수 안에서 `newFunction`을 호출**하면 위의 문제가 쉽게 해결될 것 같다🤗

- 코드로 이해해보자.

  ```js
  function loadScript(src, callback) {
    let script = document.createElement("script");
    script.src = src;
  
    // 스크립트의 로딩이 끝나면 콜백함수를 실행한다.
    script.onload = () => callback();
  
    document.head.append(script);
  }
  
  // 스크립트 로딩 후 script.js의 newFunction() 실행하기
  loadScript("/my/script.js", function () {
    newFunction();
  });
  ```

- 이제 실제로 잘 된다!!

  ![image](https://user-images.githubusercontent.com/67737432/128361376-064489e8-60ca-4cd3-8a6b-09c04058fe73.png)

---

## 🔍그런데 왜 콜백을 사용하지 말라는 걸까?

### 👿콜백 지옥 맛보기👿

- 만약 **비동기 동작이 꼬리에 꼬리를 물게 된다**면...?

- 예를 들어서 다음과 같은 상황이 있다고 가정해보자

  1. `1.js` 로드 후 에러가 없다면

  2. `2.js` 로드 후 에러가 없다면
  3. `3.js` 로드 후 에러가 없다면
  4. `4.js` 로드 후 (...)

- 이러한 상황을 **콜백 함수로 구현**해보면 다음과 같을 것이다! 
  (스크립트를 읽어오는 함수는 위의 예시 그대로 `loadScript`를 사용하겠다)

  ```js
  function loadScript(src, callback) {
    let script = document.createElement("script");
    script.src = src;
  
    // 에러 처리 추가
    script.onload = () => callback(null, script);
    script.onerror = () => callback(new Error("error!"), script);
  
    document.head.append(script);
  }
  
  // 정말 끔찍한 코드!!😱😱😱
  loadScript("1.js", function (error, script) {
    if (error) { // 에러 처리
      alert(error);
    } else { // 에러가 없다면 2.js 읽어오기...
      loadScript("2.js", function (error, script) {
        if (error) { // 에러 처리
          alert(error);
        } else { // 에러가 없다면 3.js 읽어오기...
          loadScript("3.js", function (error, script) {
            if (error) {
              alert(error);
            } else {
              loadScript("4.js", function (error, script) {
                if (error) {
                  alert(error);
                } else {
                  // ...
                }
              });
            }
          });
        }
      });
    }
  });
  ```

- 비동기 동작이 하나씩 추가될때마다 중첩 호출이 만들어내는 피라미드가 오른쪽으로 점점 커진다...😱

> ### 즉, 콜백 헬(콜백 지옥)은..👿
>
> **가독성을 나쁘게** 하며 **실수를 유발하는 원인**이 된다! 
> 또한 **에러를 처리**하는 것도 매우 어려워진다..

---

### 😮어떻게 해결할 수 있나?

- 해답은 바로 이번 챕터의 주제인 **프로미스(promise)**에 있다!
- 지금부터는 본격적으로 프로미스(promise)에 대해 이해해보자👏🏻👏🏻

