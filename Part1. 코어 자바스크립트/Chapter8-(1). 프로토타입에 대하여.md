# 8-(1). 프로토타입에 대하여

> *자바스크립트는 대표적인 **프로토타입 기반**의 객체지향 언어이다*



## 💡 먼저, 프로토타입 상속이 왜 필요할까?🤷🏻‍♀️

>  사람에 관한 프로퍼티와 메서드를 가진 `user`라는 객체가 있는데, `user`와 상당히 유사하지만 약간의 차이가 있는 `admin`과 `guest` 객체를 만들어야 한다고 가정해 봅시다. 이때 "**`user`의 메서드를 복사하거나 다시 구현하지 않고 `user`에 약간의 기능을 얹어 `admin`과 `guest` 객체를 만들 수 있지 않을까?**"라는 생각이 들 겁니다. 자바스크립트 언어의 고유 기능인 ***프로토타입 상속***(prototypal inheritance) 을 이용하면 위와 같은 생각을 실현할 수 있습니다.

### 🤔 무슨 말이지? 이해해보자!

- ```js
  function User(name) {
    this.name = name;
    this.sayHi = function () {
      return "hello, " + this.name;
    };
  }
  // admin 객체 만듦
  const admin = new User("admin");
  // guest 객체 만듦
  const guest = new User("guest");
  
  console.log(admin.sayHi === guest.sayHi); // ❌false❌
  console.log(admin.sayHi()); // hello, admin
  console.log(guest.sayHi()); // hello, guest
  ```

- 위의 경우에서 User 생성자 함수는 **인스턴스를 생성할 때마다 sayHi 메서드를 중복 생성하고 모든 인스턴스가 중복 소유**하고 있다!

- 그림으로 나타내보면...
  

---

- 그런데 우리가 바라는 모습은...
  ![image-20210731162529261](C:\Users\eunse\AppData\Roaming\Typora\typora-user-images\image-20210731162529261.png)

---

- **프로토타입 기반 상속**이 무엇인지 알았으니 코드로 구현해보자!

  ```js
  function User(name) {
    this.name = name;
  }
  
  // User 생성자 함수가 생성한 모~든 인스턴스가
  // sayHi 메소드를 *공유*해서 사용할 수 있도록 프로토타입에 추가한다.
  User.prototype.sayHi = function() {
      return "Hello, " + this.name;
  }
  
  // admin 인스턴스 생성
  const admin = new User("admin");
  // guest 인스턴스 생성
  const guest = new User("guest");
  
  // admin과 guest 모두 프로토타입 User.prototype으로부터 sayHi 메소드를 *상속* 받는다.
  console.log(admin.sayHi === guest.sayHi); // ✨true✨
  
  console.log(admin.sayHi()); // hello, admin
  console.log(guest.sayHi()); // hello, guest
  ```

  

> ### 💡상속은 코드의 재사용 관점에서 매우 유용하다!
>
> - 모든 인스턴스가 **공통으로 사용할 프로퍼티, 메서드를 프로토타입에 미리 구현**해 놓으면 
>   **생성자 함수가 생성할 모~든 인스턴스는 별도의 구현 없이 상위 객체인 프로토타입의 자산을 공유**하여 사용할 수 있다!

---

