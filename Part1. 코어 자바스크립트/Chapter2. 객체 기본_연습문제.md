# βπ»κ°μ²΄: κΈ°λ³Έ μ°μ΅λ¬Έμ 

## πκ°μ²΄

- κ°μ²΄κ° λΉμ΄μλμ§ νμΈνκΈ°

  ```javascript
  function isEmpty(obj) {
   for (let key in obj) {
       return false;
   }
   return true;
  }
  ```

- νλ‘νΌν° ν©κ³ κ΅¬νκΈ°

  ```js
  let sum = 0
  for (let key in salaries) {
      sum += salaries[key];
  }
  ```

- νλ‘νΌν° κ° λ λ°°λ‘ λΆνλ¦¬κΈ°

  ```js
  function multiplyNumeric(obj) {
      for (let key in obj) {
          if (typeof obj[key] === 'number') {
              obj[key] *= 2;
          }
      }
  }
  ```

  > ν¨μλ 'λ³΅μ¬λ κ°'μ λ£λλ€κ³  νλλ° κ°μ²΄λ λ³΅μ¬λ κ°μ λ£μ΄λ κ°μ΄ λ°λλ?
  >
  > - **λ³μμ κ°μ²΄κ° κ·Έλλ‘ μ μ₯λλ κ²μ΄ μλλΌ, κ°μ²΄κ° μ μ₯λμ΄μλ 'λ©λͺ¨λ¦¬ μ£ΌμβμΈ κ°μ²΄μ λν 'μ°Έμ‘° κ°βμ΄ μ μ₯λ©λλ€.**

---

## πλ©μλμ this

- κ°μ²΄ λ¦¬ν°λ΄μμ 'this' μ¬μ©νκΈ°

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

- κ³μ°κΈ° λ§λ€κΈ°

  ```js
  let calculator = {
      read() {
          this.a = +prompt("λν  κ° μλ ₯", 1);
          this.b = +prompt("λν  κ° μλ ₯", 2);
          // promptλ String νμ λ°ν,
          // +(λ¨ν­μ°μ°μ) λΆμ΄λ©΄ μ«μλ μ«μνμΌλ‘ λ³νλ¨
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

## πnew μ°μ°μμ μμ±μ ν¨μ

- κ³μ°κΈ° λ§λ€κΈ°

  ```js
  function Calculator() {
    this.read = function () {
      this.a = prompt("κ°1", 0);
      this.b = prompt("κ°2", 0);
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

