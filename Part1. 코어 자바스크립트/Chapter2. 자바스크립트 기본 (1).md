# 👏🏻Chapter2. 자바스크립트 기본 - (1)

### 📌`use strict`

- `use strict`는 스크립트 최상단이 아닌 함수 본문 맨 앞에 올 수도 있습니다. 이때는 오직 해당 함수만 엄격 모드로 실행됩니다.
- 엄격모드는 대개 스크립트 전체에 적용한다. 따라서 최상단에 위치해야 합니다.

- 코드를 클래스와 모듈을 사용해 구성한다면 `use strict`를 생략해도 됩니다. (모던 자바스크립트는 '클래스’와 '모듈’이라 불리는 진일보한 구조를 제공합니다. 이 둘을 사용하면 `use strict`가 자동으로 적용됩니다.)

---

### 📌변수와 상수

- `let user = 'John', age = 25, message = 'Hello';` → 한줄에 여러 변수를 생성하면 코드가 좀 더 짧아 보이긴 하지만 권장하는 방법은 아닙니다. 가독성을 위해 한 줄에는 하나의 변수를 작성해주세요.
- `let` 변수를 2번 선언하면 에러가 발생합니다.  선언한 변수를 참조할 때는 `let` 없이 변수명만 사용해 참조해야 합니다.



#### 🔍변수 명명 규칙
1. 변수명에는 오직 문자와 숫자, 그리고 기호 `$`와 `_`만 들어갈 수 있습니다.
2. 첫 글자는 숫자가 될 수 없습니다.



#### 🔍대문자 상수

- 기억하기 힘든 값을 변수에 할당해 별칭으로 사용하는 것은 널리 사용되는 관습입니다.

- 이런 상수는 대문자와 밑줄로 구성된 이름으로 명명합니다. ex)`const COLOR_RED = "#F00";`

- 대문자 상수는 ‘하드 코딩한’ 값의 별칭을 만들 때 사용하면 됩니다.
- 반면 다음 상황에서는 대문자 상수를 사용하지 않습니다. 
  - `const pageLoadTime = /* 웹페이지를 로드하는데 걸린 시간 */;` 
  -  `pageLoadTime`의 값은 페이지가 로드되기 전에는 정해지지 않기 때문에 일반적인 방식으로 변수명을 지었습니다. 하지만 이 값은 최초 할당 이후에 변경되지 않으므로 여전히 상수입니다.



#### 🔍변수명 짓기

- 자신만의 규칙이나 소속된 팀의 규칙을 따르세요. 만약 사이트 방문객을 'user’라고 부르기로 했다면, 이와 관련된 변수를 `currentVisitor`나 `newManInTown`이 아닌 `currentUser`나 `newUser`라는 이름으로 지어야 합니다.
- 최대한 서술적이고 간결하게 명명해 주세요. `data`와 `value`는 나쁜 이름의 예시입니다. 이런 이름은 아무것도 설명해주지 않습니다. 코드 문맥상 변수가 가리키는 데이터나 값이 아주 명확할 때에만 이런 이름을 사용합시다.
- 변수를 추가하는 것은 악습이 아닙니다. 좋은 습관입니다. 모던 자바스크립트 압축기(minifier)와 브라우저는 코드 최적화를 잘해줍니다. 변수를 추가한다고 해서 성능 이슈가 생기지 않죠. 값이 다른 경우, 변수를 다르게 선언해 주면 코드 최적화에 도움이 될 수도 있습니다.

---

### 📌자료형

- `Infinity`
  - 어느 숫자든 0으로 나누면 무한대를 얻을 수 있습니다.
- `NaN`
  - `NaN`은 계산 중에 에러가 발생했다는 것을 나타내주는 값입니다. 부정확하거나 정의되지 않은 수학 연산을 사용하면 계산 중에 에러가 발생하는데, 이때 `NaN`이 반환됩니다.
