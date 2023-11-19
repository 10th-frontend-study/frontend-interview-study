# 배경: 배열을 순회해 보자

## 🚀 퀴즈

- 1부터 5까지 숫자가 하나씩 담겨있는 `arr` 배열이 있다.
- 우리는 `arr` 의 각 요소에 2를 곱할 것이다.
- 이때 배열의 내장 메소드인 `map`과 `forEach`를 사용해보자 !

```jsx
const arr = [1, 2, 3, 4, 5];
```

### map으로 수행하는 방법

```jsx
const arr = [1, 2, 3, 4, 5];

const newArr = arr.map((item) => {
  return item * 2;
});
```

### forEach로 수행하는 방법

```jsx
const arr = [1, 2, 3, 4, 5];

const newArr = arr.forEach((item) => {
  return item * 2;
});
```

- `map`과 `forEach` 각각의 방법으로 수행했을 때 출력 결과를 생각해보자.
- 정답

  - `map`의 출력 결과

  ```jsx
  const arr = [1, 2, 3, 4, 5];
  const newArr = arr.map((item) => {
    return item * 2;
  });

  // 출력 결과
  console.log(newArr); // [ 2, 4, 6, 8, 10 ]
  ```

  (참고: return을 생략해서 사용하는 법)

  ```jsx
  const arr = [1, 2, 3, 4, 5];
  const newArr = arr.map((item) => item * 2);

  console.log(newArr); // [ 2, 4, 6, 8, 10 ]
  ```

  - `forEach`의 출력 결과

  ```jsx
  const arr = [1, 2, 3, 4, 5];
  const newArr = arr.forEach((item) => {
    return item * 2;
  });

  // 출력 결과
  console.log(newArr); // undefined
  ```

  (참고: return을 생략해서 사용하는 법)

  ```jsx
  const arr = [1, 2, 3, 4, 5];
  const newArr = arr.forEach((item) => item * 2);

  console.log(newArr); // undefined
  ```

그렇다면 `forEach()`는 어떻게 작성해야 정상적인 결과를 얻을 수 있을까

```jsx
const arr = [1, 2, 3, 4, 5];
const newArr = [];

arr.forEach((item) => newArr.push(item * 2));

console.log(newArr); // [ 2, 4, 6, 8, 10 ]
```

이렇게 별도로 `push` 메소드를 사용해서 새 배열에 입력해 주어야 한다.

그 이유는 아래에서 `map`과 `forEach`의 차이를 보면서 이해하자.

<br/>
<br/>

# Map

### map 이란?

- `map` 메서드는 자신을 호출한 배열의 모든 요소를 순회하면서 인수로 전달받은 콜백 함수를 반복 호출한다.
  - 여기까지는 `forEach`와 동일하다
- 그리고 콜백 함수의 반환값들로 구성된 새로운 배열을 반환한다!
  - `forEach`와 가장 큰 차이점

### 직접 사용해 보자

```jsx
const arr = [1, 2, 3, 4, 5];
const newArr = arr.map((item) => item * 2);
```

- 이때 원본 배열은 변경하지 않는다.
- `map` 메서드에서는 원본 배열의 원소와 일대일 mapping이 진행된다.
  - 배열의 길이는 항상 같다.

```jsx
console.log(arr); // 원본 배열 arr: [1, 2, 3, 4, 5];
console.log(newArr); // 새 배열 newArr: [ 2, 4, 6, 8, 10 ]
```

<br/>
<br/>

# forEach

### forEach 란?

- `for문`을 대체할 수 있는 함수다.
- `forEach` 메서드는 자신의 내부에서 반복문을 실행한다.
- 즉, 내부에서 반복문을 통해 자신을 호출한 배열을 순회하면서 수행해야할 처리를 콜백 함수로 전달받아 반복 호출한다.

### 직접 사용해 보자

```jsx
const arr = [1, 2, 3, 4, 5];
const newArr = [];

arr.forEach((item) => newArr.push(item * 2));
```

- 이때 원본 배열은 변경하지 않는다.
- `forEach` 메서드의 반환값은 언제나 `undefined` 이다.

```jsx
console.log(arr); // 원본배열 arr: [1, 2, 3, 4, 5];
console.log(newArr); // 새 배열 newARr: [ 2, 4, 6, 8, 10 ]
```

<br/>
<br/>

# 결론

- `forEach` 메서드는 단순히 **반복문을 대체하기 위한 함수**이고,
- `map` 메서드는 요소값을 다른 값으로 mapping한 **새로운 배열을 생성하기 위한** 고차함수이다.
