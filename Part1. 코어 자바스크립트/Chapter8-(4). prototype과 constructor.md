#  8-(4). prototype과 constructor

>  앞에서 살짝 보긴했지만 다시 한번😉

- ```js
  function User() {}
  ```

  이렇게 (생성자) 함수를 만들면 프로토타입 객체와 함께 `prototype` 프로퍼티와 `constructor` 프로퍼티가 생성된다.

   <img src="C:\Users\eunse\AppData\Roaming\Typora\typora-user-images\image-20210730023640821.png" alt="image-20210730023640821" style="zoom: 50%;" />

  

- 즉, 이런 코드도 성립한다.
  `console.log(User.prototype.constructor === User); // true`

- `User`으로 `admin`이라는 객체를 만들어보자. `admin`에서 `constructor`에 대한 특별한 조작을 하지 않고 사용하면, `User.prototype`을 거쳐서 접근한다.

  ```js
  function User(name) {
    this.name = name;
  }
  
  const admin = new User("admin");
  console.log(admin.constructor === User);	// true (프로토타입을 거쳐서 접근)
  ```
  
   <img src="C:\Users\eunse\AppData\Roaming\Typora\typora-user-images\image-20210731154635076.png" alt="image-20210731154635076" style="zoom:50%;" />




- 이때 중요한 것은, **자바스크립트는 알맞은 `"constructor"` 값을 보장하지 않는다**는 점이다.

  **즉, 만약 함수의 기본 `prototype` 을 다른 객체로 교체해버리면 이 객체엔 `"constructor"`가 없을 것이다.**

  

  > ### 🤔이해해보자!

  프로토타입을 다음과 같이 객체 리터럴로 교체해보자. 그러면 어떻게 될까?

  ```js
  function User(name) {
    this.name = name;
  }
  
  // User.prototype을 새로운 객체로 덮어버렸다. 
  User.prototype = {
    walk: true,
  };
  
  const admin = new User("admin");
  
  console.log(admin.constructor === User); // ✨false✨
  console.log(admin.constructor === Object);	// ✨true✨
  ```

  - **이제는 `admin.constructor === User`이 성립하지 않게 되었다. 왜 그럴까?**

  - 위의 상황을 그림으로 표현해보면 다음과 같다.
    <img src="C:\Users\eunse\AppData\Roaming\Typora\typora-user-images\image-20210731155956654.png" alt="image-20210731155956654" style="zoom:50%;" />

  

  - 따라서 `admin.constructor`을 호출하면 `admin`의 프로토타입인 `User.prototype`에서 `constructor` 프로퍼티를 찾고,
    다시 `User.prototype`의 프로토타입인 `Object.prototype`에서 `constructor`을 찾는 것이다.
  - 즉, `admin.constructor`을 실제로 실행한 것은 `Object.constructor` 이었고, 따라서 `console.log(admin.constructor === Object);`가 true로 출력되었던 것이다~!!😆

  

  만약, 원래대로 기존의 constructor을 유지하려면 prototype에 원하는 프로퍼티를 추가/제거하는 방식으로 구현해야 된다!

  ```js
  function User(name) {
    this.name = name;
  }
  
  // 이렇게 기본으로 만들어진 프로토타입은 건드리지 않으면서.. 프로퍼티를 추가한다.
  User.prototype.walk = true;
  
  const admin = new User("admin");
  
  console.log(admin.constructor === User); // ✨true✨
  ```

  - 또는 직접 `constructor`을 추가해주는 방법도 있다.

  ```js
  function User(name) {
    this.name = name;
  }
  
  // User.prototype을 새로운 객체로 덮어버렸다. 
  User.prototype = {
    constructor: User,
    walk: true,
  };
  
  const admin = new User("admin");
  
  console.log(admin.constructor === User); // ✨true✨
  ```

  

> ### 👏🏻따라서 정리하면!
>
> - 모든 **함수**는 기본적으로 `F.prototype = { constructor : F }`를 가지고 있으므로 
>   함수의 `"constructor"` 프로퍼티를 사용하면 객체의 생성자를 얻을 수 있다!
>
> - 그러나 `constructor` 프로퍼티는 만약 개발자가 프로토타입 객체를 교체해버리면 사라질수도 있다! (값이 보장되지는 않는다)
>
> - 일반 객체는 `prototype` 프로퍼티를 소유하지 않는다. 위의 내용은 모두 ✨**함수**✨에서 적용된다는 점을 기억하자!!

---



## ✍🏻함께 풀어볼만한 과제 문제...

> ### 동일한 생성자 함수로 객체 만들기
>
> - 생성자 함수 를 이용해 만든 임의의 객체 `obj`가 있을 때, 이 `obj`를 이용하여 새로운 객체 `obj2`를 만들고자 합니다.
>
>   ```js
>   let obj2 = new obj.constructor();
>   ```
>
>   (1) 이와 같은 코드를 사용해 객체를 만들 수 있게 해주는 생성자 함수를 작성해보세요.
>   (2) 그리고 위의 코드가 동작하지 않는 예시도 만들어주세요!



### 💡문제 해결

1. `let obj2 = new obj.constructor();`의 코드가 제대로 동작하려면, (생성자 함수는 `Foo`이라고 가정)
   `Foo.prototype.constructor == Foo`이 성립해야 된다.

   ```js
   function Foo() {}
   
   let obj = new Foo();
   let obj2 = new obj.constructor();	// 결국 new Foo(); 와 같다.
   
   console.log(obj2);	// Foo {}
   ```

2. 위의 코드가 동작하지 않으려면 Foo.prototype을 덮어쓰면 된다!

   ```js
   function Foo() {}
   Foo.prototype = {};
   
   let obj = new Foo();
   let obj2 = new obj.constructor();	// 이제 new Foo(); 가 아니다.
   
   console.log(obj2);	// {} => 🤯왜 빈 객체가 출력되는지 알아보자!!
   ```

   - `new obj.constructor()`는 `obj`에서 `constructor`을 찾지 못한다.
     그래서 프로토타입에서 검색을 하게 되는데..
     **`obj`의 프로토타입인 `Foo.prototype`은 `{}`, 빈 객체로 `constructor`이 없다.**
   - 따라서 `Foo`에서도 찾지 못했으므로, **`Foo`의 프로토타입인 `Object.prototype`에서 찾게 된다.**
     (일반 객체의 프로토타입은 `Object.prototype`이다.) 
   - `Object`는 `constructor`이 제대로 있으므로, `Object.prototype.constructor == Object`가 성립란다. 
     **따라서 최종적으로 `let obj2 = new Object();`가 실행된다.**
   - 그런데 `Object`의 생성자는 인수를 무시하고 항~상 **<u>빈 객체</u>**를 생성한다.
   - 따라서 **`obj2`를 출력해보면 `{}` 빈 객체가 출력**되는 것이다!