- `null`
  -  자바스크립트에선 `null`을 ‘존재하지 않는(nothing)’ 값, ‘비어 있는(empty)’ 값, ‘알 수 없는(unknown)’ 값을 나타내는 데 사용합니다.
- `undefined`
  - `undefined`는 '값이 할당되지 않은 상태’를 나타낼 때 사용합니다. 
  - 변수에 `undefined`를 명시적으로 할당하는 것도 가능하나, 하지만 이렇게 `undefined`를 직접 할당하는 걸 권장하진 않습니다. 변수가 ‘비어있거나’ ‘알 수 없는’ 상태라는 걸 나타내려면 `null`을 사용하세요. 

> #### 💡자바스크립트에서의 수학 연산의 안전성
>
> - 말이 안 되는 수학 연산을 하더라도 스크립트는 치명적인 에러를 내뿜으며 죽지 않습니다. `NaN`을 반환하며 연산이 종료될 뿐입니다.



#### 🔍`typeof` 연산자

```javascript
typeof Math // "object"  - Math는 수학 연산을 제공하는 내장 객체

typeof null // "object" - null은 별도의 고유한 자료형을 가지는 특수 값으로 객체가 아니지만, 하위 호환성을 유지하기 위해 이런 오류를 수정하지 않고 남겨둔 상황임. 즉 언어 자체의 오류이므로 null이 객체가 아님에 유의하기!

typeof alert // "function"  (3) '함수형'이라는 것은 따로 없지만, 피연산자가 함수이면 'function'을 반환한다. 이 또한 하위 호환성 유지를 위해 남겨둔 상황임.
```

---

### 📌형변환

#### 🔍숫자형으로 변환

- 숫자형으로의 변환은 수학과 관련된 함수와 표현식에서 자동으로 일어납니다.
- `alert( "6" / "2" ); // 3, 문자열이 숫자형으로 자동변환된 후 연산이 수행됩니다.`

- `let num = Number("123"); // 문자열 "123"이 숫자 123으로 변환됩니다. `

- 한편, 숫자 이외의 글자가 들어가 있는 문자열을 숫자형으로 변환하려고 하면, 그 결과는 `NaN`이 됩니다. 
- `Number(null); // 0`
- `Number(undefined); // Nan`



#### 🔍불린형으로 변환

- 숫자 `0`, 빈 문자열, `null`, `undefined`, `NaN`과 같이 직관적으로도 “비어있다고” 느껴지는 값들은 `false`가 됩니다.
- 그 외의 값은 `true`로 변환됩니다.
- 자바스크립트에선 비어 있지 않은 문자열은 언제나 `true`입니다. 즉, 문자열 `"0"`, `" "(공백)`은 자바스크립트에서 `true`입니다.

---

### 📌기본 연산자

#### 🔍단항 연산자 +와 숫자형으로의 변환

- 덧셈 연산자 `+`는 이항 연산자뿐만 아니라 단항 연산자로도 사용할 수 있습니다.

- 숫자에 단항 덧셈 연산자를 붙이면 이 연산자는 아무런 동작도 하지 않습니다. 그러나 피연산자가 숫자가 아닌 경우엔 숫자형으로의 변환이 일어납니다.

- ```javascript
  let apples = "2";
  let oranges = "3";
  // 1. 문자열을 연결하는 +(이항 덧셈 연산자)
  alert( apples + oranges ); // 23,
  
  // 2. 단항 연산자(+)로 숫자형으로 먼저 변환하고 더하기
  alert( +apples + +oranges ); // 5
  ```



#### 🔍쉼표 연산자

- 쉼표 연산자 `,`는 여러 표현식을 코드 한 줄에서 평가할 수 있게 해줍니다. 이때 표현식 각각이 모두 평가되지만, **마지막 표현식의 평가 결과만 반환되는 점**에 유의해야 합니다.

- ```javascript
  // 한 줄에서 세 개의 연산이 수행됨
  for (a = 1, b = 3, c = a * b; a < 10; a++) {
   ...
  }
  ```

