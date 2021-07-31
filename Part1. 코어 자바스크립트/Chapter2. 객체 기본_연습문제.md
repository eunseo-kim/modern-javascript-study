# ✍🏻객체: 기본 연습문제

## 📌객체

- 객체가 비어있는지 확인하기

  ```javascript
  function isEmpty(obj) {
   for (let key in obj) {
       return false;
   }
   return true;
  }
  ```

- 프로퍼티 합계 구하기

  ```js
  let sum = 0
  for (let key in salaries) {
      sum += salaries[key];
  }
  ```

- 프로퍼티 값 두 배로 부풀리기

  ```js
  function multiplyNumeric(obj) {
      for (let key in obj) {
          if (typeof obj[key] === 'number') {
              obj[key] *= 2;
          }
      }
  }
  ```

  > 함수는 '복사된 값'을 넣는다고 했는데 객체는 복사된 값을 넣어도 값이 바뀌나?
  >
  > - **변수엔 객체가 그대로 저장되는 것이 아니라, 객체가 저장되어있는 '메모리 주소’인 객체에 대한 '참조 값’이 저장됩니다.**

---

## 📌메서드와 this

- 객체 리터럴에서 'this' 사용하기

  ```js
  function makeUser() {
    return {
      name: "John",
      ref() {	
          return this;
      }
    };
  };
  
  let user = makeUser();
  
  alert( user.ref().name ); // John
  ```

- 계산기 만들기

  ```js
  let calculator = {
      read() {
          this.a = +prompt("더할 값 입력", 1);
          this.b = +prompt("더할 값 입력", 2);
          // prompt는 String 타입 반환,
          // +(단항연산자) 붙이면 숫자는 숫자형으로 변환됨
      },
      sum() {
          return this.a + this.b;
      },
  	mul() {
          return this.a * this.b;
      },
  }
  calculator.read();
  alert( calculator.sum() );
  alert( calculator.mul() );
  ```

---

## 📌new 연산자와 생성자 함수

- 계산기 만들기

  ```js
  function Calculator() {
    this.read = function () {
      this.a = prompt("값1", 0);
      this.b = prompt("값2", 0);
    };
    this.sum = function () {
      return this.a + this.b;
    };
    this.mul = function () {
      return this.a * this.b;
    };
  }
  
  let calculator = new Calculator();
  calculator.read();
  
  alert("Sum=" + calculator.sum());
  alert("Mul=" + calculator.mul());
  ```

  

---

