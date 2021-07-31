# 8-(5) 네이티브 프로토타입

## 🔍Object.prototype

> 이전에 잠깐 나온 `Object.prototype`을 더 알아보자!!✨

```js
let obj = { x: 1 };
alert(obj); // "[object Object]" ?
```

- 왜 `[object Object]`가 출력되었을까?

- alert 함수를 실행하면 자동으로 `toString`이 실행된다. 그런데 obj에는 toString이 없다. 따라서 obj의 프로토타입에서 찾게 되는데....

- 객체 리터럴(`{...}`) 방식으로 (또는 `new Object()`를 통해) 생성된 **객체의 프로토타입은 `Object.prototype` 이다**!
  
  따라서 내장 객체 생성자 함수인 `Object`와 객체 `obj` 사이의 관계를 그림으로 나타내면...
  <img src="C:\Users\eunse\AppData\Roaming\Typora\typora-user-images\image-20210730222716609.png" alt="image-20210730222716609" style="zoom:18%;" />
  
- `Object.prototype.toString()`은 기본적으로 재정의하여 사용하지 않는다면 `[object type]`를 반환한다. 
  obj는 type이 Object(객체) 이므로 `[object Object]`를 반환한 것!
  
- \+ 이러한 코드가 모두 `true`로 평가되는 것도 어렵지 않게 이해할 수 있다!

  ```js
  let obj = {};
  
  alert(obj.__proto__ === Object.prototype); // true
  
  alert(obj.toString === obj.__proto__.toString); //true
  alert(obj.toString === Object.prototype.toString); //true
  ```

> ### 주의할 것은!
>
> 이전의 **프로토타입 체인**에서 설명 했듯이
> **`Object.prototype` 위의 체인엔 `[[Prototype]]`이 없다는 점**을 주의하자!
>
> 💡 복습 → **프로토타입 체인의 최상위에 위치하는 객체는 언제나 `Object.prototype`(프로토타입 체인의 종점) 이다.**



### 정리해보면...

|    <u>*객체 생성 방식*</u>     |               객체 리터럴 `{...}`로 만든 객체                | `Object` 생성자 함수로 만든 객체 |                   생성자 함수로 만든 객체                    |
| :----------------------------: | :----------------------------------------------------------: | :------------------------------: | :----------------------------------------------------------: |
| <u>***객체의 프로토타입***</u> |                      `Object.prototype`                      |        `Object.prototype`        | 생성자 함수의 prototype 프로퍼티에 <br/>바인딩 되어있는 객체 |
|                                | <img src="C:\Users\eunse\AppData\Roaming\Typora\typora-user-images\image-20210730222716609.png" alt="image-20210730222716609" style="zoom:15%;" /> |        객체 리터럴과 동일        | <img src="C:\Users\eunse\AppData\Roaming\Typora\typora-user-images\image-20210730224859636.png" alt="image-20210730224859636" style="zoom:25%;" /> |



---



## 🔍여러 내장 객체들과 프로토타입

- `Array`, `Date`, `Function`을 비롯한 내장 객체들 역시 프로토타입에 메서드를 저장한다!
- 모든 내장 프로토타입의 꼭대기엔 `Object.prototype`이 있다.

<img src="C:\Users\eunse\AppData\Roaming\Typora\typora-user-images\image-20210730225126168.png" alt="image-20210730225126168" style="zoom:50%;" />

> ### 🤷🏻‍♀️ Number.prototype?
>
> 원시값(문자열, 숫자, 불린값)은 객체가 아닌데?!🤯🤯🤯
>
> - 원시값들의 프로퍼티에 접근하려고 하면 내장 생성자 `String`, `Number`, `Boolean`을 사용하는 **<u>임시 래퍼(wrapper) 객체</u>**가 생성된다!
> - 각 자료형에 해당하는 래퍼 객체의 메서드를 프로토타입 안에 구현해 놓고 `String.prototype`, `Number.prototype`, `Boolean.prototype`을 사용해 쓸 수 있도록 규정한다.
> - (예시) number은 객체가 아니지만 **래퍼 객체**를 통해 객체처럼 메서드를 사용할 수 있다!
>   <img src="C:\Users\eunse\AppData\Roaming\Typora\typora-user-images\image-20210730231509854.png" alt="image-20210730231509854" style="zoom:70%;" />



> ### ⁉네이티브 프로토타입에 새로운 내장 메서드를 추가하면 편하지 않을까?
> 
> - ✋🏻❌프로토타입은 전역으로 영향을 미치기 때문에 프로토타입을 조작하면 충돌이 날 가능성이 높다! 
  따라서 네이티브 프로토타입을 수정하는 것은 추천하지 않는다고 한다~





## 🔍네이티브 프로토타입에서 메소드 빌려오기

### 💁🏻‍♂️이 기능은 언제 필요할까?

- (예시) `obj`라는 객체를 만들었는데 이 객체를 배열처럼 처리해서 Array의 `join` 메소드를 사용하고 싶다.
  
   ![image-20210730232612802](C:\Users\eunse\AppData\Roaming\Typora\typora-user-images\image-20210730232612802.png)

  

### 👏🏻해결 방법

- 이럴때 사용하는게 바로 **네이티브 프로토타입에서 메소드 빌려오기**이다!
- 내장 메서드 `join`의 내부 알고리즘은 제대로 된 인덱스가 있는지와 `length` 프로퍼티가 있는지만 확인하기 때문에
  **`obj`에 `length`와 `인덱스`만 있다면 원하는 결과를 출력**할 수 있다!

- ```js
  let obj = {
    0: "Hello",
    1: "world!",
    length: 2,
  };
  
  obj.join = Array.prototype.join;
  
  console.log(obj.join(",")); // Hello,world!
  ```

