# 👏🏻Chapter4. 객체: 기본 (2)

### 📌new 연산자와 생성자 함수

> 생성자의 의의 - 재사용할 수 있는 객체 생성 코드를 구현하는 것입니다.

- 생성자 함수의 관례 → **함수 이름의 첫 글자가 대문자 + 반드시 `new` 연산자를 붙여 사용하는 것**

  ```js
  function User(name) {
    this.name = name;
    this.isAdmin = false;
  }
  let user = new User("John");
  alert(user.name); // John
  ```

  함수에`new`를 붙여 실행한다면 어떤 함수든 생성자 함수가 될 수 있습니다. 또한 이름의 첫글자가 대문자인 함수는 new를 붙여 실행하는 것은 공동의 약속입니다.

#### 🔍new function() / 익명 생성자 함수

- 복잡한 객체를 만들되, 재사용할 필요가 없는 경우 **익명 생성자 함수**로 감싸주는 방식을 사용할 수 있습니다. 따라서 재사용은 막으면서 코드를 캡슐화 할 수 있습니다.

  ```js
  let user = new function() {
    this.name = "John";
    this.isAdmin = false;
    // ... 복잡한 코드
  };
  ```

---

### 📌옵셔널 체이닝 `?.`

- `?.` 왼쪽 평가 대상이 `null`이나 `undefined`인지 확인하고 `null`이나 `undefined`가 아니라면 평가를 계속 진행합니다.

  > `?.`은 언제 사용하나요?
  >
  > - `?.`은 **`?.`왼쪽 평가대상이 없어도 괜찮은 경우에만 선택적으로 사용**해야 합니다.


---

### 📌심볼형

- `Symbol` : '**심볼(symbol)**'은 유일한 식별자(unique identifier)를 만들고 싶을 때 사용합니다.
- **심볼은 문자형으로 자동 형 변환되지 않습니다.** 심볼을 반드시 출력해줘야 하는 상황이라면 아래와 같이 `.toString()` 메서드를 명시적으로 호출해주면 됩니다.

#### 🔍숨김 프로퍼티

- 심볼을 이용하면 ‘숨김(hidden)’ 프로퍼티를 만들 수 있습니다. 숨김 프로퍼티는 외부 코드에서 접근이 불가능하고 값도 덮어쓸 수 없는 프로퍼티입니다.

- **심볼 값을 프로퍼티 키로 사용하여 프로퍼티를 생성하면 외부에 노출할 필요가 없는 프로퍼티를 은닉할 수 있습니다.** `for ... in` 문이나 `Object.keys`, `Object.getOwnPropertyNames` 메서드로도 찾을 수 없습니다.

  ```JS
  const obj = {
      [Symbol('mySymbol')]: 1
  };
  for (const key in obj) {
      console.log(key);	// 아무것도 출력되지 않는다.
  }
  console.log(Object.keys(obj));	// []
  console.log(Object.getOwnPropertyNames(obj));	// []
  ```

  - 그런데 `Object.assign`은 키가 심볼인 프로퍼티를 배제하지 않고 객체 내 모든 프로퍼티를 복사합니다.

#### 🔍전역 심볼

- 심볼은 이름이 같더라도 모두 별개로 취급됩니다. 그런데 이름이 **같은 심볼이 같은 개체를 가리키길 원하는 경우**도 가끔 있습니다.

- `전역 심볼 레지스트리(global symbol registry)` : 전역 심볼 레지스트리 안에 심볼을 만들고 해당 심볼에 접근하면, 이름이 같은 경우 항상 동일한 심볼을 반환해줍니다.

  ```js
  let id1 = Symbol("id");
  let id2 = Symbol("id");
  console.log(id1 === id2); // false
  
  let id3 = Symbol.for("id");
  let id4 = Symbol.for("id");
  console.log(id3 === id4); // true
  ```

- `Symbol.keyFor` 메서드를 이용하면 전역 심벌 레지스트리에 저장된 심벌 값의 키를 추출할 수 있습니다.

  ```js
  const s1 = Symbol.for("mySymbol");
  Symbol.keyFor(s1);	// mySymbol
  
  // 그러나 keyFor은 Symbol 함수를 호출하여 생성한 심벌값은 사용할 수 없습니다.
  // keyFor의 검색 범위가 전역 심볼 레지스트리이기 때문입니다.
  const s2 = Symbol("foo");
  Symbol.keyFor(s2);	// → undefined
  ```

- 일반 심볼에서 이름을 얻고 싶으면 `description` 프로퍼티를 사용하면 됩니다. → `s2.description`

---

### 📌객체를 원시형으로 변환하기

#### 🔍ToPrimitive

- 객체의 형 변환 3가지 (`hint`(구분기준)에 따라 3가지로 분류해봅시다.)

  - `string`

    ```js
    // 객체를 출력하려고 함
    alert(obj);
    ```

  - `number`

    예를 들어 이런 경우에서 hint는 `number`이 됩니다.

    ```js
    let obj = new Date();
    console.log(obj); //2021-07-24T15:15:32.127Z
    
    let number = Number(obj);  //hint가 number이 됨
    console.log(number); //1627139732127
    ```

  - `default`

    연산자가 기대하는 자료형이 ‘확실치 않을 때’ hint는 `default`가 됩니다.

#### 🔍자바스크립트의 형변환 알고리즘

1. **객체에  `obj[Symbol.toPrimitive](hint)`메서드가 있는지 찾고 있다면 메서드 호출**

   - ```js
     let user = {
       name: "John",
       money: 1000,
     
       [Symbol.toPrimitive](hint) {	// 얘를 실행시킨다.
         alert(`hint: ${hint}`);
         return hint == "string" ? `{name: "${this.name}"}` : this.money;
       }
     };
     
     alert(user); // hint: string -> {name: "John"}
     alert(+user); // hint: number -> 1000
     alert(user + 500); // hint: default -> 1500
     ```

   - 이처럼 `Symbol.toPrimitive`를 활용하면 해당 심볼 메소드 하나로 객체의 형 변환을 다룰 수 있다.

2. **1에 해당하지 않고, hint가 `string`이라면 `toString` → `valueOf` 순으로 호출**

3. **1, 2에 해당하지 않고 hint가 `number` 또는 `default` 라면 `valueOf` → `toString` 순으로 호출**

   - ```js
     let user = {
       name: "John",
       money: 1000,
     
       // hint가 "string"인 경우
       toString() {
         return `{name: "${this.name}"}`;
       },
     
       // hint가 "number"나 "default"인 경우
       valueOf() {
         return this.money;
       }
     };
     
     alert(user); // toString -> {name: "John"}
     alert(+user); // valueOf -> 1000
     alert(user + 500); // valueOf -> 1500
     ```

     > #### 💡일반 객체에서 toString과 valueOf
     >
     > - `toString`은 문자열 `"[object Object]"`을 반환합니다.
     > - `valueOf`는 객체 자신을 반환합니다.

---



