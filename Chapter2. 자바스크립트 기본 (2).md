# 👏🏻Chapter2. 자바스크립트 기본 - (2)

### 📌논리연산자

#### 🔍첫 번째 truthy를 찾는 OR 연산자 `||`

- 가장 왼쪽 피연산자부터 시작해 오른쪽으로 나아가며 피연산자를 평가합니다.

- 각 피연산자를 불린형으로 변환합니다. 변환 후 그 값이 `true`이면 연산을 멈추고 **해당 피연산자의 변환 전 원래 값을 반환** 합니다.

- 피연산자 모두를 평가한 경우(모든 피연산자가 `false`로 평가되는 경우)엔 **마지막 피연산자를 반환**합니다.

  ```javascript
  alert( null || 2 || undefined );	// 2
  ```

  > 그렇다면 AND `&&` 연산자는 당연히 첫번째 `falsy`를 찾겠죠?

  

#### 🔍OR 연산자`||`를 이용한 단락 평가

- OR 연산자 `||`가 제공하는 또 다른 기능은 '단락 평가(short circuit evaluation)'입니다.

- OR`||`은 왼쪽부터 시작해서 오른쪽으로 평가를 진행하는데, truthy를 만나면 나머지 값들은 건드리지 않은 채 평가를 멈춥니다. 이런 프로세스를 '단락 평가’라고 합니다.

	 ```javascript
  true || alert("not printed");	// true를 만나 평가를 멈춤(출력 X)
  false || alert("printed");	// printed가 출력된다.



#### 🔍NOT 연산자 2개 `!!`로 불린형으로 변환하기

- NOT 연산자 2개  `!!` 로 내장 함수 `Boolean`을 사용한 것과 같은 결과를 도출할 수 있습니다.

	```javascript
alert( !!"non-empty string" ); // true
alert( !!null ); // false

---

### 📌null 병합 연산자 `??`

- null 병합 연산자(nullish coalescing operator) `??`를 사용하면 짧은 문법으로 여러 피연산자 중 **그 값이 확정되어있는 변수**를 찾을 수 있습니다. 즉, `null`이나 `undefined`가 아닌 첫 번째 피연산자를 찾을 수 있습니다.

- 예를 들어 세 변수 중 실제 값이 있는 변수의 값을 출력하는데, 세 변수 모두 값이 없다면 '익명의 사용자’가 출력되도록 할 때 `??`를 사용하면 간편합니다.


  ```javascript
  alert(firstName ?? lastName ?? nickName ?? "익명의 사용자"); // 바이올렛
  ```

- 얼핏 보면 `||`(첫번째 truthy 값을 반환)와 비슷해보이지만 다음 경우를 한번 봅시다.

  ```javascript
  let height = 0;
  alert(height || 100); // 100, 0을 falsy로 취급했습니다.
  alert(height ?? 100); // 0, 0을 '할당'했기 때문에 값이 확정되어있는 height을 출력합니다.
  ```

  이런 특징 때문에 **높이처럼 `0`이 할당될 수 있는 변수를 사용해 기능을 개발할 땐 `||`보다 `??`가 적합합니다.**

---

### 📌while과 for 반복문

#### 🔍`?` 오른쪽엔 `break`나 `continue`가 올 수 없습니다.

- `(i > 5) ? alert(i) : continue; // error - 여기에 continue를 사용하면 안 됩니다.`
- 이는 물음표 연산자 `?`를 `if`문 대용으로 쓰지 말아야 하는 이유 중 하나입니다.

---

### 📌함수

- 함수 내부에 외부 변수와 동일한 이름을 가진 변수가 선언되었다면, 내부 변수는 외부 변수를 **가립니다**. 

- 변수는 연관되는 함수 내에 선언하고, 전역 변수는 되도록 사용하지 않는 것이 좋습니다. 다만 프로젝트 전반에서 사용되는 데이터는 전역 변수에 저장하는 것이 유용한 경우도 있으니 이 점을 알아두시기 바랍니다.