- 쉼표 연산자는 코드 가독성에 도움이 되지 않습니다. 따라서 곰곰이 생각해 본 후, 진짜 필요한 경우에만 사용하시길 바랍니다.

---

### 📌비교 연산자

- 비교하려는 값의 자료형이 다르면 자바스크립트는 이 값들을 숫자형으로 바꿉니다.

  - `alert( '2' > 1 ); // true, 문자열 '2'가 숫자 2로 변환된 후 비교가 진행됩니다.`
  - 그러나 둘 다 문자열이면 일반적인 '사전순'으로 비교합니다. `alert("2" > "12"); // true`

- 이 때문에 종종 흥미로운 상황이 생깁니다.

	```javascript
  let a = 0;
  alert( Boolean(a) ); // false
  
  let b = "0";
  alert( Boolean(b) ); // true => 따로따로 보면 각각 true와 false입니다.
  
  alert(a == b); // true! 
  // 왜냐하면 (동등비교연산자)==는 b("0")를 0으로 형변환합니다.
  // 그러나 Boolean("0")은 "0"을 비어있지 않은 문자열로 처리합니다.
  ```
  
- 이를 방지하는 방법은 `===`를 사용하는 것! 
  **일치 연산자(strict equality operator) `===`를 사용하면 형 변환 없이 값을 비교할 수 있습니다.**

- `null`과 `undefined`도 마찬가지로 `==`로 비교하면 `true`이지만, `===`로 비교하면 두 값의 자료형이 다르기 때문에 `false`입니다.
  단, `null`과 `undefined`는 자기들끼리만 잘 어울립니다. 
  
  ```javascript
  alert( null === undefined ); // false
  alert( null == undefined ); // true
  ```


- `null` vs `0`

  ```javascript
  alert( null > 0 );  // (1) false
  alert( null == 0 ); // (2) false : ==(동등연산자)는 피연산자가 undefined/null일 때 형변환하지 않습니다. 양쪽이 "둘 다" undefined/null이면 true를 반환하지만, 한쪽만 undefined/null이면 false를 반환합니다.
  alert( null >= 0 ); // (3) true : null이 숫자형 0으로 변환됩니다.
  ```
  

- `undefined`는 (`null`을 제외한) 다른 값과 비교해서는 안 됩니다. 항상 `false`를 반환합니다.



#### 🔍그래서 중요한 것은...

- 일치 연산자 `===`를 제외한 비교 연산자의 피연산자에 `undefined`나 `null`이 오지 않도록 특별히 주의하시기 바랍니다.
- 또한, `undefined`나 `null`이 될 가능성이 있는 변수가 `<`, `>`, `<=`, `>=`의 피연산자가 되지 않도록 주의하시기 바랍니다. 명확한 의도를 갖고 있지 않은 이상 말이죠. 만약 변수가 `undefined`나 `null`이 될 가능성이 있다고 판단되면, 이를 따로 처리하는 코드를 추가하시기 바랍니다.

---

### 📌if와 ?를 사용한 조건 처리

- 숫자 `0`, 빈 문자열`""`, `null`, `undefined`, `NaN`은 불린형으로 변환 시 모두 `false`가 됩니다. 이런 값들은 ‘falsy(거짓 같은)’ 값이라고 부릅니다.
- 이 외의 값은 불린형으로 변환시 `true`가 되므로 ‘truthy(참 같은)’ 값이라고 부릅니다.

#### 🔍물음표 연산자 `?`

- 사용 방법은 일반 `?`와 동일합니다 - `조건 ? 참일때 리턴 : 거짓일때 리턴`

- 물음표 연산자`?`는 조건에 따라 반환 값을 달리하려는 목적으로 만들어졌습니다. 이런 목적에 부합하는 곳에 물음표를 사용하시길 바랍니다. 여러 분기를 만들어 처리할 때는 `if`를 사용하세요.

---

