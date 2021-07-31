# 7-(1). 프로퍼티 플래그와 설명자

## 🔍 프로퍼티 플래그?

프로퍼티의 설명자에는 기본적으로 4가지가 있다!
이 중에서 값(value)와 함께 플래그(flag)라 불리는 특별한 속성 3가지를 갖는다.

1. `value` : 프로퍼티의 값
2. `writable` : 값(value)의 갱신 가능 여부  🚩
3. `enumerable` : 열거 가능 여부 🚩 (해당 속성이 `for...in`루프나 `Object.keys()`에서 노출될지 말지를 정의한다)
4. `configurable` : 제거 또는 재정의 가능 여부 🚩



- 프로퍼티 플래그는 `Object.getOwnPropertyDescriptor`라는 메소드를 통해 정보를 얻을 수 있다
  (보통 '평범한 방식'으로 프로퍼티를 만들면 플래그는 모두 `true`가 된다.)

  ```js
  const person = {
      name: "Kim",
  };
  // 프로퍼티 동적 생성
  person.age = 22;
  
  // getOwnPropertyDescriptors(obj) => 모든 프로퍼티에 대한 정보를 얻을 수 있다.
  console.log(Object.getOwnPropertyDescriptors(person));
  
  // getOwnPropertyDescriptor(obj, 프로퍼티 이름) => 또는 이렇게 특정 프로퍼티에 대해서만 추출하는 것도 가능하다.
  console.log(Object.getOwnPropertyDescriptor(person, 'name'));
  ```

  > #### 출력 결과
  >
  > ```
  > {
  >   name: {
  >     value: 'Kim',
  >     writable: true,
  >     enumerable: true,
  >     configurable: true
  >   },
  >   age: { value: 22, writable: true, enumerable: true, configurable: true }
  > }
  > { value: 'Kim', writable: true, enumerable: true, configurable: true }
  > ```



## 🔍프로퍼티 플래그 변경하기

`Object.defineProperty(obj, propertyName, 적용하려는 프로퍼티 설명자)`를 사용하면 플래그를 변경할 수 있다!

1. `Object.defineProperty`로 value 변경하기

   ```js
   let user = {};
   
   Object.defineProperty(user, "name", { value: "Eunseo" });
   
   console.log(Object.getOwnPropertyDescriptor(user, "name"));
   // { value: 'Eunseo', writable: false, enumerable: false, configurable: false }
   ```

   > `defineProperty`를 사용해 프로퍼티를 만든 경우, `descriptor`에 플래그 값을 명시하지 않으면 
   > 플래그 값이 자동으로 `false`가 된다.
   
   

2. `Object.defineProperty`로 writable 변경하기

   ```js
   let user = { name: "Eunseo" };
   
   Object.defineProperty(user, "name", { writable: false });
   user.name = "John"; // 엄격 모드에서는 error
   console.log(user); // Eunseo (바뀌지 않았음)
   ```

   

3. `Object.defineProperty`로 enumerable 변경하기

   ```js
   let obj = {
     a: 1,
     b: 2,
     c: 3,
     d: 4,
   };
   
   // b, c의 enumerable 플래그를 false로 변경한다.
   Object.defineProperty(obj, "b", { enumerable: false });
   Object.defineProperty(obj, "d", { enumerable: false });
   
   for (let value in obj) {
     console.log(value);
   }
   // a c 만 출력된다.
   ```

   

4. `Object.defineProperty`로 configurable 변경하기

   ```js
   let obj = {};
   
   Object.defineProperty(obj, "a", {
     configurable: false,
   });
   
   // 삭제가 안된다!
   delete obj.a;
   console.log(obj.a); // 1
   
   // 재정의도 불가능하다!
   Object.defineProperty(obj, "a", { value: 2000 }); // error => TypeError: Cannot redefine property: a
   ```

   