- `return`문이 없거나 `return` 지시자만 있는 함수는 `undefined`를 반환합니다.



#### 🔍함수는 언제나 '복사된 값'을 사용합니다.

```javascript
// 함수를 호출하면, 함수에 전달된 인자는 지역변수 from과 text에 복사됩니다.
function showMessage(from, text) {	
  from = '*' + from + '*';
  alert( from + ': ' + text );
}

let from = "Ann";
showMessage(from, "Hello"); // *Ann*: Hello

// 함수는 복사된 값을 사용하기 때문에 바깥의 "from"은 값이 변경되지 않습니다.
alert( from ); // Ann
```



#### 🔍기본값

- 매개변수에 값을 전달하지 않으면 그 값은 `undefined`가 됩니다. 그러나 매개변수에 값을 전달하지 않아도 그 값이 `undefined`가 되지 않게 하려면 '기본값(default value)'을 설정해주면 됩니다.

- 매개변수 기본값은 `if문`, `||`, `??` 등으로도 설정할 수 있습니다.

  - 논리 연산자 `||`를 사용합니다.

    ```javascript
    // 매개변수가 생략되었거나 빈 문자열("")이 넘어오면 변수에 '빈 문자열'이 할당됩니다.
    function showMessage(text) {
      text = text || '빈 문자열';
    }
    ```

  - null 병합연산자 `??`를 사용합니다. `0`처럼 falsy로 평가되는 값들도 일반 값으로 처리할 수 있어서 좋습니다.

    ```javascript
    // 매개변수 'count'가 넘어오지 않으면 'unknown'을 출력해주는 함수
    function showCount(count) {
      alert(count ?? "unknown");
    }
    showCount(0); // 0
    showCount(null); // unknown
    showCount(); // unknown
    ```

#### 🔍함수 이름짓기

- `"get…"` – 값을 반환함
- `"calc…"` – 무언가를 계산함
- `"create…"` – 무언가를 생성함
- `"check…"` – 무언가를 확인하고 불린값을 반환함
- `"show…"` – 무언가를 보여줌

> #### 💡함수 == 주석
>
> - 함수를 간결하게 만들면 테스트와 디버깅이 쉬워집니다. 그리고 함수 그 자체로 주석의 역할까지 합니다!
> - 이름만 보고도 어떤 동작을 하는지 알 수 있는 코드를 **자기 설명적(self-describing)** 코드라고 부릅니다.

---

### 📌함수 표현식

- 자바스크립트에서 함수는 값입니다. 따라서 함수를 값처럼 취급할 수 있습니다. 

  ```javascript
  function sayHi() {   // (1) 함수 생성
    alert( "Hello" );
  }
  let func = sayHi;    // (2) 함수 복사
  
  func(); // Hello     // (3) 복사한 함수를 실행(정상적으로 실행됩니다)!
  sayHi(); // Hello    //     본래 함수도 정상적으로 실행됩니다.
  ```



#### 🔍콜백 함수

- 함수를 함수의 인수로 전달하고, 필요하다면 인수로 전달한 그 함수를 "나중에 호출(called back)"하는 것이 콜백 함수의 개념입니다. 

  ```javascript
  function ask(question, yes, no) {
    if (confirm(question)) yes()
    else no();
  }
  
  function showOk() {alert( "동의하셨습니다." );}
  function showCancel() {alert( "취소 버튼을 누르셨습니다." );}
  
  // 함수 ask의 인수, showOk와 showCancel은 콜백 함수 또는 콜백이라고 불립니다.
  ask("동의하십니까?", showOk, showCancel);
  
  // 또는 익명함수로 한번에 나타낼 수 있다.
  ask(
    "동의하십니까?",
    function() { alert("동의하셨습니다."); },
    function() { alert("취소 버튼을 누르셨습니다."); }
  );
  ```

  사용자가 "yes"라고 대답한 경우 `showOk`가 콜백이 되고, "no"라고 대답한 경우 `showCancel`가 콜백이 됩니다.

