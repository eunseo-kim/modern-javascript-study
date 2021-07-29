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
  function User() {}
  const admin = new User();
  console.log(admin.constructor === User);	// true (프로토타입을 거쳐서 접근)
  ```

   <img src="C:\Users\eunse\AppData\Roaming\Typora\typora-user-images\image-20210730024640853.png" alt="image-20210730024640853" style="zoom:50%;" />




- 중요한 것은, **자바스크립트는 알맞은 `"constructor"` 값을 보장하지 않는다**는 점이다.
  만약 함수의 기본 `"prototype"` 값을 다른 객체로 바꾸면 이 객체엔 `"constructor"`가 없을 것이다!

  ```js
  function User() {}
  User.prototype = {
    walk: true,
  };
  let admin = new User();
  console.log(admin.constructor === User); // false
  ```

  따라서 원래대로 기존의 constructor을 유지하려면 prototype에 원하는 프로퍼티를 추가/제거하는 방식으로 구현해야 된다!!

  ```js
  function User() {}
  User.prototype.walk = true;	// 이 부분을 수정!
  
  let admin = new User();
  console.log(admin.constructor === User); // true
  ```

  

---

> ### 👏🏻따라서 정리하면!
>
> 모든 **함수**는 기본적으로 `F.prototype = { constructor : F }`를 가지고 있으므로 
> 함수의 `"constructor"` 프로퍼티를 사용하면 객체의 생성자를 얻을 수 있다!
>
> 그러나 일반 객체는 `prototype` 프로퍼티를 소유하지 않는다. 위의 내용은 모두 ✨**함수**✨에서 적용된다는 점을 기억하자!!

