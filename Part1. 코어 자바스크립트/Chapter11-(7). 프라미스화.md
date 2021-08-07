# Chaper11-(7). 프라미스화

## 🔍프라미스화?

- **콜백을 받는 함수**를 **<u>프라미스를 반환하는 함수로 바꾸는 것</u>**을 '프라미스화(promisification)'라고 한다.

- 어떻게? => `promisify()` 메서드를 이용하면 된다.
- 따라서 `callback`함수를 `new Promise`로 감싸주지 않아도 `promisify()`를 통해 간단히 `promise`로 사용할 수 있다!



...🤯