#### 🔍함수 표현식 vs 함수 선언문

```javascript
// 함수 선언문
function sum(a, b) {return a + b;}
```

```javascript
// 함수 표현식
let sum = function(a, b) {return a + b;};
```



> #### 💡차이점 1 : 자바스크립트 엔진이 `언제` 함수를 생성하는지에 있습니다.

- **함수 표현식은 실제 실행 흐름이 해당 함수에 도달했을 때 함수를 생성합니다. **
  **따라서 실행 흐름이 함수에 도달했을 때부터 해당 함수를 사용할 수 있습니다.**

  ```javascript
  sayHi("John"); // error! : 전역 함수 선언문은 스크립트 어디에 있느냐에 상관없이 어디에서든 사용할 수 있습니다.
  
  let sayHi = function(name) {
    alert( `Hello, ${name}` );
  };
  ```

  

- **함수 선언문은 함수 선언문이 정의되기 전에도 호출할 수 있습니다.**

  ```javascript
  // 함수 선언문, sayHi는 스크립트 실행 준비 단계에서 생성되기 때문에, 스크립트 내 어디에서든 접근할 수 있습니다.
  sayHi("John"); 
  
  function sayHi(name) {
    alert( `Hello, ${name}` );
  }
  ```



> #### 💡차이점 2 : 스코프
>
 - `함수 선언문` : 엄격 모드에서 **함수 선언문**이 **코드 블록 내에** 위치하면 해당 함수는 블록 내 어디서든 접근할 수 있습니다. 
   **하지만 블록 밖에서는 함수에 접근하지 못합니다.**

   ```javascript
   let age = prompt("나이를 알려주세요.", 18);
   
   // 조건에 따라 함수를 선언함
   if (age < 18) {
     function welcome() {
       alert("안녕!");
     }
   } else {
     function welcome() {
       alert("안녕하세요!");
     }
   }
   
   // 함수를 나중에 호출합니다.
   welcome(); // Error: welcome is not defined
   ```

 - `함수 표현식` 

   ```javascript
   let age = prompt("나이를 알려주세요.", 18);
   let welcome;
   
   if (age < 18) {
     welcome = function() {
       alert("안녕!");
     };
   } else {
     welcome = function() {
       alert("안녕하세요!");
     };
   }
   
   welcome(); // 제대로 동작합니다.
   ```



#### 🔍**함수 선언문과 함수 표현식 중 무엇을 선택해야 하나요?**

- 경험상, 함수 선언문을 이용해 함수를 선언하는 걸 먼저 고려하는 게 좋습니다. 함수 선언 방식이 더 “**눈길을 사로잡습니다**”.

---

### 📌화살표 함수

> 함수 표현식보다 단순하고 간결한 문법으로 함수를 만들 수 있는 방법! `=>`

- 인수가 하나밖에 없다면 인수를 감싸는 괄호를 생략할 수 있습니다. 괄호를 생략하면 코드 길이를 더 줄일 수 있습니다.
  - `let double = n => n * 2; `
- 인수가 하나도 없을 땐 괄호를 비워놓으면 됩니다. **다만, 이 때 괄호는 생략할 수 없습니다.**
  - `let sayHi = () => alert("안녕하세요!");`



#### 🔍본문이 여러줄인 화살표 함수?

- 이때는 중괄호 안에 평가해야 할 코드를 넣어주어야 합니다. 
  **그리고 `return` 지시자를 사용해 명시적으로 결괏값을 반환**해 주어야 합니다.

  ```javascript
  let sum = (a, b) => {  // 중괄호는 본문 여러 줄로 구성되어 있음을 알려줍니다.
    let result = a + b;
    return result; // 중괄호를 사용했다면, return 지시자로 결괏값을 반환해주어야 합니다.
  };
  
  alert( sum(1, 2) ); // 3
  ```



---

