# 7-(2). 프로퍼티 getter와 setter

📌프로퍼티는 [데이터 프로퍼티]와 [접근자 프로퍼티]로 구분할 수 있다!

|    종류    |                  데이터 프로퍼티                   |                       접근자 프로퍼티                        |
| :--------: | :------------------------------------------------: | :----------------------------------------------------------: |
|  **특징**  |       `키 : 값`으로 구성된 일반적인 프로퍼티       | 자체적으로는 값을 갖지 않지만<br/>다른 데이터 프로퍼티의 값을 읽거나 저장할 때<br/>호출되는 **접근자 함수**로 구성된 프로퍼티 |
| **설명자** | `value`, `writable`, `enumerable`, `configurable`, |          `get`, `set`, `enumerable`, `configurable`          |



## 🔍이번에는 [접근자 프로퍼티]에 대해 알아보자!

- 접근자 프로퍼티는 `getter`과 `setter` 메서드로 표현된다.

- 객체 리터럴 안에서 getter, setter 메서드는 `get`, `set`으로 나타낼 수 있다.

  ```js
  let user = {
    name: "John",
    surname: "Smith",
  
    get fullName() {
      return `${this.name} ${this.surname}`;
    },
  
    set fullName(value) {
      [this.name, this.surname] = value.split(" ");
    },
  };
  
  // set fullName 실행
  user.fullName = "Alice Cooper";
  
  // get fullName 실행
  console.log(user.fullName); //Alice Cooper
  ```

  

- 참고로 getter, setter 둘 중 하나만 만들면 (엄격모드에서) 에러가 발생하며 제대로 실행되지 않는다!

  ```js
  let user = {
    get fullName() {
      return `...`;
    },
  };
  
  user.fullName = "Test"; // strict mode => TypeError: Cannot set property fullName of #<Object> which has only a getter
  ```

---



## 🔍getter, setter 어떻게 활용할 수 있을까?

- 실제 프로퍼티 값을 감싸는 래퍼(wapper)처럼 사용하기!

   ```js
   let user = {
     // 프로퍼티 이름과 getter, setter의 이름이 동일함(프로퍼티 이름을 덮어 씀)
     get name() {
       return this._name;
     },
   
     set name(value) {
       if (value.length < 4) {
         console.log("too short!");
         return;
       }
       this._name = value;
     },
   };
   
   user.name = "Pete";
   
   // get name 실행
   console.log(user.name); // Pete
   
   // set name 실행
   user.name = ""; // too short!
   ```

   > 💥물론 이렇게
   >
   > ```js
   > user._name = "a";
   > console.log(user._name);  // a
   > ```
   >
   > 직접 `user._name`을 이용해서 바로 접근할 수도 있다.
   >
   > **그러나 `_` 로 시작하는 프로퍼티는 외부에서 건드리지 않는 것이 관습**✔

---



## 🔍Object.defineProperty로 접근자를 만들어보자

> `Object.defineProperty(obj, propertyName, 적용하려는 프로퍼티 설명자)`

- 앞의 [데이터 프로퍼티]에서 사용했던 방식과 거의 동일하다.

```js
let user = {
  name: "John",
  surname: "Smith",
};

Object.defineProperty(user, "fullName", {
  get() {
    return `${this.name} ${this.surname}`;
  },

  set(value) {
    [this.name, this.surname] = value.split(" ");
  },
});

console.log(user.fullName); // John Smith
```



