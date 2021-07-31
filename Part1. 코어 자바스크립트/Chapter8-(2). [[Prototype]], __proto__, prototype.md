# 8-(2). [[Prototype]], \_\_proto\_\_, prototype🤦🏻‍♂️?

### 🔍1. `[[Prototype]]`은 무엇인가!

- 모든 자바스크립트 객체는 `[[Prototype]]`이라는 **<u>숨김 프로퍼티/내부 프로퍼티</u>**를 갖는다. 
  이 숨김 프로퍼티 값은 `null`이거나 **다른 객체에 대한 참조**가되는데, 
  다른 객체를 참조하는 경우 **참조 대상을 '프로토타입(prototype)'**이라 부른다.

  > 💡자바스크립트에서 `[[...]]` 로 감싼 이름(내부 슬롯, 내부 메서드)들은 자바스크립트 엔진에서 실제로 동작은 하지만 개발자가 직접 접근할 수 있도록 외부로 공개된 객체의 프로퍼티는 아니다! 
  >
  > 💡따라서 원칙적으로는 `[[...]]`에 **직접적으로 접근하거나 호출할 수 있는 방법을 제공하지 않는다**고 한다.
  
- Javascript 언어 표준 스펙에서`[[prototype]]`으로 표현되는 **<u>프로토타입 객체에 대한 "링크"</u>**는 **<u>내부 속성</u>**으로 정의되어 있다. (MDN)

  > **프로토타입 객체**(또는 줄여서 **프로토타입**) 
  > : 어떤 객체의 **상위(부모) 객체의 역할을 하는 객체**로서, 다른 객체에 공유 프로퍼티(메서드 포함)을 제공한다.

- 정리하면 ......

  ![image](https://user-images.githubusercontent.com/67737432/127733123-10f32488-7819-4552-815d-00bea0df4108.png)


---

### 🔍2. `__proto__`는 무엇인가!

- `[[Prototype]]`은 **내부 프로퍼티** 이면서 **숨김 프로퍼티**이므로 `[[Prototype]]`을 통해서는 프로토타입 객체에 접근할 수 없다.

- 그러나 간접적으로 접근할 수 있는 수단을 제공하는데... 그것이 바로  `__proto__` 프로퍼티~!

- `__proto__`는 `[[Prototype]]`의 getter(획득자)이자 setter(설정자) 이다.


    ![image](https://user-images.githubusercontent.com/67737432/127733137-0fe3a01c-7839-4682-a702-dfe2e0f12aed.png)

  

- 예시 : `__proto__`로 새로운 프로토타입 할당하고 호출해보기

  ```js
  let animal = {
    eats: true,
  };
  let rabbit = {
    jumps: true,
  };
  // setter 함수인 set __proto__가 호출되어 rabbit 객체의 프로토타입을 교체
  rabbit.__proto__ = animal;
  // getter 함수인 get __proto__가 호출되어 rabbit 객체의 프로토타입을 취득
  rabbit.__proto__;
  
  // 프로퍼티 eats를 rabbit에서도 사용할 수 있게 되었습니다.
  console.log(rabbit.eats); // true
  ```
  
  
  
  > ### ✋🏻그러나!
  >
  > 요즘에는 `__proto__`를 잘 사용하지 않는다고 한다. 대신 함수 `Object.getPrototypeOf`나 `Object.setPrototypeOf`을 써서 프로토타입을 획득(get)하거나 설정(set)한다고 하는데.......🤯
  >
  > 이유는 이따 뒤에서 알아보자!!!
  

---

### 🔍3. `prototype` 프로퍼티는 무엇인가!

- 일반적인 프로토타입 객체(줄여서 프로토타입) 말고 `prototype`이라고 불리는 **프로퍼티**가 있다...🤦🏻‍♀️
- **✨함수 객체만✨이 소유하는 `prototype` 프로퍼티**는
  생성자 함수가 생성할 인스턴스(객체)의 **프로토타입을 가리킨다**!
- 앞에서 계속 사용했던 예시를 다시 가져와보면......
  ![image-20210731163244289](C:\Users\eunse\AppData\Roaming\Typora\typora-user-images\image-20210731163244289.png)
  
  

---

####  🤯결국 `__proto__`와 `prototype` 프로퍼티 둘 다 동일한 프로토타입을 가리킨다! 

> #### 하지만 프로퍼티를 사용하는 주체가 다르다!

- 한번 코드로 확인해보자!!

  ```js
  function Rabbit(name) {
    this.name = name;
  }
  
  // animal은 프로토타입 객체
  let animal = {
    eats: true,
  };
  Rabbit.prototype = animal;
  
  // 객체(인스턴스) 생성
  let bunny = new Rabbit("bunny");
  
  // (Rabbit) 생성자 함수의 prototype 프로퍼티를 이용하여 프로토타입 객체에 접근
  // (bunny) 객체의 __proto__ 프로퍼티를 이용하여 프로토타입 객체에 접근
  console.log(Rabbit.prototype === bunny.__proto__); // ✨true✨ - 결국 동일한 프로토타입을 가리킨다.
  
  // bunny는 prototype 프로퍼티를 소유하지 않는다.
  console.log(bunny.prototype); // undefined
  ```

---

### + 재밌는 사진...🐔🥚

### <img src="https://media.vlpt.us/post-images/adam2/12a5e250-fd90-11e9-959f-1f9679bea880/1nDBFaMpflmSsIKfMLxWIvQ.jpeg" alt="자바스크립트 Prototype 완벽 정리" style="zoom:37%;" />

#### 📌생성자 함수(🐔) = `prototype`  프로퍼티 소유

#### 📌(생성자를 이용해서 만든) 객체(🥚) = ` __proto__` 

---

# 🤗총정리!!! (처음 예제로...)

1. `User` 이라는 생성자 함수를 만들면 `User.prototype` 이라는 프로토타입 객체가 함께 생성된다.
   
   ![image](https://user-images.githubusercontent.com/67737432/127734418-a12b35fe-5972-4ef8-b791-14259ea90c78.png)
   
   
   
2. `User` 생성자 함수에는 `prototype` 이라는 프로퍼티가 존재하고, `User.prototype` 프로토타입 객체에는 `constructor` 이라는 프로퍼티가 존재한다. `prototype` 프로퍼티는 프로토타입 객체를 가리킨다. `constructor` 프로퍼티는 자신을 참조하는 생성자 함수를 가리킨다.
  
    ![image](https://user-images.githubusercontent.com/67737432/127734448-2990b459-c568-4e72-82a5-ac8dabe3732e.png)

   

3. `User.prototype`에 메소드나 프로퍼티를 새로 추가할 수 있다. 프로토타입 객체에 추가된 메소드나 프로퍼티는 상속된다.

    ![image](https://user-images.githubusercontent.com/67737432/127734480-7e1b9814-17bb-4137-93c3-7550d801a6d0.png)
   

4. `User` 생성자 함수로 새로운 객체 `admin`을 추가하면, `admin` 에서는 `__proto__`라는 접근자 프로퍼티를 통해 `User.prototype`에 접근할 수 있다. 즉,  `__proto__` 프로퍼티도 `User.prototype` 프로토타입 객체를 가리킨다. 

    ![image](https://user-images.githubusercontent.com/67737432/127734517-d443e83c-8609-45a5-b359-486c396d4413.png)

   

5. 만약 `admin`에 없는 메소드나 프로퍼티를 호출한 경우, 프로토타입 객체에 찾는 값이 있는지 찾아본다.

   ```js
   console.log(admin.sayHi());	// sayHi를 실행함
   ```

---

