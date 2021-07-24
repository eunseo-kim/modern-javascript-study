# 👏🏻Chapter4. 객체: 기본 (1)

### 📌객체

#### 🔍객체 리터럴 
> **객체 리터럴** : 중괄호 `{...}`를 이용해 객체를 선언하는 것
> 프로퍼티 : ‘키: 값’ 쌍으로 구성된 것

```js
const user = {			// 객체
  name: "John",			// 키
  age: 30,				// 값
  "likes birds": true  	// 복수의 단어는 따옴표로 묶어야 합니다.
};
```

> #### 💡**상수 객체는 수정될 수 있습니다.**
>
> - 주의하세요. `const`로 선언된 객체는 수정될 수 있습니다.
>
> - 예를 들어, 위의 예시에서 `const`는 `user = ...`를 전체적으로 설정하려고 할 때만 오류가 발생합니다.
> - 즉, `user.name = "Peter";` 을 실행해도 오류가 발생하지 않습니다.



#### 🔍프로퍼티 값 읽기 - 점 표기법 `.`  / 대괄호 표기법 `[]` 

- 키가 `"like birds"`와 같이 복수의 단어인 경우, 점 표기법을 사용할 수 없습니다. 따라서 대괄호 표기법을 사용해야 합니다.
  `user["likes birds"] = false;`

- 변수 `key`는 런타임에 평가되기 때문에 사용자 입력값 변경 등에 따라 값이 변경될 수 있습니다. 
  어떤 경우든, 평가가 끝난 이후의 결과가 프로퍼티 키로 사용됩니다.

- 프로퍼티 이름이 확정된 상황이고, 단순한 이름이라면 처음엔 점 표기법을 사용하다가 뭔가 복잡한 상황이 발생했을 때 대괄호 표기법으로 바꾸는 경우가 많습니다.

  ```js
  let user = {
    name: "John",
    age: 30,
  };
  let key = "name";
  
  // 대괄호 표기법
  console.log(user[key]);	// John
  
  // 점 표기법 => 불가능!
  console.log(user.key);	// undefined
  ```

#### 🔍계산된 프로퍼티

- 객체 리터럴 안의 프로퍼티 키가 대괄호로 둘러싸여 있는 경우, 이를 **계산된 프로퍼티**라고 합니다.

  ```js
  let fruit = prompt("어떤 과일을 구매하시겠습니까?", "apple");
  
  let bag = {
    [fruit]: 5, // 변수 fruit에서 프로퍼티 이름을 동적으로 받아 옵니다.
  };
  
  alert( bag.apple ); // fruit에 "apple"이 할당되었다면, 5가 출력됩니다.
  ```

#### 🔍프로퍼티 존재 여부 확인하기

- 자바스크립트는 존재하지 않는 프로퍼티에 접근하려 해도 에러가 발생하지 않고 `undefined`를 반환합니다.
- `in` : `"key" in object` - 프로퍼티 이름이 객체에 존재하는지 확인한 후 `true`, `false`를 반환합니다.

---

### 📌참조에 의한 객체 복사

#### 🔍객체 복사, 병합과 Object.assign

- `Object.assign(dest, [src1, src2, src3...])`
  - 객체 `src1, ..., srcN`의 프로퍼티를 `dest`에 복사하고 `dest`를 반환합니다.

#### 🔍중첩 객체 복사

```js
let user = {
  name: "John",
  sizes: {
    height: 182,
    width: 50
  }
};

let clone = Object.assign({}, user);
// 그러나 여전히 user과 clone은 같은 sizes를 공유합니다.
alert( user.sizes === clone.sizes ); // true
```

- 이 문제를 해결하려면 `user[key]`의 각 값을 검사하면서, 그 값이 객체인 경우 객체의 구조도 복사해주는 반복문을 사용해야 합니다. 이런 방식을 '**깊은 복사(deep cloning)**'라고 합니다.
- `_.cloneDeep(target)` 을 이용하면 깊은 복사를 할 수 있습니다.

---

### 📌메서드와 this

- `this` 값은 런타임에 결정됩니다. 동일한 함수라도 다른 객체에서 호출했다면 `this`가 참조하는 값이 달라집니다.

  ```js
  let user = { name: "John" };
  let admin = { name: "Admin" };
  
  function sayHi() {
    alert( this.name );
  }
  
  // 별개의 객체에서 동일한 함수를 사용함
  user.f = sayHi;
  admin.f = sayHi;
  
  user.f(); // John  (this == user)
  admin.f(); // Admin  (this == admin)
  ```

  - 함수 본문에 `this`가 사용되었다면, 객체 컨텍스트 내에서 함수를 호출할 것이라고 예상하시면 됩니다.
  - 자바스크립트에서 `this`는 런타임에 결정됩니다. 메서드가 어디서 정의되었는지에 상관없이 `this`는 ‘점 앞의’ 객체가 무엇인가에 따라 ‘자유롭게’ 결정됩니다.

> #### 💡메소드와 함수의 차이?
>
> - 함수는 객체로부터 독립적이며, 메소드는 객체에 종속적이다. [**+ 설명 더보기**](https://stackoverflow.com/questions/155609/whats-the-difference-between-a-method-and-a-function)

---





