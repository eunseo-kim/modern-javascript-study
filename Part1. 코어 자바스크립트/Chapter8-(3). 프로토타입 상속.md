# 8-(3). 프로토타입 상속✨

> 이미 상속에 대해 어느정도 이해했지만 추가적으로 몇 개의 예시를 더 보면...



## 🔍프로토타입 상속에서 this는 언제 결정될까?

> **메서드를 객체에서 호출했든 프로토타입에서 호출했든 상관없이 `this`는 언제나 `.` 앞에 있는 객체가 된다.**

- 코드로 이해해보자.

  ```js
  let animal = {
    sleep() {
      this.isSleeping = true;	// 🤔이 this는 과연 언제 결정될까?????🤔
    },
  };
  
  let rabbit = {
    __proto__: animal,	// 이제 animal은 rabbit의 프로토타입(객체)이다.
  };
  
  // rabbit의 프로퍼티 isSleeping을 true로 변경한다.
  rabbit.sleep();
  console.log(rabbit.isSleeping); // true
  
  //프로토타입(animal)에는 isSleeping이라는 프로퍼티가 없다.
  console.log(animal.isSleeping); // undefined 
  ```

- 최종적으로 객체와 프로토타입은 이런 상태가 된 것이다!!

  프로토타입(animal)에 없는 `isSleeping`을 불렀으니 당연히 undefined가 나온다!
  <img src="C:\Users\eunse\AppData\Roaming\Typora\typora-user-images\image-20210730011605813.png" alt="image-20210730011605813" style="zoom:50%;" />



> #### 👏🏻따라서 중요한 것은,
>
> - 상속받은 메서드를 사용하더라도 객체는 프로토타입이 아닌 자신의 상태를 수정한다.
> - 즉! **메서드는 공유**되지만, ✨**객체의 상태는 공유되지 않는다!!!!!**✨

---



## 🔍`for...in` 반복문으로 객체를 순회하면...?

- 상속된 프로퍼티를 순회 대상에 포함시키는 방법 => 그냥 일반`for... in` 반복문
- 상속된 프로퍼티를 순회 대상에 포함시키지 않는 방법 =>  `obj.hasOwnProperty(key)`을 사용한다. 
  (`obj.hasOwnProperty(key)` → 상속된 프로퍼티가 아니고 직접 구현되어 있는 프로퍼티일 때 `true`를 반환함)

```js
let animal = {
  eats: true,
};

let rabbit = {
  jumps: true,
  __proto__: animal,
};

for (let prop in rabbit) {
  let isOwn = rabbit.hasOwnProperty(prop); // 직접 구현되어있는 프로퍼티일 때 true 반환

  if (isOwn) {
    console.log(`객체 자신의 프로퍼티: ${prop}`); // 객체 자신의 프로퍼티: jumps
  } else {
    console.log(`상속 프로퍼티: ${prop}`); // 상속 프로퍼티: eats
  }
}
```

---



## 🔍프로토타입 체인

>  자바스크립트는 객체의 프로퍼티(메서드 포함)에 접근하려고 할 때 해당 객체에 접근하려는 프로퍼티가 없다면, `[[Prototype]]` 참조를 따라 자신의 부모 역할을 하는 프로토타입의 프로퍼티를 순차적으로 검색한다.
> 이를 **프로토타입 체인**이라고 한다! 프로토타입 체인은 자바스크립트가 객체 지향 프로그래밍의 상속을 구현하는 메커니즘이다.

 <img src="C:\Users\eunse\AppData\Roaming\Typora\typora-user-images\image-20210730021738549.png" alt="image-20210730021738549" style="zoom:50%;" />

- **프로토타입 체인의 최상위에 위치하는 객체는 언제나 `Object.prototype`** 이다. 따라서 이를 **프로토타입 체인의 종점**이라고 한다.

---



##  ✍🏻함께 풀어볼만한 과제 문제...

> #### 왜 햄스터 두 마리의 배는 꽉 찰까요?🐹🐹

```js
let hamster = {
  stomach: [],

  eat(food) {
    this.stomach.push(food);
  }
};

let speedy = {
  __proto__: hamster
};

let lazy = {
  __proto__: hamster
};

// 햄스터 한 마리가 음식을 찾았습니다.
speedy.eat("apple");
alert( speedy.stomach ); // apple

// 🤯🤯이 햄스터도 같은 음식을 가지고 있습니다. 왜 그럴까요? 고쳐주세요.
alert( lazy.stomach ); // apple
```

> ### 🤷🏻‍♀️왜 lazy도 음식을 가지고 있을까?
>
> 1. `speedy.eat("apple");` → 프로토타입 hamster에서 발견 → `this.stomach.push(food);`에서 this는 speedy
> 2. `speedy.stomach.push(food);`에서 `speedy.stomach`를 찾아보려는데 없음! 
>    **따라서 다시 프로토타입으로 올라가 hamster의 `stomach`에 음식을 저장함**
> 3. 결국 lazy도 마찬가지로 stomach가 없어서 **프로토타입 hamster의 stomach를 공유하게 되는 상황!!!!**
>
> 



### 💡문제 해결

> 각각의 햄스터에게 `stomach`를 만들어주면 된다!

```js
let hamster = {
  stomach: [],
  eat(food) {
    this.stomach.push(food);
  }
};

let speedy = {
  __proto__: hamster,
  stomach: []
};

let lazy = {
  __proto__: hamster,
  stomach: []
};

// speedy는 음식을 발견합니다.
speedy.eat("apple");
alert( speedy.stomach ); // apple

// lazy의 stomach은 비어있습니다.
alert( lazy.stomach ); // <nothing>
```
- 이렇게 되면 `this.stomach.push(food);`를 수행할 때 speedy가 자신의 stomach에만 음식을 저장할 수 있다!!

---

