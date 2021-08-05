# Chapter11-(4). 프로미스 체이닝

> 아직 해결하지 않은 한가지 문제가 남아있다!
> 바로 👿**콜백 지옥**👿 문제인데...
>
> 이를 해결하기 위해서는 **프로미스 체이닝**에 대해 이해해야 한다🔗

### 🔍프로미스 체이닝 예제 (setTimeout() 비동기 API를 사용한 예제)

```js
new Promise(function (resolve, reject) {
  // 2초 후에 resolve(1) 호출
  setTimeout(function () {
    resolve(1);
  }, 2000);
})
  .then(function (result) {	 // 💡then의 첫번째 인수는 성공했을 때 실행되는 함수이다.
    console.log(result);     // 1 💡result는 프로미스의 비동기 처리 결과(즉, resolve(1)에서 1)
    return result * 2;		 // 실제로 반환되는 값은 resolve(2)로, 프로미스를 반환한다.
  })
  .then(function (result) {
    console.log(result); // 2
    return result * 2;
  })
  .then(function (result) {
    console.log(result); // 4
  });

```

- `then`, `catch`, `finally`의 **반환값** 또한 **프로미스**이므로 **체이닝이 가능**하다!

- 위의 예제에서 `return result + 10;` 과 같이 함수가 프로미스가 아닌 값을 반환할 수도 있지만
  **`.then`은 그 값을 암묵적으로 `resolve`(또는 `reject`) 하여 프로미스를 생성해 반환**하게 된다.

   ![image](https://user-images.githubusercontent.com/67737432/128402186-606ace38-3275-43fd-b5e0-fe42c73d2a99.png)

---

## 🔍👿콜백 지옥 개선하기👼

### 끔찍한 코드를 다시 가져와보면...

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

- 이제는 프로미스의 **`.then`**을 이용하여 간단하게 바꿀 수 있다!

```js
function loadScript(src) {
  return new Promise(function (resolve, reject) {
    let script = document.createElement("script");
    script.src = src;

    script.onload = () => resolve(script);
    script.onerror = () => reject(new Error("error!"));

    document.head.append(script);
  });
}

loadScript("../my/1.js")
  .then(function (script) {
    return loadScript("../my/2.js");
  })
  .then(function (Script) {
    return loadScript("../my/3.js");
  })
  .then(function (script) {
    return loadScript("../my/4.js");
  })
  .then(function (script) {
    func1(); // func1은 1.js에 있는 함수
    func2(); // func2은 2.js에 있는 함수
    func3(); // func3은 3.js에 있는 함수
    func4(); // func3은 4.js에 있는 함수
    // ...
  });
```

> 👼 모든 비동기 동작이 성공했다면 다음과 같이 잘 출력될 것이다!
>
> ![image](https://user-images.githubusercontent.com/67737432/128403830-41bf2dba-de4a-49a1-bf02-b04c178bfeba.png)

---







> \+ `fetch`와 프로미스의 체이닝 응용은 아직...
