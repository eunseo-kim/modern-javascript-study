# 8-(6). 프로토타입 메서드와 `__proto__`가 없는 객체

> 8-(2)에서 `__proto__`에 대해 설명할 때, `__proto__`로 프로토타입 객체에 접근하는 방식은 **구식**이라고 했었다!🤭
>
> ![image-20210731001250636](C:\Users\eunse\Desktop\Javascript 스터디 발표 자료\image-20210731001250636.png)
>
> 👎🏻MDN에서도 이렇게 `__ proto__`를 '더 이상 사용하지 말라'고 명시해놓았다!



## 🔍그렇다면 왜 `__proto__`를 사용하지 말라는 걸까?

> 모든 객체가 `__proto__` 프로퍼티를 사용할 수 있는 것이 아니기 때문이다! 



#### 🤔무슨말이지? 이해해보자!

- 우선, `__proto__`는 **객체의 프로퍼티가 아니라 `Object.prototype`의 접근자 프로퍼티**이다.

- 예를 들어 이렇게 객체를 생성해보자.

  ```js
  let obj = {};
  ```

- 그럼 다음과 같은 프로토타입이 형성될 것이다.
  ![image-20210731001928690](C:\Users\eunse\AppData\Roaming\Typora\typora-user-images\image-20210731001928690.png)

- 위의 사진에서 주의할 것은, `__proto__`가 `obj`의 프로퍼티가 아닌,
  `Object.prototype`의 접근자 프로퍼티라는 것이다!

- 따라서 `obj__proto__`를 호출하면 프로토타입인 `Object.prototype`에서 `__proto__`를 찾아 사용하는 것이다~!
  이런점에서 `[[Prototype]]`에 간접적으로 접근할 수 있다고 말하는 것이다!!

- 🤔그런데...🤔
  **<u>만약 `obj`가 Object.prototype을 상속받지 않는다면?</u>**

- ```js
  let obj = Object.create(null);
  ```

  `Object.create(prototype)` 메서드는 **명시적으로 프로토타입을 개발자가 지정하여 객체를 생성**한다.
  즉, 프로토타입을 `null`로 설정한다면 obj는 **프로토타입 체인의 종점**이므로 `Object.prototype`을 상속받지 않는다.

- 다시말해 이때의 obj는 `__proto__` 접근 프로퍼티를 사용할 수 없다!!!!!!!!!!!!!!!!!!!
  ![image-20210731003221226](C:\Users\eunse\AppData\Roaming\Typora\typora-user-images\image-20210731003221226.png)



### 👏🏻따라서!

> 요즘에는 `__proto__` 대신 이런 방법을 권장한다고 한다 :D

- `Object.getPrototypeOf(obj)` : obj의 프로토타입 참조를 취득
- `Object.setPrototypeOf(obj, proto)` : obj의 프로토타입 참조를 proto로 교체

```js
const parent = { x: 1 };
const obj = {};

// obj.__proto__ = parent;	// 사용하지 않습니다!

// obj 객체의 프로토타입을 취득
Object.getPrototypeOf(obj); // obj.__proto__; 와 같습니다.

// obj 객체의 프로토타입을 교체
Object.setPrototypeOf(obj, parent); // obj.__proto__ = parent; 와 같습니다.

console.log(obj.x); // 1
```

